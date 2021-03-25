---
description: Provenance transaction gas and fees.
---

# Gas and Fees

Gas is a consumable that is used to power the Provenance blockchain. Each execution of the blockchain requires enough gas to complete the requires reads, writes, and computation encompassed by the submitted transaction\(s\).  

{% hint style="info" %}
Refer to the [Fees/Distribution section](../../operations/distribution.md) for information on how the Provenance fee economy operates.
{% endhint %}

## Gas Definition

On Provenance, [`gas` is a special unit that is used to track the consumption of resources ](https://docs.cosmos.network/master/basics/gas-fees.html)during transaction execution. `gas` is typically consumed whenever read and writes are made to a Provenance state store, but it can also be consumed if expensive computation needs to be done. It serves two main purposes:

* Make sure blocks are not consuming too many resources and will be finalized. This is implemented by default via the [block gas meter](https://docs.cosmos.network/master/basics/accounts.html#block-gas-meter).
* Prevent spam and abuse from end-users. `gas` consumed during [`message`](https://docs.cosmos.network/master/building-modules/messages-and-queries.html#messages) execution is typically priced, resulting in a `fee` \(`fees = gas * gas-prices`\). `fees` generally have to be paid by the sender of the `message`.

## Gas Flow

When an end-user account submits a transaction request, they must indicate 2 of the 3 following parameters \(the third one being implicit\): `fees`, `gas` and `gas-prices`. This signals how much they are willing to pay for nodes to execute their transaction.

Provenance will verify that the gas prices provided with the transaction is greater than the node's `min-gas-prices` \(as a reminder, gas-prices can be deducted from the following equation: `fees = gas * gas-prices`\). `min-gas-prices` is a parameter local to each full-node and used to discard transactions that do not provide a minimum amount of fees. This ensures that the blockchain is not spammed with garbage transactions.

Provenance will use the transaction request gas parameters to determine if the transaction can be submitted.

![Conceptual Gas Flow](../../.gitbook/assets/image%20%2813%29.png)

The Conceptual Gas Flow diagram illustrates the transaction request gas calculations and the affect gas calculations have on the transaction request and the end-user account.

* A **Transaction Request** is a transaction built and signed by the end-user account.  The Transaction Request contains:
  * A **Gas** limit to use for the transaction
  * The maximum **Gas Price** the requestor is willing to pay.
  * A **Requested Fee** is calculated as **`Gas * Gas Price`**.
* The Transaction Request is submitted to a node, which in turn may submit it to other nodes until it reaches a validator node.  Each validator node sets their own **Minimum Gas Price.** 
* Provenance will validate that the requested **Gas Price** is &gt;= **Minimum Gas Prices** for all nodes involved.
  * If the requested **Gas Price** is too low, the transaction is rejected and an error is sent back to the requestor.
* Based on the transaction type and size, a **Gas Needed** amount is calculated by Provenance.
* Provenance calculates a **Required Fee** as `Gas Needed * Minimum Gas Price`.
* If the **Requested Fee** is &gt;= **Required Fee** the **Requested Fee** is deducted from the requestor's account and the transaction is submitted.
* If the **Requested Fee** is &lt; **Required Fee** the **Requested Fee** is deducted from the requestor's account, the transaction is marked as error, and an error is returned to the requestor.

### Gas Examples

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

Now, let's use the Provenance Faucet to transfer Hash to our address:

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

Next, let's estimate the `gas` requirements for a `1nhash` Hash transfer transaction from our account:

```bash
provenanced --testnet tx bank send tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 tp1qgjnuqnrqwhg2kfl0dk9rhkcga5lehns2hdycm 1nhash \
  --chain-id pio-testnet-1 \
  --gas 65000 \
  --gas-prices 1nhash \
  --dry-run
```

```bash
gas estimate: 66347
```

Now, let's set our transaction request gas parameters at that gas estimate:

```bash

```



