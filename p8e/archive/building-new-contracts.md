# Building New Contracts

## Download the Repository

Contracts and the data formats they use are located in the p8e-contract repository. To contribute to this repository by creating new contracts, you need to download a copy of the GitHub repository. Provenance will grant you access to this repository when you provide your GitHub account information.

```text
git clone https://github.com/FigureTechnologies/p8e-contract.git
```

## Contract Creation

When preparing to create contracts, information about the transaction must be identified. This information includes the participants involved, the role of each participant \(i.e. who initiates the contract\), the input and output data formats, and the requirements of the contract \(i.e. validation requirements\).

Every contract is required to extend the P8eContract abstract class. This class includes common functionality used by many contracts. This also facilitates identifying all contracts existing in the system.

Only the hashed results of contract execution are memorialized to the blockchain. The only way to verify the hashed results originated from a given set of information is to run it through the contract and compare the hashed results. This validation process relies on all contracts to be deterministic, meaning the same inputs to to a contract are required to produce the same hashed results. For this reason, generating timestamps or random UUIDs in contracts should be avoided. Instead, this functionality should be handled outside of the contract and be passed in as an input parameter to the contract. The same applies for other things that affect determinism, such as floats, map ordering, and external calls.

Parameters required by contracts that have previously been saved to the blockchain are class parameters with the Fact annotation. Facts are used when modifying previously saved data or when new data needs to reference existing data. Fact names are required to be unique for each contract.

```kotlin
class <ContractClass>(@Fact(name = <Name>) name: <Protobuf>): P8eContract()
```

Participants involved in transactions are identified by providing a list of PartyType to the Participants annotation. PartyType values are predefined in Provenance and include owners, originators, custodians, servicers, etc. All participants must be included in the Participants annotation in order to participate in the transaction. The participants included all need to agree on the contract before it will be saved to the blockchain.

```kotlin
@Participants(roles = [<PartyType>, <PartyType>])
```

Contracts can be comprised of many functions. The participant responsible for submitting the results to the blockchain is required for each function and is identified using the Function annotation. The responsible participant compiles the hashed execution results from all participants and sends the information to Provenance where the nodes endorse and memorialize the transaction to the blockchain. This functionality is provided by the SDK. All functions with the Function annotation must be executed before the output will be memorialized to the blockchain.

```kotlin
@Function(invokedBy = <PartyTypeInitiatingTransaction>)
```

The hashed execution results of each function with a Function annotation are saved to the blockchain with a label identified by the Fact annotation. Fact names are required to be unique for each contract.

```kotlin
@Fact(name = <BlockchainDataLabel>)
```

Parameters to functions are required to be annotated with either the Input or the Fact annotation. Proposed parameters are those that have not yet been saved to the blockchain. As previously explained, facts have already been memorialized to the blockchain. Input names are required to be unique for each contract.

```kotlin
fun name(@Input(name = <InputDataName>) name: <Protobuf> )
```

## Contract Inheritance

Contracts can also inherit from other contracts, provided the base contract still extends the P8eContract abstract class. The following example demonstrates how a contract extends the HelloWorld contract.

```kotlin
@Participants(roles = [OWNER])
open class HelloWorldDerivedContract(): HelloWorldContract() {
    @Function(invokedBy = OWNER)
    @Fact(name = "address")
    open fun address(@Input(name = "address") address: ExampleAddress ) = address
}
```

When this contract is executed, name is required to be supplied to fulfill the name function in the superclass. Address is also required to be supplied to fulfill the address function in the subclass.

## Deploy Contract/Submit for Review

New contracts or changes to existing contracts should be made in a branch of p8e-contract. Once the changes are ready for testing, the branch will be pushed to the GitHub repository. Branches with names beginning with “feature/” will automatically be deployed to the sandbox environment. When the changes are ready for review, a pull request needs to be created.

## Update Dependencies

The following three dependencies are required for processes to execute contracts:

1. p8e-sdk - contains the components required to submit contracts and communicate with the Provenance Protocol and underlying blockchain network.
2. p8e-contract - contains the contracts available for use within Provenance Blockchain. Historical versions of the contracts are kept and can be used by changing the version number.
3. p8e-proto - Provenance uses Protobufs to define input and output to contracts. p8e-proto contains the Protobuf definitions supported by Provenance Blockchain. Similar to contracts, historical versions are kept and can be used by changing the version number.

