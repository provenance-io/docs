---
description: >-
  Provenance Blockchain facilitates the development of blockchain-based financial services
  via components.
---

# Major Components

### Application Trinity

Provenance Blockchain is composed of these core concepts:

* Modules that implement financial services business logic.  Modules are composed to realize complex financial services processes.
* Smart Contracting engine to develop and deploy contracts directly to the Provenance Blockchain.
* Off-chain client-side agreements using the Contract Execution Environment.

Provenance Blockchain distinguishes three types of applications based on these core concepts:

#### On-Chain

Any client application that is configured with the proper key/addresses and modules, and uses Hash 2.0 tokens for service payments, can transact directly on-chain within the Provenance Blockchain ecosystem.  Applications interact directly with the core [Provenance Blockchain Modules](../modules/inherited-modules.md) and a Provenance Blockchain Node.  Applications may implement and leverage custom Smart Contracts specific to the business application use case.

#### Side-Chain

Using the client-side [Contract Execution Environment](../p8e/overview.md), Provenance Blockchain includes functionality to encrypt and store confidential documents, and securely and selectively share those with other clients.  Note that the on-chain immutability is extended to the off-chain documents when the on-chain contract refers to those documents by their hash-id, i.e. the cryptographic hash of the document’s content. This functionality allows for off-chain transactions that optionally can complement the on-chain ones.  The Contract Execution Environment is a client hosted solution \(or, optionally, hosted by a trusted service provider\) that also interacts with a Provenance Blockchain Node.

#### Hybrid

Financial transactions often consist of on-chain and side-chain parts. For financial transactions, for example, the confidential documents supporting the transaction can be referred to by reference in the on-chain transaction. Those references point at immutable and encrypted documents that are safely stored off-chain. As part of the transaction, those confidential documents can be securely and selectively shared between the business partners. Provenance Blockchain fully supports this hybrid model, which allows for more complex financial transactions to be conducted within the Provenance Blockchain-ecosystem.

## Provenance Blockchain Node

