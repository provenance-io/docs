---
description: 'Hosting a node on Provenance mainnet, testnet, or local development.'
---

# Running a Node

Nodes on Provenance are simply servers running the `provenanced` process with a specialized configuration. There are several types of configurations that can be used to assist in application development, seeding, data archival, validation, etc... To start, let's quickly introduce the types of configurations that may be helpful when deciding what type of node is best suited for your use case.

### Node Configurations

`provenanced` is an all encompassing command that can operate as any type of node based completely on how it has been configured. When determining how a particular node should be configured, it is necessary to take a deeper look at the Provenance and how it structured. 

### Seed Node

A node configured to provide an ip/port address book for other nodes to initially find and connect to peers. This node acts as a pivot point for connections to sentinel nodes on the network. 

### Sentinel Node

A node configured to provide data and connectivity to the mesh network. Sentinels can be configured to provide rpc based p2p communication to other network participants, archival of blockchain data and query access to blockchain data. This is the most used node type and will be the first node that most network participants will interact with.

### Sentry Node

A node configured to provide a secured gateway that can communicate with a validator node. Sentry nodes protect validators by providing a layer between inbound proposals and the validator network. 

### Validator Node

A node configured to participate in the consensus algorithm and is responsible for signing and proposing blocks to commit to the blockchain. 

### More Information

[Tendermint](https://tendermint.com/) has is an excellent resource for information regarding node configuration and details. For a detailed overview of the different types of nodes supported, see [Tendermint Node Types](https://docs.tendermint.com/master/nodes/).

## Next

Most network participants will need a specialized sentinel node configuration that provides api level access to Provenance. Let's use `provenanced` configured as a full sentinel node and [join testnet](join-provenance-testnet.md).

