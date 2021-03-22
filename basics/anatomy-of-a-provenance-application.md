---
description: >-
  Provenance is an ecosystem for application-specific financial services
  blockchain applications.
---

# Anatomy of the Provenance Application

In it's simplest form, Provenance is an [application-specific blockchain](https://docs.cosmos.network/master/intro/why-app-specific.html) build on the [Cosmos SDK.](https://docs.cosmos.network/master/intro/overview.html)  Thus, the anatomy described in this section is a derivative of the [Cosmos SDK Anatomy of an SDK ](https://docs.cosmos.network/master/basics/app-anatomy.html)[Application](https://docs.cosmos.network/master/basics/app-anatomy.html) document.

The Provenance SDK \(build upon the Cosmos SDK\) enables developers to build modules that implement the business logic of a financial services blockchain. In other words, SDK modules implement the bulk of the logic of the blockchains, while the core does the wiring and enables modules to be composed together. The end goal is to build a robust ecosystem of open-source SDK modules, making it increasingly easier to build complex blockchain applications.

While the Cosmos SDK documentation does a good job explaining the concepts of an application-specific blockchain, it is helpful to drill down into the core concepts and get hands-on.  That's what this section aims to do.

### Install the Node Client

{% hint style="info" %}
We'll be using the `provenanced` application to connect to the Provenance testnet in this section.  Follow the installation instructions in the [Installing Provenance section](../running-a-node/) before continuing with this section.
{% endhint %}

Before continuing with this section, confirm the `provenanced` binary is correctly installed:

```text
provenanced version
```

This command will output the `provenanced` version: 

```bash
$ provenanced version
0.2.0
```

If the `provenanced` does not work, refer to [Installing Provenance](../running-a-node/).

### Core Application

The Provenance core application-specific blockchain is effectively encompassed by the `provenanced` binary - which is both a client and a daemon process.  It is the core process of the Provenance blockchain.  Participants in the network run this process to host a node \(initialize their state-machine\), connect with other nodes, and update their state-machine as new blocks come in.  

As shown in the Provenance Node Components diagram, the Provenance application-specific blockchain is composed of multiple components all running on a single binary \(`provenanced`\) on a machine \(node\):

![Provenance Node Components](../.gitbook/assets/image%20%288%29.png)

#### Core State Machine Components

The **Tendermint BFT State Machine Replication** component \(again, compiled into the `provenanced` binary\) handles state machine replication, distribution, consensus, and networking.

The **Provenance SDK**, built on top of the Cosmos SDK, manages a state-machine and key-value store.  The Provenanced SDK [includes base modules from the Cosmos SDK](../modules/inherited-modules.md) as well as custom modules like [Metadata](../modules/metadata-module.md) and [Marker](../modules/marker-module.md) for financial-services related functionality.  The Provenance SDK component communicates with [Tendermint using the ABCI](https://docs.tendermint.com/master/spec/abci/#abci).  ABCI is a Tendermint construct and serves as the interface between Tendermint \(a state-machine replication engine\) and the Provenance application \(the actual state machine and blockchain implementation\).

The `provenanced` binary encapsulates the Provenance SDK \(and inheritied Cosmos SDK\) and Tendermint engine.  [Cosmovisor](https://docs.cosmos.network/master/run-node/cosmovisor.html) is a small process manager around `provenanced` that monitors the governance module via stdout to see if there's a chain upgrade proposal coming in. If it see a proposal that gets approved it can be run manually or automatically to download the new code, stop the node, run the migration script, replace the node binary, and start with the new genesis file.

#### Node Resources

The `provenanced` binary also communicates with and manages external \(to the binary\) components on the node including:

* A **Keyring** backend which holds the private/public keypairs used to interact with a node. For instance, a validator key needs to be set up before running the blockchain node, so that blocks can be correctly signed. The private key can be stored in different locations, called "backends".  Backends may include a file, the operating system's own key storage, or a Hardware Security Module \(preferred for Validators\).
* **Config Files** and **Genesis App** files that define the specific configuration settings of the node and genesis data consumed and stored as part of the blockchain node during initial startup.
* An **Upgrades** directory used by the `cosmovisor` process manager during chain upgrades.
* A **Data** directory where blockchain state is stored.  This is effectively the key-value storage used by the core application and modules.

#### External Application Interfaces

External applications communicate with a `provenanced` node instance using `gRPC`.  The `gRPC` layer of the `provenanced` application exposes the Provenance modules to external applications.  Thus, the core blockchain modules that implement the business logic of the blockchain, are available via the `gRPC` interface.

Where the `gRPC` interface provides access to the Provenance blockchain modules, the Event Manager dispatches blockchain transaction events.  External applications can subscribe to events like token or coin transfers, metadata updates, and governance proposals.

Multiple Figure applications leverage the `gRPC` and Event Manager interfaces including:

* Figure Pay: a payment application that uses stable coin, ledgered on Provenance, for consumer-to-consumer and consumer-to-business payment transactions.  
* Figure Lending: asset on-boarding and funding, pooling, and ownership exchange.
* Figure Marketplace: an asset exchange and marketplace.

