# Building New P8e Contracts

## P8e Contract Creation

When preparing to create contracts, information about the transaction must be identified. This information includes the participants involved, the role of each participant \(i.e. who initiates the contract\), the input and output data formats, and the requirements of the contract \(i.e. validation requirements\).

Every contract is required to extend the `P8eContract` abstract class. This class includes common functionality used by many contracts. This also facilitates identifying all contracts existing in the system.

Only the hashed results of contract execution are memorialized to the blockchain. The only way to verify the hashed results originated from a given set of information is to run it through the contract and compare the hashed results. This validation process relies on all contracts to be deterministic, meaning the same inputs to to a contract are required to produce the same hashed results. For this reason, generating timestamps or random UUIDs in contracts should be avoided. Instead, this functionality should be handled outside of the contract and be passed in as an input parameter to the contract. The same applies for other things that affect determinism, such as floats, map ordering, and external calls.

Parameters required by contracts that have previously been saved to the blockchain are class parameters with the Record annotation. Records are used when modifying previously saved data or when new data needs to reference existing data. Record names are required to be unique for each contract.

```kotlin
class <ContractClass>(@Record(name = <Name>) name: <Protobuf>): P8eContract()
```

Participants involved in transactions are identified by providing a list of PartyType to the Participants annotation. PartyType values are predefined in Provenance and include owners, originators, custodians, servicers, etc. All participants must be included in the Participants annotation in order to participate in the transaction. The participants included all need to agree on the contract before it will be saved to the blockchain.

```kotlin
@Participants(roles = [<PartyType>, <PartyType>])
```

Contracts can be comprised of many functions. The participant responsible for submitting the results to the blockchain is required for each function and is identified using the Function annotation. The responsible participant compiles the hashed execution results from all participants and sends the information to Provenance where the nodes endorse and memorialize the transaction to the blockchain. This functionality is provided by the SDK. All functions with the Function annotation must be executed before the output will be memorialized to the blockchain.

```kotlin
@Function(invokedBy = <PartyTypeInitiatingTransaction>)
```

The hashed execution results of each function with a Function annotation are saved to the blockchain with a label identified by the Record annotation. Record names are required to be unique for each contract.

```kotlin
@Record(name = <BlockchainDataLabel>)
```

Parameters to functions are required to be annotated with either the Input or the Record annotation. Proposed parameters are those that have not yet been saved to the blockchain. As previously explained, records have already been memorialized to the blockchain. Input names are required to be unique for each contract.

```kotlin
fun name(@Input(name = <InputDataName>) name: <Protobuf> )
```

## Contract Inheritance

Contracts can also inherit from other contracts, provided the base contract still extends the P8eContract abstract class. The following example demonstrates how a contract extends the HelloWorld contract.

```kotlin
@Participants(roles = [OWNER])
open class HelloWorldDerivedContract(): HelloWorldContract() {
    @Function(invokedBy = OWNER)
    @Record(name = "address")
    open fun address(@Input(name = "address") address: ExampleAddress ) = address
}
```

When this contract is executed, name is required to be supplied to fulfill the name function in the superclass. Address is also required to be supplied to fulfill the address function in the subclass.

## Contract Bootstrapping

Once contracts are defined like was shown above, there's a bootstrapping phase that's necessary before the contracts can be executed to record data to Provenance. A [gradle plugin](https://github.com/provenance-io/p8e-gradle-plugin) is provided that is capable of bootstrapping JVM based JARs that contain Java or Kotlin based P8eContract implementations.

The plugin exposes mutiple tasks that can be used to check contract syntax, persist the contract specifications and JARs to Object Store, and memorializing the scope and contract specifications to Provenance. That uberjar \(fully packaged Java JAR, including all dependencies\) is saved to Object Store so that the exact uberjar has the ability to be loaded into the Java memory class loader and executed by all contract participants.

This is the first point at which a Provenance account would be needed. Due to transaction sequencing, it is **highly** recommended that a separate, standalone, Provenance account is used strictly for use with the plugin for bootstrapping. Any participants that need to have access to the contract specifications or JARs can be included as participants in the plugin configuration.

