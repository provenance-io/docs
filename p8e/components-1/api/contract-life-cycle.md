# Contract Life Cycle

Contracts are executed through a series of life cycle events that are intercepted by client software. These events \(except for the initial 'Accepted' event\) are handled by passing callback functions or handler objects via the contract watchBuilder as in the following example \(note the return `true` to indicate successful processing of each event\)

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
        .error { error: ContractScope.EnvelopeError ->
            // handle errors
            true
        }
        .disconnect {
            Thread.sleep(10000)
            it.reconnect()
        }.watch()
```

## **Life Cycle Events**

### Accepted

Upon calling the `execute()` method on a contract, the envelope is either initially accepted into the system, or rejected with an error. This is the only 'event' in the lifecycle that happens synchronously, not asynchronously.

You can handle this event with the accepted and error handlers as shown below. These are similar to the event listeners in the watch builder, but will be executed synchronously.

```kotlin
contractManager.newContract(HelloWorldContract::class.java, UUID.randomUUID().toProtoUuidProv()).apply {
    addProposedFact("name", ExampleName.newBuilder()
        .setFirstName("John").setLastName("Doe").build())
    satisfyParticipant(OWNER, publicKey.toJavaPublicKey())
    execute()
        .accepted { contract ->
            // do something with accepted Contract<T> object
            true
        }
        .error { err ->
            // handle the EnvelopeError as needed
            true
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

As its name would imply, there was an error at some stage of contract execution, this event allows you to see what the error was and handle it appropriately.



