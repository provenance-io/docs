---
description: Provenance Blockchain transaction gas and fees.
---

# Gas and Fees

## Gas Definition

On Provenance Blockchain, [`gas` is a special unit that is used to track the consumption of resources ](https://docs.cosmos.network/main/basics/gas-fees.html)during transaction execution. `gas` is typically consumed whenever read and writes are made to a Provenance Blockchain state store, but it can also be consumed if expensive computation needs to be done. It serves two main purposes:

* Make sure blocks are not consuming too many resources and will be finalized. This is implemented by default via the [block gas meter](https://docs.cosmos.network/main/basics/accounts.html#block-gas-meter).
* Prevent spam and abuse from end-users. `gas` consumed during [`message`](https://docs.cosmos.network/main/building-modules/messages-and-queries.html#messages) execution is typically priced, resulting in a `fee` \(`fees = gas * gas-prices`\). `fees` generally have to be paid by the sender of the `message`.

## Gas Calculations

First, some terms:

**`gas`** an integer unit, based on transaction type and payload size, that is calculated for each transaction request

**`gas-prices`** a decimal unit, local to a node, that indicates the price per `gas` unit.  For example, `0.1nhash` per `gas` unit.

**`fees`** the cost to execute a transaction request where `fees = gas * gas-prices`. 

$$
fees = gas * gas prices
$$

Nodes set their own minimum gas prices.  When an account submits a transactions it spec/ifies that gas fees it is willing to pay.  So, for example, let's say we're submitting a transaction to `Node A`.  `Node A` has set their `gas-prices` to `0.025nhash`.  Given a transaction that requires 20,000 `gas` units, we will end up paying `fees` equal to `20,000 * 0.025nhash` or `500nhash`.

Provenance Blockchain will verify that the gas prices provided with the transaction is greater than the node's `min-gas-prices` \(as a reminder, gas-prices can be deducted from the following equation: `fees = gas * gas-prices`\). `min-gas-prices` is a parameter local to each full-node and used to discard transactions that do not provide a minimum amount of fees. This ensure that the blockchain is not spammed with garbage transactions.

When the end-user account generates a transaction, they must indicate 2 of the 3 following parameters \(the third one being implicit\): `fees`, `gas` and `gas-prices`. This signals how much they are willing to pay for nodes to execute their transaction.

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

Now, let's use the Provenance Blockchain Faucet to transfer Hash to our address:

```bash
curl 'https://test.provenance.io/blockchain/faucet/external'  \
  -X 'POST'  -H 'Content-Type: application/json'  \
--data-binary '{"address":"tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6"}'
```

And now, query our new Provenance Blockchain account's Hash balance:

```bash
provenanced --testnet query bank balances tp1hn42260zk29s8kfqy55pfzv0e2frvykvl886p6 \
  --node=https://rpc.test.provenance.io:443
```

```bash
balances:
- amount: "1000000000"
  denom: nhash
```



