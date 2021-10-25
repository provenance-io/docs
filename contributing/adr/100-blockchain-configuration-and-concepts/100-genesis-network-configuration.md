---
description: >-
  Description of the contents of the genesis block, mainnet, testnet, and
  related configuration for connecting to the networks.
---

# 100 - Genesis Network Configuration

## Changelog

* 20200202: Initial version

## Context

There are many different settings and parameters that must be included in the initial genesis of the blockchain. These parameters range from Tendermint settings related to Consensus to the initial state of ledger applications.

## Decision

The initial structure of the blockchain will be as follows.

* [Consensus Parameters]()

### Consensus Parameters

```text
ConsensusParams {
    BlockSize {
        MaxBytes int = 22020096
        MaxGas int   = 10000000 // a huge amount... more than we need but a value so that Gas will be turned on
        TimeIOTAms   = 1000     // default?  Perhaps 500ms is better as a floor if a block filled early under load...
    }
    TxSize {
        MaxBytes int =
        MaxGas int   =
    }
    BlockGossip {
        BlockPartSizeBytes int 
    }
}
```

The `BlockPartSizeBytes` and the `BlockSize.MaxBytes` are enforced to be greater than 0. The former because we need a part size, the latter so that we always have at least some sanity check over the size of blocks.

[ADR-005-consensus-params](https://github.com/tendermint/tendermint/blob/master/docs/architecture/adr-005-consensus-params.md) [Tendermint Documentation](https://docs.tendermint.com/master/spec/abci/apps.html#consensus-parameters)

#### A Note on Gas and Fees

For Provenance Blockchain the accepted denom for fees is `vspn` :::

Transactions on Provenance Blockchain need to include a transaction fee in order to be processed. This fee pays for the gas required to run the transaction. The formula is the following:

```text
fees = ceil(gas * gasPrices)
```

The `gas` is dependent on the transaction. Different transaction require different amount of `gas`. The `gas` amount for a transaction is calculated as it is being processed, but there is a way to estimate it beforehand by using the `auto` value for the `gas` flag. Of course, this only gives an estimate. You can adjust this estimate with the flag `--gas-adjustment` \(default `1.0`\) if you want to be sure you provide enough `gas` for the transaction.

The `gasPrice` is the price of each unit of `gas`. Each validator sets a `min-gas-price` value, and will only include transactions that have a `gasPrice` greater than their `min-gas-price`.

The transaction `fees` are the product of `gas` and `gasPrice`. As a user, you have to input 2 out of 3. The higher the `gasPrice`/`fees`, the higher the chance that your transaction will get included in a block. You can use fee as a flat calculation for a transaction but the easiest method is to simply set a gas price and use `auto` for the gas flag.

### Genesis Validators

Per Hash specifications each of the initial member banks that run a validator node must stake 500,000,000hash against their node. This amount combined with the total of 12 validator nodes ensures that the total voting power will be at least 6000000000hash with each of the validator nodes maintaining the minimum required delegation. Additional delegations from hash holder are expected to slightly increase this amount of over time.

```text
Validators [
    GenesisValidator {
        Address = // ...
        PubKey  = // ...
        Power   = 500000000 // 500 million minimum staking requirement
        Name    = example   // colchis, dvone, ellington, experian, feb, kempner, lakestar, medalist, passport, prime, sandler, templeton
    }
]
```

For the genesis validator set we will issue a single node per existing member node. Within the Provenance Blockchain namespace we will run 3 nodes which will not be validators but will serve as interface points for internal and external blockchain applications to connect to. These nodes will bridge the internal validator network supported by Figure/Provenance Blockchain with the external public network. It is acceptable for a validator to run their own configuration of a protected validator network separately.

[Peer and Validator Networking](https://kb.certus.one/peers.html)

### Hash Auction \(Gas Fee Conversion\)

Currently, accredited investors who are Provenance Blockchain members can trade Hash directly with other members. Soon, these members are able to sell their Hash to the administrator as part of the Dutch Auction for fee distribution \(“the fee auction”\). Distributed fees are the primary driver of economic value of Hash.

In the fee auction, members set a reserve price for their Hash. For example, they may opt to sell 100 Hash at $1/Hash, 200 at $2, etc. Members are not obligated to set reserve prices. When a member pays a transaction fee on Provenance Blockchain, that fee is auctioned at the clearing reserve price. For example, if the fee was $1 and the best price for Hash is $1/Hash, that offer would get filled. The seller then gets the $1, and the Hash is distributed to the stakeholders, the administrator and the other Hash holders.

If the best offer cannot clear the auction, then the next best offer is taken and so on until the fee clears. For example, assume the fee is $1, and there is an offer of 1 Hash for $0.50 and 1 Hash for $1 by two separate members. In this case, the first offer won’t clear at $0.50, so the price moves to $1. All three offers sell 0.33 Hash at $1/Hash.

## Status

Proposed

## Consequences

> This section describes the resulting context, after applying the decision. All consequences should be listed here, not just the "positive" ones. A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

### Positive

{positive consequences}

### Negative

{negative consequences}

### Neutral

{neutral consequences}

## References

> A list of relevant PRs, issues that led up to this, or articles referenced for why we made the given design choice? A list of external reference links for any included above in \[markdown reference format\]\[ref-slug-or-number\]
>
> \[ref-slug-or-number\]: [http://example.org/reference/link](http://example.org/reference/link)

* {reference link}
* {markdown references}