## Contract Watchers

Before contracts can be executed, watchers have to be configured and running to listen to messages from contract execution. There are four types of watchers to handle contract messages.

* Request - Receive messages when a contract has been initiated and is ready to be executed by all participants.
* Step Completion - Receive messages when a contract, in whole or part, has been executed by all participants and has been memorialized to the blockchain.
* Error - Receive messages when an error occurs during contract execution.
* Disconnect - Receive messages when the watchers have been disconnected from the stream.

Watchers are configured and started using a ContractManager instance.

```kotlin
contractManager.watchBuilder(<SmartContractClass>::class.java).watch()
```

Starting watchers for a given contract manager/contract starts a bidirectional stream to the backend. Events are then sent back and forth on that stream.

Default implementations are provided by the SDK for each type. The defaults simply log a message and acknowledge receipt of the message by returning true from the watcher. The defaults can be overridden by providing a new function when the watcher is configured.

```kotlin
contractManager.watchBuilder(<SmartContractClass>::class.java)
    .request{
        // TODO add request watcher logic

        // Execute the contract
        contractManager.execute(it)

        // Ack that the contract has been received.
        true
    }.stepCompletion {
        // TODO add step completion watcher logic

        // Ack that the message has been received.
        true
    }.error {
        // TODO add error watcher logic

        // Ack that the message has been received.
        true
    }.watch()
```

The default request watcher does not include functionality to execute contracts. This functionality was intentionally omitted to ensure contracts are executed intentionally. When the request watcher is overridden, the functionality must include logic to execute the contract. A helper is also provided to allow contracts to be automatically executed without overriding the request watcher.

```kotlin
contractManager.watchBuilder(<SmartContractClass>::class.java)
    .executeRequests()
    .watch()
```

If the watchers get disconnected, the default watcher simply outputs an error message to the log. If this happens, the watchers need to be restarted. This default watcher can be overridden with custom functionality and a helper is provided to enable reconnecting.

```kotlin
contractManager.watchBuilder(<SmartContractClass>::class.java)
    .disconnect {
        // TODO add disconnect watcher logic

        it.reconnect()
    }.watch()
```

## Contract Execution

Once the new contract has been deployed and the dependencies have been added and refreshed, the SDK methods can be utilized to create a process to execute the new contract. Executing a new contract is similar to how the HelloWorld contract is executed. In order for a participant to initiate transactions, they must create the contract, identify other participants involved, and satisfy any parameters. Other participants involved execute the same contract created by the initiator. By successfully executing the contract, other participants are essentially signing the contract.

The initiator is required to run watchers for contracts they invoke using the same instance of the ContractManager. If a watcher isn’t started for a contract being initiated, an error message will be logged.

To initiate a contract, a ContractManager is created using the primary key of the participant initiating the transaction.

```kotlin
private val contractManager = ContractManager.create("<private_key_text>".toJavaPrivateKey(), "<api_url>")
```

Using the ContractManager, a new contract is created with the class name of the contract.

```kotlin
var contract = contractManager.newContract(<SmartContractClass>::class.java)
```

Creating a new contract with just the contract class will cause a new scope UUID to be generated. The scope UUID can also be set by the affiliate initiating the contract. Provenance validates the UUID to prevent duplicates. If the UUID set by the affiliate has already been used and memorialized to the blockchain, the contract will fail during execution.

```kotlin
var contract = contractManager.newContract(<SmartContractClass>::class.java, <Scope UUID>)
```

Required parameters are defined in the contracts by either the Fact or Input annotations. Fact by definition is a fact that has previously been memorialized to the blockchain. The easiest way to satisfy required Fact parameters is to include the scope in the constructor when new contracts are instantiated. The SDK provides a client where the scope can be retrieved using the scope UUID of when the fact was first saved to the blockchain.

```kotlin
val wrapper = contractManager.indexClient.findLatestScopeByUuid(<UUID>)
var contract = contractManager.newContract(<SmartContractClass>::class.java, wrapper.scope)
```

Proposed facts, identified using the Input annotation, are satisfied using the addProposedFact function of the contract.

```kotlin
contract.addProposedFact(<FactName>, <value>)
```

Using the contract created with the contract class, a parameter for each participant in the transaction is passed in using the satisfyParticipant function.

