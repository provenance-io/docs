# Quick Start

The quickest and easiest way to get started executing contracts and memorializing transactions to the blockchain is to utilize Provenance’s sandbox environment. A basic Hello World contract has been deployed to the sandbox. This contract facilitates a single party agreement by an owner. You will be acting as the owner in this example and will provide a name to be memorialized to the blockchain. There are three parts to this example:

1. Execute a contract to memorialize a fact to the blockchain that name information has been added.
2. Execute a contract to update the name information that was memorialized to the blockchain in step 1.
3. Retrieve the information from the blockchain and encrypted object store.

This section primarily focuses on how to execute the example contracts. The next section \(Hello World Example\) will explain the example in further detail.

## Register Public/Private Keys

To verify identity and to encrypt data, a public/private key pair is used. Generate the key pair. For this example, Provenance can generate these on your behalf. Once the keys have been generated, they need to be provided to Provenance where they will be registered against the test system.

```kotlin
        ProvenanceKeyGenerator.generateKeyPair().let {
            it.private.toHex().also(::println)
            it.public.toHex().also(::println)
        }
```

## Prepare Project

Create a new project which will be used to execute the contracts. Once the new project is created, dependencies for p8e-sdk, p8e-contract, and p8e-contract-proto need to be added. For example, if using Gradle the following dependencies need to be added to the gradle.build.

```text
dependencies {
    implementation("io.p8e:p8e-sdk:master-+")
    implementation("io.p8e.contracts:p8e-contract:master-+")
    implementation("io.p8e.contracts:p8e-contract-proto:master-+")
}
```

Classes need to be created to execute the contracts. For this example, separate classes are used for inserting, updating, and retrieving. The class to execute the initial contract should consist of the following functionality:

* Load the primary key to allow the contract to be initiated.
* Configure watchers to listen for responses and errors from the contract execution.
* Create a new contract and identify the information needing to be memorialized to the blockchain by using the addProposedFact function.
* Execute the contract.

An example contract execution class is provided. When creating the ContractManager, the primary\_key\_text is the key previously registered against the system. Ensure there is a watcher configured to listen for responses which will log the resulting UUID for future use.

```kotlin
package example

import io.p8e.ContractManager
import io.p8e.contracts.example.HelloWorldContract
import io.p8e.proto.ContractSpecs.PartyType.*
import io.p8e.proto.contract.HelloWorldExample.ExampleName
import io.p8e.util.toJavaPrivateKey
import io.provenance.core.extensions.logger

class HelloWorld {
    private val log = logger()
    private val contractManager = ContractManager.create("<private_key_text>".toJavaPrivateKey(), "<api_url>")

    init {
        contractManager.watchBuilder(HelloWorldContract::class.java).watch()
    }

    fun runHelloWorld() {
        contractManager.newContract(HelloWorldContract::class.java, OWNER).apply {
            addProposedFact("name", ExampleName.newBuilder()
                .setFirstName("Hello")
                .setLastName("World")
                .build())
            contractManager.execute(this)
        }
    }
}

fun main() {
    HelloWorld().runHelloWorld()
}
```

## Configure Project

Next, an Environment variable needs to be added so the application executes against the sandbox environment.

```text
API_URL=grpcs://test.figure.com
```

As an alternate to using an environment variable, the API URL can be passed in as a parameter when the ContractManager is created.

```kotlin
private val contractManager = ContractManager.create("<private_key_text>".toJavaPrivateKey(), "grpcs://test.figure.com")
```

## Execute Example

Now the contract should be ready to be executed. Execute the newly-created class. This will execute the HelloWorldContract. Since this is a single contract, no other affiliates need to execute and sign the contract. If the test is successful, informational messages will be logged stating the transaction was recorded to the blockchain. Make note of this scope UUID as it will be used when executing the other contracts.

```text
... with Scope UUID: <UUID>
```

If the test is unsuccessful, an error message will be logged detailing the failure reason.

This demonstrated how a single party executes an existing contract to memorialize new information to the blockchain. Next, changes to the information will be memorialized to the blockchain.

## Execute Modify Example

Similar to the class to execute the initial contract, create a new class to execute the modify contract. In addition to the same steps, since a modification is being proposed, the contract also needs to include the facts memorialized by the first contract. This is accomplished by passing in the scope \(retrieved using the scope UUID output from the first contract execution\) passed into the ContractManager constructor.

Create the ContractManager using the same private key as the first step. Ensure there are watchers also configured to listen for responses from this contract.

```kotlin
package example

import io.p8e.ContractManager
import io.p8e.contracts.example.HelloWorldData
import io.p8e.contracts.example.HelloWorldModifyContract
import io.p8e.proto.ContractSpecs.PartyType.*
import io.p8e.proto.contract.HelloWorldExample.ExampleName
import io.p8e.util.toJavaPrivateKey
import io.provenance.core.extensions.logger
import java.util.*

class HelloWorldModify {
    private val log = logger()
    private val contractManager = ContractManager.create("<private_key_text>".toJavaPrivateKey(), "<api_url>")

    init {
        contractManager.watchBuilder(HelloWorldModifyContract::class.java).watch()
    }

    fun runHelloWorldModify() {
        val data = contractManager.hydrate(UUID.fromString("<scope UUID>"), HelloWorldData::class.java)
        contractManager.newContract(HelloWorldModifyContract::class.java, data.scope, OWNER).apply {
            addProposedFact("name", ExampleName.newBuilder()
                .setFirstName(data.name.firstName)
                .setLastName(data.name.lastName)
                .setMiddleName("testing")
                .build()
            )
            contractManager.execute(this)
        }
    }
}

fun main() {
    HelloWorldModify().runHelloWorldModify()
}
```

Execute the class once it’s created. Since this is again a single contract, no other affiliates need to execute and sign the contract. This demonstrated how a single party executes an existing contract to memorialize modifications to existing information to the blockchain. Next, the information will be retrieved from the blockchain.

## Retrieve Information Example

This step will demonstrate how to retrieve information from the blockchain as well as the actual asset data from the encrypted object store. Create another new class to retrieve the data using the scope UUID output from the first contract. This example uses the ContractManager to retrieve the information and log the output.

```kotlin
package example

import io.p8e.ContractManager
import io.p8e.contracts.example.HelloWorldData
import io.p8e.util.toJavaPrivateKey
import io.provenance.core.extensions.logger
import java.util.*

class HelloWorldRetrieve {
    private val log = logger()
    private val contractManager = ContractManager.create("<private_key_text>".toJavaPrivateKey(), "<api_url>")

    fun runHelloWorldRetrieve(scopeUuid: String) {
        val scopeWrapper = contractManager.indexClient
            .findLatestScopeByUuid(UUID.fromString(scopeUuid))
        if (scopeWrapper.scope.recordGroupList.size == 0) {
            log.info("No records found for the provided scope UUID")
        } else {
            log.info(scopeWrapper.scope.toString())
        }

        log.info(contractManager.hydrate(UUID.fromString(scopeUuid),
            HelloWorldData::class.java).toString())
    }
}

fun main() {
    HelloWorldRetrieve().runHelloWorldRetrieve("<scope UUID>")
}
```

Execute the class once it’s created. This will log information retrieved from the blockchain, such as the participants involved in the transaction, the contract executed, and the hashed execution results of the contract. It will also log the asset data retrieved from the encrypted object store. In this case, it’s name information.

The Hello World example demonstrated how a single party executes smart contracts, memorialize the results to the blockchain, retrieves events when the processes complete, and retrieves information from the blockchain and encrypted object store. The next section will explain the processes in further detail.

