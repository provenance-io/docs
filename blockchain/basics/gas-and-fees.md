---
description: Provenance transaction gas and fees.
---

# Gas and Fees

Gas is a consumable that is used to power the Provenance blockchain. Each execution of the blockchain requires enough gas to complete the requires reads, writes, and computation encompassed by the submitted transaction\(s\).  

{% hint style="info" %}
Refer to the [Fees/Distribution section](../../ecosystem/financial-services-blockchain/distribution.md) for information on how the Provenance fee economy operates.
{% endhint %}

{% hint style="info" %}
For an in-depth understanding of the transaction lifecycle and how and when a Cosmos SDK-based application does fee checks refer to [Transaction Lifecycle.](transaction-lifecycle.md)
{% endhint %}

Fees limit the growth of the state stored by every full node and allow for general purpose censorship of transactions of little economic value. Fees are best suited as an anti-spam mechanism where validators are disinterested in the use of the network and identities of users.

Fees are determined by the gas limits and gas prices transactions provide, where`fees = ceil(gasLimit * gasPrices)`. Transactions incur gas costs for all state reads/writes, signature verification, as well as costs proportional to the transaction size \(this is known as `gas needed`\). Node operators set minimum gas prices when starting their nodes. If a minimum gas price is not set, `provenanced` defaults to `0.025nhash`.

When adding transactions to the [mempool or gossipping transactions](transaction-lifecycle.md), nodes check if the transaction's gas prices, which are determined by the provided fees, meet the node's minimum gas prices.

