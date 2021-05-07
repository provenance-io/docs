---
description: >-
  Before you are able to execute P8e contracts, you must first publish
  specifications to P8e (and ultimately the blockchain). These specifications
  define the ruleset that govern contracts.
---

# Specifications

## Contract Specifications

Contract specifications define all the information for a contract. When a contract is executed, it is validated against the associated specification. The specification includes information such as parties with their roles, inputs/outputs of facts, preconditions, as well as cross scope facts. The following examples can be found [here](https://github.com/provenance-io/p8e-gradle-plugin/tree/main/example-kotlin).

```kotlin
@Participants(roles = [OWNER])
@ScopeSpecification(names = ["io.p8e.contracts.examplekotlin.helloWorld"])
open class HelloWorldContract() : P8eContract() {
    @Function(invokedBy = OWNER)
    @Fact(name = "name")
    open fun name(@Input(name = "name") name: ExampleName) =
        name.toBuilder()
            .setFirstName(name.firstName.plus("-hello"))
            .setLastName(name.lastName.plus("-world"))
            .build()
}
```

**Description of annotations**

* Participants - The party types associated with this specification
* ScopeSpecification - All the scopes, by name, that can execute this contract
* Function - Specifies a function and which party is responsible for running it
* Input - A function parameter
* Fact - The output of the associated function will be saved as this

Contract specifications are dehydrated into a [protobuf](https://github.com/provenance-io/p8e/blob/main/p8e-proto-internal/src/main/proto/p8e/contract_spec.proto) representation. 

## Scope Specifications

```kotlin
@ScopeSpecificationDefinition(
    name = "io.p8e.contracts.examplekotlin.helloWorld",
    description = "A generic scope that allows for a lot of example hello world contracts.",
    partiesInvolved = [OWNER],
)
open class HelloWorldScopeSpecification() : P8eScopeSpecification()

```

Scope specifications serve the following purposes and only act as displayable metadata.

* Validation - When a contract is executed, P8e will validate that the provided contract can operate on the scope as defined by the contract specification and scope specification mappings. The blockchain mimics this same validation. Scope specification parties must be involved on all contracts associated with this scope. This means that in practice the parties involved in the scope specification are often a subset of parties of the encompassing contracts. For instance, if a scope has `OWNER` party on one contract and `[OWNER, ORIGINATOR]` on another contract, the scope specification only only include the `OWNER` party. 
* Display - The [provenance explorer](https://explorer.provenance.io/dashboard) uses the fields of the scope specifications for display purposes.

## Publishing to P8e

Contract and scope specifications are declarative in nature. They are defined and published to P8e out of band from the executable that will be creating and executing contracts. The [p8e-gradle-plugin](https://github.com/provenance-io/p8e-gradle-plugin)  is used to publish these specifications, as well as other supporting tasks \(the [README](https://github.com/provenance-io/p8e-gradle-plugin/blob/main/README.md) is a great source for plugin usage\).

When a contract specification is published, it is dehydrated into a [protobuf](https://github.com/provenance-io/p8e/blob/main/p8e-proto-internal/src/main/proto/p8e/contract_spec.proto) representation. This representation contains a hash of the uberjar that it was contained within. This means that whenever a contract specification directly changes, or a protobuf that the contract specification uses changes, both the hash of the uberjar and the hash of the contract specification change as well. Both the protobuf representation and the uberjar are persisted to the [EOS](../overview/encrypted-object-store/). P8e is able to load any historical state for a scope as it was at the time it was executed because the uberjar and contract specification can be pulled from EOS based on their hashes. P8e uses these same hashes to load the correct uberjar at runtime to execute the functions defined in the contract specification.

