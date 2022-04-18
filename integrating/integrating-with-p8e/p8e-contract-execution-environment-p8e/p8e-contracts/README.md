---
description: Developing and publishing p8e contracts for loan life cycle events
---

# P8e Contracts

Asset originators can technically get away with mastering a single p8e contract to simply onboard assets to Provenance, however, executing a single contract throughout the entire life cycle of an asset such as a mortgage would provide little utility. For that reason this guide will touch on a series of p8e contracts designed to onboard and update a mortgage as it goes through the flow of being onboarded and validated by a third party in preparation for sale to an investor.

### P8e Gradle Plugin

{% hint style="info" %}
Having an understanding of the [Provenance Metadata module](https://docs.provenance.io/modules/metadata-module) is strongly recommended.
{% endhint %}

In order to execute contracts with the [Provenance Scope SDK](https://github.com/provenance-io/p8e-scope-sdk), the contracts must be published into your execution environment. The `p8e-gradle-plugin` provides a set of tasks in order to accomplish just that. Publishing contracts performs the following high level actions in order to allow contracts to be executed:

* An uberjar is built which contains a set of contracts and all associated protobuf messages associated with those contracts. This uberjar is persisted to p8e's encrypted [object store](https://github.com/provenance-io/object-store). This allows p8e to later pull it and make use of a [Class Loader](https://docs.oracle.com/javase/7/docs/api/java/lang/ClassLoader.html) to load it at runtime.
* A concrete implementation of [ContractHash](https://github.com/provenance-io/p8e-scope-sdk/blob/main/contract-base/src/main/kotlin/io/provenance/scope/contract/contracts/ContractHash.kt) is generated and stored alongside your source code. Similarly, an implementation of [ProtoHash](https://github.com/provenance-io/p8e-scope-sdk/blob/main/contract-proto/src/main/kotlin/io/provenance/scope/contract/proto/ProtoHash.kt) is also generated. These classes will be built into the jars that are depended on by the loan onboarding service, or any other application that will execute contracts. These classes provide a mapping from P8eContracts and their associated protobuf messages to the hash of the uberjar they are contained within. Ultimately, the sdk will make use of these classes via the [service provider](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html) facility.

### Publishing via the p8e CEE API

To make things easier, the `p8e-CEE-API` exposes the `p8e-gradle-plugin` functionality in a RESTful endpoint, allowing users to publish their own p8e contracts or pre-defined contracts into their own p8e environment. See the [Create Contract Specification](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-contract-specification) documentation on the API Specification page.

The inputs that remain the same no matter which environment you're working in include:

* account - the originator ID that will map to your key management solution
* scopeId - UUID for the new scope created for this new contract specification
* scopeSpecId - The ID of the Scope Specification associated with this contract
* contractSpecId - UUID for the new contract specification

The `chainId` and `nodeEndpoint` values depend on your environment.

#### Local Environment

These values depend on values set in the `docker-compose.yml` and `genesis.json` files, but they default to:

`chainId` - chain-local

`nodeEndpoint` - tcp://localhost:26657

#### Test Environment

`chainId` - pio-testnet-1

`nodeEndpoint` - Ask for appropriate node endpoint

#### Production Environment

`chainId` - pio-mainnet-1

`nodeEndpoint` - Ask for appropriate node endpoint