Thus, on Provenance Blockchain, [`gas` is a special unit that is used to track the consumption of resources ](https://docs.cosmos.network/master/basics/gas-fees.html)during transaction execution to ensure:

* Blocks are not consuming too many resources and will be finalized.
* No spam or abuse from end-users.

## Gas Flow

When an end-user account submits a transaction request, they must indicate two of the three following parameters \(the third one being implicit\): `fees`, `gas`, and `gas-prices`. This signals how much they are willing to pay for nodes to execute their transaction.

Provenance will verify that the gas prices provided with the transaction is greater than the node\(s\) `min-gas-prices` \(as a reminder, gas-prices can be calculated from the following equation: `fees = gas * gas-prices`\). `min-gas-prices` is a parameter local to each full-node and used to discard transactions that do not provide a minimum amount of fees. This ensures that the blockchain is not spammed with garbage transactions.

![Conceptual Gas Flow](../../.gitbook/assets/image%20%2813%29.png)

The Conceptual Gas Flow diagram illustrates the transaction request gas calculations and the affect gas calculations have on the transaction request and the end-user account.

* A **Transaction Request** is a transaction built and signed by the end-user account.  The Transaction Request contains:
  * A **Gas** limit to use for the transaction
  * The maximum **Gas Price** the requestor is willing to pay.
  * A **Requested Fee** is calculated as **`Gas * Gas Price`**.
* The Transaction Request is submitted to a node, which in turn may submit it to other nodes until it reaches a validator node.  Each node sets their own **Minimum Gas Price.** 
* Provenance will validate that the requested **Gas Price** &gt;= **Minimum Gas Prices** for all selected nodes involved.
  * If the requested **Gas Price** is too low, the transaction is rejected and an error is sent back to the requestor.
* Based on the transaction type and size, a **Gas Needed** amount is calculated by Provenance Blockchain.
* Provenance calculates a **Required Fee** as `Gas Needed * Minimum Gas Price`.
* If the **Requested Fee** &gt;= **Required Fee** the **Requested Fee** is deducted from the requestor's account and the transaction is submitted.
* If the **Requested Fee** &lt; **Required Fee** the **Requested Fee** is deducted from the requestor's account, the transaction is marked as error, and an error is returned to the requestor.

### Sample Scenarios

The following table illustrates sample gas scenarios, whether the transaction is submitted, and the net affect on the account's balance:

| Txn | Gas | Gas Price | Fee | Gas Needed | Min Gas Price | Req Fee | Result | Beg Bal | Fee Paid | End Bal |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 5,000 | 0.0025 | 12.5 | 5,000 | 0.025 | 125 | **Error: Gas Price &lt; Min Gas Price** | 100,000 | **0** | 100,000 |
| 2 | 5,000 | 0.025 | 125 | 5,000 | 0.025 | 125 | Success: Submitted | 100,000 | 125 | 99,875 |
| 3 | 5000 | 1.000 | 5000 | 6,000 | 0.025 | 150 | **Error: Gas &lt; Gas Needed** | 99,875 | **5,000** | 94,875 |
| 4 | 10,000 | 0.025 | 250 | 8,000 | 0.025 | 200 | Success: Submitted | 94,875 | 250 | 94,625 |
| 5 | 8,000 | 0.05 | 400 | 6,000 | 0.075 | 450 | **Error: Gas Price &lt; Min Gas Price** | 94,625 | **0** | 94,625 |
| 6 | 6,000 | 0.1 | 600 | 6,000 | 0.075 | 450 | Success: Submitted | 94,625 | 600 | 94,025 |

## Demonstration of Gas and Fees

{% hint style="info" %}
To follow along with this section refer to [Installing Provenance](../running-a-node/) to install the `provenanced` binary.
{% endhint %}

Let's try this out.  First, let's create a local key pair and address:

```bash
provenanced --testnet keys add gas_example
```

```bash
- name: gas_example
  type: local
  address: tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6
  pubkey: tppub1addwnpepqgnpa42wk4gtq8lllvf2t9aegsa6550zvamhfgfn74ga5aelluunyjkp9vv
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

Now, let's use the [Provenance Faucet](https://explorer.test.provenance.io/faucet) to transfer Hash to our address:

```bash
curl 'https://test.provenance.io/blockchain/faucet/external'  \
  -X 'POST'  -H 'Content-Type: application/json'  \
--data-binary '{"address":"tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6"}'
```

And now, query our new Provenance account's Hash balance:

```bash
provenanced --testnet query bank balances tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 \
  --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
balances:
- amount: "1000000000"
  denom: nhash
```

{% hint style="info" %}
What is `nhash`?  `nhash` is the smallest unit of Hash, a nano-Hash, where 1 Hash = 1,000,000,000 `nhash`.   
{% endhint %}

Next, let's estimate the `gas` requirements for a `1nhash` Hash transfer transaction from our account:

```bash
provenanced --testnet tx bank send tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm 1nhash \
  --node=tcp://rpc-0.test.provenance.io:26657
  --chain-id pio-testnet-1 \
  --gas 65000 \
  --gas-prices 1nhash \
  --dry-run
```

```bash
gas estimate: 63094
```

Let's set our `gas-price` too low and see the affect on our account `nhash` balance:

```bash
provenanced --testnet tx bank send tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm 1nhash \
  --node=tcp://rpc-0.test.provenance.io:26657 \
  --chain-id pio-testnet-1 \
  --gas 65000 \
  --gas-prices 0.0025nhash
```

```bash
{
  "height": "0",
  "txhash": "F92BA2CA52E11009689A61518E1F1C9941FCCF86FC65DF0A11F385AA408D546B",
  "codespace": "sdk",
  "code": 13,
  "data": "",
  "raw_log": "insufficient fees; got: 163nhash required: 1625nhash: insufficient fee",
  "logs": [],
  "info": "",
  "gas_wanted": "65000",
  "gas_used": "0",
  "tx": null,
  "timestamp": ""
}
```

This had no affect on our account balance:

```bash
provenanced --testnet query bank balances tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 \
  --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
balances:
- amount: "1000000000"
  denom: nhash
```

Now, let's set our `gas-price` to a value equal to the `min-gas-prices` for the nodes but set our `gas` lower than the gas estimate:

```bash
provenanced --testnet tx bank send tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm 1nhash \
  --node=tcp://rpc-0.test.provenance.io:26657 \
  --chain-id pio-testnet-1 \
  --gas 60000 \
  --gas-prices 0.025nhash  
```

```bash
{
  "height": "270671",
  "txhash": "930CE3180CF6ACB09D5BF4E0B7BC3900E2F81FFE43FA9CCAC8B030294C20B13E",
  "codespace": "sdk",
  "code": 11,
  "data": "",
  "raw_log": "out of gas in location: WritePerByte; gasWanted: 60000, gasUsed: 60071: out of gas",
  "logs": [],
  "info": "",
  "gas_wanted": "60000",
  "gas_used": "60071",
  "tx": null,
  "timestamp": ""
}
```

Notice that the `gas_wanted` is lower than the `gas_used` and as a result, the transaction returned an error.  Also notice that the `gas_used` price fluctuated from our original `gas estimate` - required gas prices will fluctuate.

Also notice that even though we received an error response, the transaction was submitted \(marked as error\) and our account was charged the a fee of `1,500` for the `gas_wanted` amount where `1,500(fee) = 60,000 (gas) * 0.025 (gas-price)`

```bash
provenanced --testnet query bank balances tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 \                                          
  --node=tcp://rpc-0.test.provenance.io:26657 
```

```bash
balances:
- amount: "999998500"
  denom: nhash
pagination:
  next_key: null
  total: "0"
```

## Why are Accounts Charged a Fee When a Transaction Runs Out of Gas?

In the previous section, the account was charged a fee even though there was not enough gas to cover the gas needed.  Here's why - the active validator begins a new block by selecting a transaction from the mempool and then:

* Checks to see that the signature is valid and that the account has enough funds to cover the transaction. 
  * It also verifies the sequence number is correct \(no other transactions have been processed ahead of this one\).
* Debits the amount of fees assigned to the transaction.
* Runs the transaction and all the messages inside inside it.
* If any message fails the transaction fails and is recorded as a failed transaction \(note the gas fee was already removed from senders account\).
* If all messages in the transaction succeed, the successful transaction is recorded in the block and all state changes are accepted for the proposed block.
* When the block is ready/cut is goes through consensus for commitment. When enough votes are received from other validator nodes the block is passed and the next validator takes over based on the calculations within the block on validator priority.