A Provenance Blockchain Node is a daemon process \(`provenanced`\) [Full-Node Client](https://docs.cosmos.network/main/core/node.html) implementation.  It is the core process that runs the Provenance Blockchain. It runs the state-machine, starting from a genesis file, and connects to peers on the network running the same client to receive and relay transactions, block proposals and signatures.  Participants in the network run this process to initialize their state-machine, connect with other full-nodes and update their state-machine as new blocks come in.  The blockchain full-node presents itself as the `provenanced` binary.

Nodes in the network include:

* Query nodes used by business application middleware for efficient and fast state query.
* Transactional nodes used by business application middleware for efficient transaction submission and event listening.
* Archival nodes where the pruning strategy is set to maintain all historic states.
* [Validators](../blockchain/provenance-blockchain/validator/) that are responsible for committing new blocks in the blockchain for which they are rewarded with gas fees, i.e. Hash. These validators participate in the consensus protocol by broadcasting votes.  Validators bond their own Hash and have Hash delegated, or staked to, them by Hash holders.  Validators have a stake in the network.
* Sentry nodes for Validator DDoS mitigation.

### Cosmovisor

[Cosmovisor](https://docs.cosmos.network/main/run-node/cosmovisor.html) is a small process manager around the Provenance Blockchain daemon process \(`provenanced`\) that monitors the governance module for upgrade proposals. Approved upgrade proposals can be run manually or automatically to download the new code, stop the node, run the migration script, replace the node binary, and start with the a genesis file.

### Modules

[Modules](https://docs.cosmos.network/main/building-modules/intro.html) define the Provenance Blockchain logic. Provenance Blockchain is composed of modules from the Cosmos SDK and custom modules to support value markers and the client-side Contract Execution Environment.  Modules provide core functionality blockchain applications needs like a [boilerplate implementation of the ABCI](https://docs.cosmos.network/main/core/baseapp.html) to communicate with the underlying consensus engine, a [multistore](https://docs.cosmos.network/main/core/store.html#multistore) to persist state, a [server](https://docs.cosmos.network/main/core/node.html) to form a full-node and [interfaces](https://docs.cosmos.network/main/interfaces/interfaces-intro.html) to handle queries.  Modules implement the bulk of the logic of financial service applications and  the core does the wiring and enables modules to be composed together. 

Modules can be seen as little state-machines within the state-machine. They generally define a subset of the state using one or more key-value stores and message types.

### Key Ring

To interact with the `provenanced` daemon, and by extension the node, a keyring that holds the private/public key pairs used to interact with a node must be established. For example, a validator key needs to be set up before running the blockchain node so that blocks can be correctly signed. The private key can be stored in different locations, called "backends", such as a file or the operating system's own key storage.  [Refer to the Cosmos keyring documentation for more information.](https://docs.cosmos.network/main/run-node/keyring.html)

### Genesis

Before running a node the chain is initialized via a genesis file.  A default Provenance Blockchain genesis file is provided depending on the network in use \(mainnet versus testnet\).  Refer to [Running a Node](../running-a-node/) for more information on establishing a genesis file.

### Interacting with a Node

There are multiple ways to interact with a node: using the CLI, using gRPC or using the REST endpoints.  With a running node, the `provenanced` daemon process can be used as a CLI.  The CLI provides functionality for signing and submitting transactions, querying, key and key ring management, as well as Module interaction.  [Refer to the Cosmos Interacting with the Node documentation for more information.](https://docs.cosmos.network/main/run-node/interact-node.html)

## Smart Contracts

The Provenance Blockchain Smart Contract engine is based on the [CosmWasm](https://docs.cosmwasm.com/0.13/introduction/intro.html) smart contracting platform.  It is referred to as `provwasm` within the ecosystem.  It provides a the smart contracting component of the ecosystem.  

[ProvWasm](https://github.com/provenance-io/provwasm) is a module within Provenance Blockchain thereby providing smart contracting support to the Provenance Blockchain chain without adjusting existing logic. Smart contracts are hosted on a running Provenance Blockchain node \(`provenanced`\) where they can then be used by applications.  Applications connect to smart contracts in the same way as they would interact with modules: CLI, gRPC, or REST. 

There are 3 phases of a ProvWasm contract that help understand how they are used:

* Upload Code: Upload the optimized smart contract \(wasm\) code, no state nor contract address \(example Standard ERC20 contract\)
* Instantiate Contract: Instantiate a code reference with some initial state, creates new address \(example set token name, max issuance, etc for _my_ ERC20 token\)
* Execute Contract: This may support many different calls, but they are all unprivileged usage of a previously instantiated contract, depends on the contract design \(example: Send ERC20 token, grant approval to other contract\).

Provenance Blockchain Smart Contract instantiation and execution is metered and requires gas. Furthermore, both instantiation and execution allow the signer to send some tokens to the contract along with the message. Two key differences are that sending tokens directly to a contract _does not trigger any contract code_. 

Provenance Blockchain Smart Contract are able to leverage Provenance Blockchain Modules.  This allows for the creation of complex, multi-module contracts.

[A reference bi-lateral exchange smart contract is maintained here](https://github.com/provenance-io/bilateral-exchange).

## Contract Execution Environment

The Provenance Blockchain client-side Contract Execution Environment \(p8e\) to directly integrate with the [Provenance Blockchain Metadata Module](../modules/metadata-module.md) to simplify generating signed records of an asset’s Provenance Blockchain as either a single party or in conjunction with multiple interacting parties. At its core, P8e is a gRPC API that is invoked using a Kotlin SDK. The API processes data through a deterministic client side process that can be shared with other parties to transform data into a hashable format.

Assets can be directly defined against the Metadata module, but the execution environment assists in the complex hashing of data, maintenance of immutable objects, and signature orchestration between multiple parties.

### Client Contracts

Client side contracts differ from Smart Contracts in that they keep your data private between parties off-chain and thus facilitate a structure to record agreed-upon state-data to the blockchain. Smart contracts in comparison are running on the blockchain and require the validators to have access to the data which complicates many consumer based transactions due to data privacy laws.

### Transaction Flow

Entities and organizations utilize the Contract Execution Environment to submit contracts to create single or multi-party agreements.  Entities use key pairs consisting of a public and private key.  Entities are known to each other using public keys and share data with each other using public keys.  Contracts and asset data are forwarded to all entities participating in the contract by public key identifier.  Entities provide an implementation of an Encrypted Object Store where encrypted data related to their public key is stored.  Contract execution uses data from the entity EOS and the results of the contract executions are returned to the submitting entity's execution environment.  The hash-ids of the Contract execution results, i.e. the cryptograph hash of its content, are submitted to the Provenance Blockchain \(via the Metadata module\) where they are validated and committed to the chain.  Provenance Blockchain emits events notifying entities the contract has been committed on the blockchain.  An index, local to the entity, is updated with the new contract information. This information is used later for searching and querying the data.

Refer to the [Contract Execution Environment](../p8e/overview.md) for information and tutorials.

