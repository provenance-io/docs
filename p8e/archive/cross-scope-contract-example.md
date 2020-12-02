# Cross Scope Contract Example

Cross scope contract refers to a contract inputting information that was output, and ultimately memorialized to the blockchain, by the execution of another contract.

## Cross Scope Contract

The HelloWorldCrossScopeContract \(HelloWorld.kt and HelloWorldJavaContracts.java in the p8e-contract project\) is an example of a cross scope contract. Input to this contract is name and phone information. The name information is required to have been memorialized to the blockchain, as designated by the Fact annotation. In this example name information was output from when the HelloWorldContract was executed. Phone information is input when the contract is initiated by an owner.

```kotlin
@Participants(roles = [OWNER])
class HelloWorldCrossScopeContract(@Fact(name = "name") val currentName: ExampleName) : P8eContract() {
    @Function(invokedBy = OWNER)
    @Fact(name = "phone")
    fun phone(@Input(name = "phone") phone: ExamplePhone) = phone
}
```

## Cross Scope Contract Execution

An owner initiates this contract similar to previous examples but with the addition of using the setFact function to load the required fact from a previous scope. This function requires two parameters; a ScopeFact, which consists of the scope UUID and the name of the fact from the previous output, and the name of how the fact will be referenced in the existing contract.

```kotlin
contractManager.newContract(HelloWorldCrossScopeContract::class.java, OWNER).apply {
    setFact(Contract.ScopeFact(<UUID>, "name"), "name")
    addProposedFact("phone",ExamplePhone.newBuilder()
                                        .setNumber("<phone number>")
                                        .build())
    contractManager.execute(this)
}
```

Similar to all contracts, executing the contract will trigger the appropriate watchers that have been configured and started.

