# P8e Contract Execution Environment

### Overview

The Provenance Blockchain Contact Execution Environment \(nicknamed “P8e”\) is an optional layer on top of the Provenance Blockchain to allow single-party and multi-party client-side contract execution while preserving data privacy. Provenance Blockchain client-side contracts take data from the user \(client\) and transform the information into encrypted data in the user’s own private object store with object hashes recorded on the blockchain. 

Components of the P8e environment include a client-side contract execution engine, an encrypted object store \(EOS\) which can be run in your infrastructure or be purchased as a managed solution. The EOS doubles as  a communication exchange for orchestrating multi-party contract execution with other parties’ own P8e environments.

To make use of the existing P8e contract execution libraries, Contracts must be written in a JVM based language and make use of the annotation structure that currently exists. Likewise for contract execution. That being said, the collection of libraries published, or software development kit \(SDK\), could be mimicked in any language that supports gRPC clients.

### Records

In the construct of the P8e contract environment, a “record” is any data object stored in the EOS whose hash sum has been recorded in a transaction on the Provenance Blockchain. All inputs and outputs to contract functions have their hash calculated and stored on-chain and become records. However, the input hash facts are merely part of the contract execution record in the scope, and are not part of the head-state of “records in the scope”.

As the records evolve through subsequent contract execution, each execution record is added to the list of record groups in the scope. In this way, the history of the evolution of data in the scope is preserved and queryable.

### Scopes

The P8e contract environment leverages the [Provenance Blockchain Metadata module](../../modules/metadata-module.md). Scopes are used as containers to represent the outputs produced from contract executions. Each contract execution on a scope generates a "session", which encapsulates one or more "records". Once memorialized, the session and records are immutably recorded to Provenance Blockchain.

### Scopes are NFTs

A scope consists of a unique set of records and the history of record evolution in the scope through contract executions. Each scope is globally unique. Value ownership of a scope is recorded on the Provenance Blockchain through a Marker established on-chain. The Marker may be tokenized, allowing for fractional ownership of the scope. In this way, Provenance Blockchain scopes are non-fungible tokens \(NFTs\) and may be traded on any Provenance Blockchain-based trading platform.

### P8e Client-Side Contracts

P8e contracts are distinct from blockchain-based smart contracts. P8e contracts execute only in the contract execution environment of the parties participating in the contract, and operate only on data stored in the EOS. A record of the contract execution is memorialized to the blockchain using the [Provenance Blockchain Metadata Module](../../modules/metadata-module.md) which, in turn, records the metadata to chain through the use of blockchain-based smart contracts. 

Client side contracts differ from smart contracts in that they keep your data private between parties off chain and allow a structure to record agreed upon state data to the blockchain. Smart contracts in comparison are running on the blockchain and require the validators to have access to the data which can be problematic for most consumer-based transactions due to data privacy laws.

