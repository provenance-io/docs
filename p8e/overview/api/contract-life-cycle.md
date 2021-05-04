# Contract Life Cycle

Contracts are executed through a series of life cycle events that are intercepted by client software. These events \(except for the initial 'Accepted' event\) are handled by passing callback functions or handler objects via the contract watchBuilder as in the following example \(note the return `true` to indicate successful processing of each event\). In cases where `false` is returned, or a networking problem causes the acknowledgment to not reach the p8e backend, the event will be redelivered after a retry delay elapses.

```kotlin
contractManager.watchBuilder(HelloWorldContract::class.java)
        .stepCompletion { contract: Contract<HelloWorldContract> ->
            // handle step completions
            true
        }
        .request { contract: Contract<HelloWorldContract> ->
            // handle incoming contract request
            true
        }
        .error { error: ContractError<HelloWorldContract> ->
            // handle errors
            true
        }.watch()
```

## **Life Cycle Events**

### Accepted

Upon calling the `execute()` method on a contract, the envelope is either initially accepted into the system, or rejected with an error. This is the only 'event' in the lifecycle that happens synchronously, not asynchronously.

The return type of `execute()` is a functional `Either<P8eError, Contract<T>>`and has the typical bifunctor interface. One such example of operating on the result is shown below.

```kotlin
contractManager.newContract(HelloWorldContract::class.java, UUID.randomUUID().toProtoUuidProv()).apply {
    addProposedFact("name", ExampleName.newBuilder()
        .setFirstName("John").setLastName("Doe").build())
    satisfyParticipant(OWNER, publicKey.toJavaPublicKey())
    execute()
        .map { contract: Contract<HelloWorldContract> ->
            // do something with accepted Contract<T> object
        }
        .mapLeft { error: P8eError ->
            // handle the EnvelopeError as needed
        }
}
```

### Fragment

This state only applies to a multi-party contract, and indicates that the contract has been sent to other participants for signing.

### Signed

This state indicates that the contract has been signed by all relevant parties. This is an internal event, it results in no contract watch builder event handlers executing.

### Blockchain

This state indicates that the contract has been sent to the blockchain and is waiting to be committed in a block. This is an internal event, it results in no contract watch builder event handlers executing.

### Index

This state indicates that the contract result is waiting to be indexed in ElasticSearch. This is an internal event, it results in no contract watch builder event handlers executing.

### Complete

This is the state you have been waiting for, the contract has completely executed successfully! This event will fire the stepCompletion event handler.

### Error

As its name would imply, there was an error at some stage of contract execution, this event allows you to see what the error was and handle it appropriately. Any events that have errored will go through a retry process before ultimately erroring the contract completely. 



