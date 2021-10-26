---
description: Submitting transactions to the blockchain.
---

# Transaction Lifecycle

## Transaction Flow

When users want to interact with the blockchain and make state changes (e.g. sending coins), they create transactions. [Transactions](https://docs.cosmos.network/master/core/transactions.html) originate via some user application and trigger state changes within a blockchain module (i.e. the bank module and transferring coin). Each of a transaction's `Msg`s must be signed using the private key associated with the appropriate [account](accounts.md)(s) before the transaction is broadcast to the network. A transaction must then be included in a block, validated, and approved by the network through the consensus process.

![Transaction Submission Flow](<../../.gitbook/assets/image (9).png>)

The Transaction Submission Flow diagram illustrates how a user application transaction (e.g. transferring coins) flows within the blockchain:

* User input is captured by a User Application. The **User Application** is responsible for:
  * Building the appropriate blockchain `Tx Msg` transaction message&#x20;
  * Signing the `Tx Msg` using a key pair from a wallet or Key Management facility
  * Setting gas and fee parameters
  * Submitting the signed `Tx Msg` to a blockchain node using the CLI or `gRPC.`
* The `Tx Msg` is received by a blockchain node, again a device running the `provenanced` daemon process.  The `Tx Msg` is placed into the `Mempool Cache` for [pre-processing validation](https://docs.cosmos.network/master/basics/tx-lifecycle.html#addition-to-mempool).  These pre-process checks include checking that the appropriate addresses are not empty, enforcing field checks like non-negative numbers, or other logic specified in the module.
  * If the `Tx Msg` is invalid it is rejected and handled by the User Application.  Nothing is added to the blockchain.
* Once the `Tx Msg` passes pre-processing in `Check Tx` it is submitted to the `Mempool` where is is broadcast to other blockchain peers and eligible for inclusion in a block.  The `Mempool` serves the purpose of keeping track of transactions seen by all full-nodes. Full-nodes keep the `Mempool Cache` of the last transactions they have seen, as a first line of defense to prevent replay attacks. `CheckTx` is responsible for identifying and rejecting replayed transactions.
* The **Consensus Engine** uses a Consensus Round Structure where a **Proposer Node** is selected in turn.  Thus, each round a new Proposer Node is designated to propose the new block.  The Proposer Node selects transactions from its Mempool for inclusion in the proposed block.  Note that each Node has an up-to-date copy of the Mempool communicated via node gossip.
  * Effectively, at each height of the blockchain a round-based protocol is run to determine the next block. Each round is composed of three _steps_ (`Propose`, `Prevote`, and `Precommit`), along with two special steps `Commit` and `NewHeight`.  Refer to [the Tendermint Byzantine Consensus Algorithm](https://docs.tendermint.com/master/spec/consensus/consensus.html) for more information.
* Once the **Proposer Node **selects the `Tx Msg` from the `Mempool` it proposes a new block and broadcasts the proposed block.
* The `Tx Msg` is in the **Prevote** phase and each node broadcasts a prevote and listens for prevotes from other nodes.
  * If the block is invalid or a timeout is reached before a prevote is complete a Prevote Nil is broadcast.  Otherwise, Pre-vote Block is broadcast.
* In the **Precommit** round, each node broadcasts its resulting Prevote response and listens for response from other nodes.
  * If > 2/3 nodes do not pre-vote for the same block and a timeout is reached Precommit Nil is broadcast.  Otherwise, if > 2/3 nodes Prevote for the block a Pre-commit Block is broadcast.
* In the **Commit** phase the block is added and **New Height** is established (i.e. the blockchain block height increments)
  * If > 2/3 nodes to not pre-commit for the block or a timeout is reached, a No Commit is broadcast and the block is not added.  Otherwise, if > 2/3 node pre-commit the block is committed.
* Finally, the blockchain nodes dispatch Tx Events that can be handled by the User Application (i.e. block committed).
* A New Round starts for the next transaction proposals.

The important components of this flow are:

* Transactions are built and signed by a User Application before being submitted to a blockchain node.  _The `provenanced` binary can be considered a User Application when run via the command-line as a client._
* The key pair and associated address used to sign and submit the transaction must hold Hash to pay the gas fees when submitting the transaction to the blockchain.
* The Transaction moves through multiple stages on the blockchain before it is committed in a block.  Each of these stages can fail and must be handled by the User Application.
* The blockchain will broadcast events related to transaction processing that can be consumed by the User Application.

{% hint style="info" %}
The [Cosmos documentation also provides a Transaction Lifecycle walk through ](https://docs.cosmos.network/master/basics/tx-lifecycle.html)with detailed information about the Mempool, State Changes, Consensus Rounds, and more. &#x20;
{% endhint %}

### Transaction Endpoints

There are multiple interfaces to submit transactions to the blockchain.

#### CLI

The main interface for an application is the command-line interface. Users connect to a full-node and run the CLI directly from their machines - the CLI interacts directly with the full-node. &#x20;

#### gRPC

Users and applications can submit transactions using [gRPC](https://grpc.io) requests to a [gRPC server](https://docs.cosmos.network/master/core/grpc\_rest.html#grpc-server). The `provenanced` daemon process is bundled with gRPC endpoints by default. The endpoints are defined as [Protocol Buffers ](https://developers.google.com/protocol-buffers)service methods inside `.proto` files, written in Protobuf's own language-agnostic interface definition language (IDL). The Protobuf ecosystem developed tools for code-generation from `*.proto` files into various languages. These tools allow to build gRPC clients easily.

[gRPCurl](https://github.com/fullstorydev/grpcurl) is an excellent command-line tool that can be used to interact with blockchain gRPC endpoints.

#### REST

Users and applications can submit transactions through HTTP Requests to a [REST server](https://docs.cosmos.network/master/core/grpc\_rest.html#rest-server). The REST server is fully auto-generated from Protobuf services, using [gRPC-gateway](https://github.com/grpc-ecosystem/grpc-gateway).

[Refer to Using Provenanced](../using-provenance/) for hands-on transaction submission.
