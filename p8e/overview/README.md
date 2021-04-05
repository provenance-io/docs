# P8e Contract Execution Environment

### Overview

The Provenance Contact Execution Environment \(nicknamed “P8e”\) is an optional layer on top of the Provenance Blockchain to allow single- and multi-party client-side contract execution while preserving data privacy. Provenance client-side contracts take encrypted data from the user \(client\) and transform the information into encrypted data in the user’s own private object store with object hashes recorded on the blockchain. 

Components of the P8e environment include a client-side contract execution engine, a locally-hosted encrypted object store \(EOS\), an Elasticsearch-based index into the EOS, and a communication exchange for orchestrating multi-party contract execution with other parties’ own P8e environments. Each P8e instance communicates with a local blockchain node to submit transactions \(contract execution hash records\) to the Provenance Blockchain.

### Facts

In the construct of the P8e contract environment, a “fact” is any data object stored in the EOS whose hash sum has been recorded in a transaction on the Provenance Blockchain. All inputs and outputs to contract functions have their hash calculated and stored on-chain and become facts. However, the input hash facts are merely part of the contract execution record in the scope, and are not part of the head-state of “facts in the scope”.

As the facts evolve through subsequent contract execution, each execution record is added to the list of record groups in the scope. In this way, the history of the evolution of data in the scope is preserved and queryable.

### Scopes

A P8e scope contains information about an asset that is memorialized to the blockchain. This information includes the participants involved in the contract, the contract executed, and the hashed execution results of the contract. A P8e scope consists of record groups, which consists of records \(facts\). Records contained in a scope can be the result of multiple contract executions.

### Scopes are NFTs

A scope consists of a unique set of facts and the history of fact evolution in the scope through contract executions. Each scope is globally unique. Value ownership of a scope is recorded on the Provenance Blockchain through a Marker established on-chain. The Marker may be tokenized, allowing for fractional ownership of the scope. In this way, Provenance scopes are non-fungible tokens \(NFTs\) and may be traded on any Provenance-based trading platform.

### P8e Client-Side Contracts

P8e contracts are distinct from blockchain-based smart contracts. P8e contracts execute only in the contract execution environment of the parties participating in the contract, and operate only on data stored in the EOS. A record of the contract execution is memorialized to the blockchain using the [Provenance Metadata Module](../../modules/metadata-module.md) which, in turn, records the metadata to chain through the use of blockchain-based smart contracts. 

Client side contracts differ from smart contracts in that they keep your data private between parties off chain and allow a structure to record agreed upon state data to the blockchain. Smart contracts in comparison are running on the blockchain and require the validators to have access to the data which can be problematic for most consumer-based transactions due to data privacy laws.

### P8e API

At its core, P8e is a GRPC API that is invoked using a Kotlin SDK. The API processes data through a deterministic client-side process that can be shared with other parties to transform data into a hashable format. Assets can be directly defined against the Metadata module, but the execution environment assists in the complex hashing of data, maintenance of immutable objects, and signature orchestration between multiple parties.





