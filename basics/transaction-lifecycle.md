---
description: Hands-on with Provenance blockchain transactions.
---

# Transaction Lifecycle

### Transaction Flow

When users want to interact with the blockchain and make state changes \(e.g. sending coins\), they create transactions. [Transactions](https://docs.cosmos.network/master/core/transactions.html) originate via some user application and trigger state changes within a blockchain module \(i.e. the bank module and transferring coin\). Each of a transaction's `Msg`s must be signed using the private key associated with the appropriate [account](accounts.md)\(s\) before the transaction is broadcast to the network. A transaction must then be included in a block, validated, and approved by the network through the consensus process.

![](../.gitbook/assets/image%20%289%29.png)

The Transaction Submission Flow diagram illustrates how a user application transaction \(e.g. transferring coins\) flows within the blockchain:

* User input is captured by a User Application.  The **User Application** is responsible for:
  * Building the appropriate blockchain `Tx Msg` transaction message 
  * Signing the `Tx Msg` using a key pair from a wallet or Key Management facility
  * Determining gas and fee costs
  * Submitting the signed `Tx Msg` to a blockchain node using `gRPC`.
* The `Tx Msg` is received by the blockchain node, again a device running the `provenanced` daemon process.  The `Tx Msg` is placed into the `Mempool Cache` for [pre-processing validation](https://docs.cosmos.network/master/basics/tx-lifecycle.html#addition-to-mempool).  These pre-process checks include checking that the appropriate addresses are not empty, enforcing field checks like non-negative numbers, or other logic specified in the module.
  * If the `Tx Msg` is invalid it is rejected and handled by the User Application.  Nothing is added to the blockchain.
* Once the `Tx Msg` passes pre-processing in `Check Tx` it is submitted to the `Mempool` where is is broadcast to other peers and eligible for inclusion in a block.  The `Mempool` serves the purpose of keeping track of transactions seen by all full-nodes. Full-nodes keep the `Mempool Cache` of the last transactions they have seen, as a first line of defense to prevent replay attacks. `CheckTx` is responsible for identifying and rejecting replayed transactions.
* The **Consensus Engine** uses a Consensus Round Structure where a **Proposer Node** is selected in turn.  Thus, each round a new Proposer Node is designated to proposed the new block.  The Proposer Node selects transactions from its Mempool for inclusion in the proposed block.  Note that each Node has an up-to-date copy of the Mempool communicated via node gossip.
  * Effectively, at each height of the blockchain a round-based protocol is run to determine the next block. Each round is composed of three _steps_ \(`Propose`, `Prevote`, and `Precommit`\), along with two special steps `Commit` and `NewHeight`.  Refer to [the Tendermint Byzantine Consensus Algorithm](https://docs.tendermint.com/master/spec/consensus/consensus.html) for more information.
* Once the **Proposer Node** selects the `Tx Msg` from the `Mempool` it proposes a new block and broadcasts the proposed block.
* The `Tx Msg` is in the **Prevote** phase and each node broadcasts a prevote and listens for prevotes from other nodes.
  * If the block is invalid or a timeout is reached before a prevote is complete a Prevote Nil is broadcast.  Otherwise, Pre-vote Block is broadcast.
* In the **Precommit** round, each node broadcasts its resulting Prevote response and listens for response from other nodes.
  * If &gt; 2/3 nodes do not pre-vote for the same block and a timeout is reached Precommit Nil is broadcast.  Otherwise, if &gt; 2/3 nodes Prevote for the block a Pre-commit Block is broadcast.
* In the **Commit** phase the block is added and **New Height** is established \(i.e. the blockchain block height increments\)
  * If &gt; 2/3 nodes to not pre-commit for the block or a timeout is reached, a No Commit is broadcast and the block is not added.  Otherwise, if &gt; 2/3 node pre-commit the block is committed.
* Finally, the blockchain nodes dispatch Tx Events that can be handled by the User Application \(i.e. block committed\).
* A New Round starts for the next transaction proposals.

The important components of this flow are:

* Transactions are built and signed by a User Application before being submitted to a blockchain node.  _The \`provenanced\` binary can be considered a User Application when run via the command-line as a client._
* The key pair and associated address used to sign and submit the transaction must hold Hash to pay the gas fees when submitting the transaction to the blockchain.
* The Transaction moves throught multiple stages on the blockchain before it is committed in a block.  Each of these stages can fail and must be handled by the User Application.
* The blockchain will broadcast events related to transaction processing that can be consumed by the User Application.

### Hands On: Submitting Transactions via the CLI

{% hint style="info" %}
This section uses the `provenanced` application to connect to the Provenance testnet.  Follow the installation instructions in the [Installing Provenance section](../running-a-node/) before continuing with this section.
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

#### Set Up a Key Pair

The `provenanced` binary provides a command-line interface to create transactions.  To create a transaction requires just a few items: 

* a key pair capable of signing the transaction
* a key pair capable of paying the gas fee
* a blockchain node to submit the transaction to.

First, let's create a key pair and store it in a local Keyring:

```bash
provenanced --testnet keys add my_tx_key
```

{% hint style="info" %}
The `--testnet`flag is a shortcut used to tell `provenanced` that we want to use testnet configurations.
{% endhint %}

Creating a key pair will return the `name` of the key and the `address:`

```bash
name: my_tx_key
  type: local
  address: tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs
  pubkey: tppub1addwnpepqgxlsrgv9znqeass2al9u9h37khapuecpp0al82wqyhzvn9hwtnzwak5uf3
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

Next, copy the `address` \(in this example `tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs`\) and open the Provenance testnet Hash faucet [https://explorer.test.provenance.io/faucet](https://explorer.test.provenance.io/faucet), paste the value, and press Get Tokens:

![Use the testnet Faucet to get Hash](../.gitbook/assets/image%20%2811%29.png)

Your address will now have enough Hash to pay gas fees.  Confirm the address has Hash:

```bash
provenanced --testnet q bank balances tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs \
 --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
balances:
- amount: "1000000000"
  denom: nhash
pagination:
  next_key: null
  total: "0"
```

{% hint style="info" %}
The `--node` flag allows us to connect our `provenanced` client to a node running remotely.  Thus, we're connecting to a public testnet node hosted at `rpc-0.test.provenance.io.`
{% endhint %}

{% hint style="info" %}
The address associated to our key pair is a [Bech32](https://en.bitcoin.it/wiki/Bech32) address which is an encoded value of the public key portion of our key pair.  Provenance testnet Bech32 addresses begin with `tp` whereas mainnet addresses begin with `pb`.  

Once we transferred Hash to our Bech32 address, it became a [Provenance account](accounts.md). 
{% endhint %}

#### Create a Transaction

Now that we have a key pair and have used the testnet Faucet to give our key gas, we can submit transactions to the blockchain.

Let's first find an existing Hash holder address that we can transfer some of our Hash to.  Using the `provenanced` query commands and the Marker module, we can list everyone holding Hash:

```bash
provenanced --testnet q marker holding nhash --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
balances:
- address: tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm
  coins:
  - amount: "963530000"
    denom: nhash
- address: tp1pr93cqdh4kfnmrknhwa87a5qrwxw9k3dhkszp0
  coins:
  - amount: "99089999999600000000"
    denom: nhash
- address: tp1pmc6v4p8rak0lmzcrsm9t2hzm9vnzwy2a5xyhv
  coins:
  - amount: "866545000"
    denom: nhash
- address: tp1zk3qvk6dvpk6394chmvq5gtrw5arqzsag072h5
```

Let's pick someone with a small amount of Hash, like the first address in our list `tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm` \(note, the list may be different when you run this command\).

{% hint style="info" %}
What is `nhash`?  `nhash` is the smallet unit of Hash, or a nano-Hash, where 1 Hash = 1,000,000,000 `nhash`. 
{% endhint %}

Now, let's transfer 1`nhash` to the other address:

```bash
provenanced --testnet tx bank send tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm 1nhash \
 --node=tcp://rpc-0.test.provenance.io:26657 \
 --chain-id pio-testnet-1 \
 --gas 65000 
 --gas-prices 1nhash
```

The CLI will prompt us to confirm our transaction request:

```bash
{"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address":"tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs","to_address":"tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm","amount":[{"denom":"nhash","amount":"1"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[{"denom":"nhash","amount":"60000"}],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]:
```

Once confirmed, the transaction will be signed using the private key portion of our key pair associated to address `tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs` \(remember we stored our key in a local keyring\) and submitted to the blockchain:

```javascript
{
  "height": "211721",
  "txhash": "903A2DFC2E3178D4AB078306E357C28A7B6167F01181AC1A17F1CCC857202D13",
  "codespace": "",
  "code": 0,
  "data": "0A060A0473656E64",
  "raw_log": "[{\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"send\"},{\"key\":\"sender\",\"value\":\"tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs\"},{\"key\":\"module\",\"value\":\"bank\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm\"},{\"key\":\"sender\",\"value\":\"tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs\"},{\"key\":\"amount\",\"value\":\"1nhash\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "log": "",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "send"
            },
            {
              "key": "sender",
              "value": "tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs"
            },
            {
              "key": "module",
              "value": "bank"
            }
          ]
        },
        {
          "type": "transfer",
          "attributes": [
            {
              "key": "recipient",
              "value": "tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm"
            },
            {
              "key": "sender",
              "value": "tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs"
            },
            {
              "key": "amount",
              "value": "1nhash"
            }
          ]
        }
      ]
    }
  ],
  "info": "",
  "gas_wanted": "65000",
  "gas_used": "61579",
  "tx": null,
  "timestamp": ""
}
```

{% hint style="info" %}
Gas, gas, and more gas - a quick discussion on gas and fees.  Notice that the transaction response includes `gas_wanted` and a `gas_used` values.  When we submitted out transaction we indicated we were willing to pay 65,000 units of gas \(`--gas 65000`\) at 1 nhash per unit \(`--gas-prices 1nhash`\).  The actual cost of the transaction was 61,579 units of gas at our 1 nhash per unit price.  Refer to the [Cosmos Introduction to Gas and Fees](https://docs.cosmos.network/master/basics/gas-fees.html) and the [Provenance Gas and Fees ](gas-and-fees.md)section.
{% endhint %}

