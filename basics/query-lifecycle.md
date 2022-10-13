---
description: Querying blockchain state.
---

# Query Lifecycle

A [**query**](https://docs.cosmos.network/main/building-modules/messages-and-queries.html#queries) is a request for information made by end-users of applications through an interface and processed by a full-node. Users can query information about the network, the blockchain itself, and blockchain state directly from the blockchain's stores or modules.  Queries differ from transactions in that they do not require consensus to be processed and therefore do not change state.  Queries can be fully handled by a single full-node.

Refer to [Using Provenanced](../using-provenance/) for hands-on queries.

### Query Endpoints

There are multiple interfaces to submit transactions to the blockchain.

#### CLI

The main interface for an application is the command-line interface. Users connect to a full-node and run the CLI directly from their machines - the CLI interacts directly with the full-node.  

#### gRPC

Users and applications can submit queries using [gRPC](https://grpc.io/) requests to a [gRPC server](https://docs.cosmos.network/main/core/grpc_rest.html#grpc-server). The `provenanced` daemon process is bundled with gRPC endpoints by default. The endpoints are defined as [Protocol Buffers ](https://developers.google.com/protocol-buffers)service methods inside `.proto` files, written in Protobuf's own language-agnostic interface definition language \(IDL\). The Protobuf ecosystem developed tools for code-generation from `*.proto` files into various languages. These tools allow to build gRPC clients easily.

[gRPCurl](https://github.com/fullstorydev/grpcurl) is an excellent command-line tool that can be used to interact with blockchain gRPC endpoints.

#### REST

Users and applications can submit queries through HTTP Requests to a [REST server](https://docs.cosmos.network/main/core/grpc_rest.html#rest-server). The REST server is fully auto-generated from Protobuf services, using [gRPC-gateway](https://github.com/grpc-ecosystem/grpc-gateway).