```kotlin
contract.satisfyParticipant(<PartyType>, "<party’s public key>".toJavaPublicKey())
```

An alternate to identifying the participant for the party initiating the contract is to provide the invoker’s role to the newContract constructor.

```kotlin
var contract = contractManager.newContract(<SmartContractClass>::class.java, <PartyType>)
```

By default contracts will remain in a pending state until all participants involved in the transaction execute the contract. To handle scenarios where an affiliate isn’t listening for a contract or chooses not to execute a contract, an expiration can be set on the contract when it is executed. This is accomplished by sending in the date-time of when the contract should expire into the setExpiration function.

```kotlin
contract.setExpiration(<OffsetDateTime>)
```

Once all the parameters have been set, the contract can be executed.

```kotlin
contractManager.execute(contract)
```

Once the contract has been executed, the contract will be sent to the other participants on the contract to execute and sign the contract before being memorialized to the blockchain.

## Rejecting/Canceling Contracts

Rejecting and canceling contracts are very similar. The only difference is contracts can only be canceled by the affiliate who initiated the transaction. Whereas any party to a transaction can reject the contract.

After a contract has been initiated but before it has completed executing, the contract can be rejected by any party involved in the transaction. Alternatively it can be canceled by the affiliate initiating the transaction. Since all parties involved in a transaction have to execute and agree to the contract, when a contract is rejected/canceled it is moved into an error state and is no longer valid. Helpers for rejecting/canceling contracts are provided in both the ContractManager and Contract classes. They include an optional message parameter that will be included in the error message when provided.

```text
Contract execution was rejected by affiliate <Affiliate UUID>: <Optional rejected message.>
```

If an affiliate other than the initiator attempts to cancel a contract an error will be returned.

```text
Contract can only be canceled by the contract invoker.
```

The primary use case for rejecting contracts is in the request handlers for the affiliates participating in the contract. The contract can be interrogated and if an affiliate determines they don’t agree to the contract, they can reject it instead of executing it.

```kotlin
contractManager.watchBuilder(HelloWorldContract::class.java)
    .request{
        var executeContract = true
        // TODO analyze the contract to see if it should be executed
        // if it shouldn't be, set executeContract flag to false
        if (executeContract) {
            contractManager.execute(it)
        } else {
            it.reject("Optional rejected message.")
        }

        // Ack that the contract has been received.
        true
    }.watch()
```

Contracts can be rejected/canceled via a ContractManager by passing either an Execution UUID or Contract instance to the reject/cancel function.

```kotlin
// cancel contract using Execution UUID
contractManager.cancel(UUID.fromString(executionUuid), "Optional canceled message.")

// cancel contract using Contract instance
val contract = contractManager.loadContract(<ContractClass>, UUID.fromString(executionUuid))
contractManager.cancel(contract, "Optional canceled message.")
```

Contracts can also be rejected/canceled directly through the Contract via the helper functions.

```kotlin
// cancel contract using Contract instance
val contract = contractManager.loadContract(<ContractClass>, UUID.fromString(executionUuid))
contract.cancel("Optional canceled message.")
```

## Contract Statuses

There are many steps involved in contract execution. As contracts progress through this process, a status is updated to allow insight into where the contract is at in the process. Since affiliates participating in a contract can be at different steps in the execution process, a separate status for each affiliate is maintained.

The following list of statuses are contained in an enum in a Protobuf defintion located in io.p8e.proto.ContractScope.Envelope.Status.

```text
CREATED
FRAGMENT
INBOX
EXECUTED
OUTBOX
SIGNED
CHAINCODE
INDEX
COMPLETE
ERROR
```

The enum also contains a brief description for each status. A helper function is provided to retrieve the description for a given status.

```kotlin
Status.CREATED.getDescription()
```

Helper functions are also provided to retrieve the status for a given contract. The ContractManager can be used to retrieve the status based on an Execution UUID or a Contract instance. The status can also be retrieved directly from a Contract instance.

```kotlin
val contract = contractManager.loadContract(<ContractClass>, <Execution UUID>)

//retrieve the status from the ContractManager using the Execution UUID
contractManager.status(<Execution UUID>)

//retrieve the status from the ContractManager using the contract instance
contractManager.status(contract)

//retrieve the status directly from the contract
contract.status()
```

