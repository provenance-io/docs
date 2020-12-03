# Multi-Contract Example

The multi-step contract example explained how a single contract can be used to orchestrate multiple steps. This section explains how multiple contracts can be used to accomplish the same functionality.

## Contracts

The multi-step contract contained separate functions for name and address. For this example, a contract has been created for each of those two methods: HelloWorldMultiContractName and HelloWorldMultiContractAddress \(HelloWorld.kt and HelloWorldJavaContracts.java in the p8e-contract project\). Similar to the multi-step example, an owner is first submitting name information. The second step again consists of address information being added to the transaction by a custodian but this time to a new contract. The order of the steps is again enforced by the Fact annotation but this time on the second contract. The hashed results of each contract will be memorialized to the blockchain once all participants involved have successfully executed the contracts.

```kotlin
@Participants(roles = [OWNER, CUSTODIAN])
open class HelloWorldMultiContractName() : P8eContract() {
    @Function(invokedBy = OWNER)
    @Fact(name = "name")
    open fun name(@Input(name = "name") name: ExampleName ) = name
}

@Participants(roles = [OWNER, CUSTODIAN])
open class HelloWorldMultiContractAddress(@Fact(name = "name") val name: ExampleName) : P8eContract() {
    @Function(invokedBy = CUSTODIAN)
    @Fact(name = "address")
    open fun address(@Input(name = "address") address: ExampleAddress ) = address
}
```

## Contract Watchers

Similar to the multi-step example, each participant is required to configure watchers to listen for messages from each contract.

```kotlin
contractManager.watchBuilder(HelloWorldMultiContractName::class.java)
    .executeRequests()
    .watch()

contractManager.watchBuilder(HelloWorldMultiContractAddress::class.java)
    .executeRequests()
    .watch()
```

## Contract Execution

Once the watchers are configured and started the contracts are ready to be executed. The owner initiates the contract the same way as the multi-step contract is executed.

```kotlin
contractManager.newContract(HelloWorldMultiContractName::class.java, OWNER).apply {
    satisfyParticipant(CUSTODIAN, <custodian's public key>)
    addProposedFact("name", ExampleName.newBuilder()
                                       .setFirstName("Hello")
                                       .setLastName("World")
                                       .build())
    contractManager.execute(this)
}
```

Executing this contract will trigger the request watcher for the custodian. Once the custodian executes the contract, it will trigger the response watchers for the owner and custodian. At this point the hashed results of the first contract will be memorialized to the blockchain. The first part of the transaction is complete and waiting for the custodian to initiate the next step by executing the second contract.

The second contract requires inputting a fact from the first contract. This is accomplished by using the scope UUID from the first contract to load the contract and include it when the contract is instantiated. Address information is also added to the contract before it is executed.

```kotlin
val wrapper = contractManager.indexClient.findLatestScopeByUuid(<UUID>)
contractManager.newContract(HelloWorldMultiContractAddress::class.java, wrapper?.scope, CUSTODIAN).apply {
    satisfyParticipant(OWNER, <owner's public key>)
    addProposedFact("address", ExampleAddress.newBuilder()
                                             .setStreet("222 N Main")
                                             .setCity("Helena")
                                             .setState("MT")
                                             .setZip("59601")
                                             .build())
    contractManager.execute(this)
}
```

Executing this contract will trigger the request watchers for the owner. Once the owner executes the contract, it will trigger the response watchers. At this point the hashed results of the second contract will be memorialized to the blockchain and the transaction is complete.

