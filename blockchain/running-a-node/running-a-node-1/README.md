---
description: 'Hosting a node on Provenance mainnet, testnet, or for local development.'
---

# Running a Node

Nodes on the Provenance blockchain network are simply servers started by the `provenanced` executable with a specific configuration. There are several types of configurations such as seeding, data archival, or validation that may be used to assist in application development. These configuration types may be helpful in deciding what type of node is best suited for the different use cases.

## Node Configurations

`provenanced` is an all-encompassing command that may operate as any type of node, depending on how it has been configured. When determining how a particular node should be configured, it is necessary to take a deeper look at Provenance and how it is structured. 

### Seed Node

A seed node is configured to provide an IP/port address book for other nodes to initially find and connect to peers. This node acts as a service directory for connections to sentry nodes on the network. 

### Sentry Node

A sentry node is configured to provide data and connectivity to the mesh network. Sentries may be configured to provide RPC-based p2p communication to other network participants, archive blockchain data, query access to blockchain data, and provide a secured gateway that can communicate with and protect a validator node. This is the most used node type and will be the first node with which most network participants will interact. 

### Validator Node

A validator node is configured to participate in the consensus algorithm and is responsible for signing and proposing blocks to commit to the blockchain. It also holds Hash as stake, and receives Hash for its services.

### More Information

[Tendermint](https://tendermint.com/) is an excellent resource for information regarding node configuration and details. For a detailed overview of the different types of nodes supported, see [Tendermint Node Types](https://docs.tendermint.com/master/nodes/).

