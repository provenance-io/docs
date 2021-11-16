---
description: How EaaS ingests data from the blockchain
---

# Ingestion

The [CosmosSDK](https://github.com/cosmos/cosmos-sdk) does have event streaming for results of transaction processing, but they lose the correlation between event and transaction. Currently there is only one way to ingest all points of data from the blockchain without losing those correlating data points that make the data useful.&#x20;

The SDK supports the use of [Google Protobuf](https://developers.google.com/protocol-buffers) for managing transactions, queries, and the objects used for both. Protobuf message objects have the ability to define whats called **gRPC** - an API layer that allows for seamless integration with any client.

EaaS leverages the given gRPC APIs to quickly query the blockchain for any piece of data it might need.&#x20;

### How it starts

As everything on a blockchain begins with a **block**, so must the ingestion process.

Block

Proposer

Validator Set

Missed Blocks

Transactions

* Transaction
* Gas fees
* Messages, message types
* Addresses
* Markers/Denoms
* NFTs
* Governance
* IBC
* Smart Contracts
* Transaction signatures

Update assets/denoms

Update validator records

Update cached tx counts

Update block latency
