---
description: An overview of validator rewards for the Provenance Blockchain
---

# Token Economics

Provenance is a public **proof-of-stake blockchain** network designed and developed to support financial service industry needs by providing a **ledger**, **registry**, and **exchange** across multiple financial assets and markets. **Hash** is the Provenance utility token to stake, govern, secure, and transact on the network.

Large financial institutions, investors, technical leaders, and individuals provide the foundation of security and stability of the Provenance network by staking Hash as a network validator or delegator for which they are rewarded with a fee. The amount of Hash staked towards a validator defines the frequency by which the validator may propose a new block and its weight in votes to commit a block \(and thus earn fees\). The n largest validators \(by weight of stake\) are defined as the Active Set of Validators where n is a number defined by the Hash community.

| Token Type | Hash |
| :--- | :--- |
| Total Supply | 100 Billion |
| Total Planned Inflation | None |
| Inflation Rate | None |
| Maximum Stake | None though an individual node is capped at 33% of total network staked weight |
| Minimum Stake | 1 Hash |
| Unbonding/Undelegating period | 21 days |
| Initial Active Set Size | 20 Validators \(can be changed by governance vote\) |
| Community pool | 2% of fees |

## Provenance Consensus Method

Provenance blockchain uses the Tendermint consensus mechanism. This relies on a set of validators that are responsible for committing new blocks to the chain. These Validators participate in the consensus protocol by broadcasting votes which contain cryptographic signatures signed by each validator's private key. Validators:

* Are full blockchain nodes
* Vote on blockchain network governance proposals
* Add blocks to the chain

There are a known and limited number of validators in the Active Set - **NV.** This set is viewable ****at [https://explorer.provenance.io](https://explorer.provenance.io/dashboard) The size of the active set is determined by community governance vote. The **NV** largest validators by staked token weight are those included in the Active Set. 

$$
N_V
$$

## Staking Hash

The Provenance blockchain network is a public Proof-Of-Stake \(PoS\) blockchain, meaning that the weight of validators is determined by the amount of staking tokens \(Hash\) bonded to them as collateral. These Hash can be self-delegated directly by the validator or delegated to them by other Hash holders. Hash holders can select any number of validators to delegate some or all of their Hash stake to. Any Hash holder can declare their intention to become a validator by sending a create-validator transaction to the network. From there, they become validator candidates.

The weight \(i.e. voting power\) of a validator determines whether or not they are an active validator. Only the top **NV** validators with the most voting power will be active validators - those in the Active Set. Only validators in the Active Set, and by association those who delegated to them, receive rewards and have the right to participate in governance.

When a validator or a delegator wants to remove part or all of their deposit from the staking pool \(i.e. no longer stake that Hash\), they send an unbonding transaction to the Provenance Blockchain. Their Hash undergoes a three week unbonding period during which the owner of the Hash still earns transaction fees, can participate in governance votes and is still liable to being slashed for potential misbehaviors committed by the validator before the unbonding process started.

If a delegator only wishes to move their stake between validators but not remove it from the staking pool they may issue a redelegation. A redelegation is an instant transfer in voting power from one validator to another that occurs at the submitted blockheight. Redelegations do not have any impact on the required 3 week unbonding period.

### Provenance slashing policy

Staking Hash can be thought of as a security deposit on validation activities. If a validator misbehaves, their delegated stake will be partially slashed. This means that every delegator that bonded Hash to this validator gets penalized in proportion to their bonded stake. Delegators are therefore incentivized to delegate to validators that they anticipate will function safely.

There are currently two faults that can result in slashing of funds for a validator and their delegators:

* Double signing: If someone reports on chain A that a validator signed two blocks at the same height on chain A and chain B, and if chain A and chain B share a common ancestor, then this validator will get slashed by 5% on chain A.
* Downtime: If a validator misses more than 99% of the last 10.000 blocks, they will get slashed by 0.01%.

When a validator node is slashed for one of the above penalties it is immediately removed from the active validator set and placed in a "jailed" state. A node may remove itself from the "jailed" status after a minimum cooling off period elapses by submitting an unjail transaction. During the jailed status period any delegator may choose to redelegate their stake to another validator on the network with the exception of the validator operator who must maintain at least their minimum stake threshold in their node.

## Validator rewards

Validators are rewarded for their work. The validator that is selected to propose the next block on the chain is called the **proposer**. Each proposer is selected deterministically and the **frequency of being chosen is proportional to the voting power** \(i.e. amount of bonded Hash\) of the validator. For example, if the total bonded stake across all validators is 100 Hash and a validator's total stake is 10 Hash, then this validator will be proposer for around 10% of the blocks.

A validator earns transaction fees in Hash tokens. The total fee for each transaction is divided among validators' staking pools according to each validator's weight. Within each validator's staking pool, the revenue is further divided among delegators in proportion to each delegator's stake. A commission on delegators' revenue is applied by the validator before it is distributed.

### Validator commission

Revenue received by a validator's pool is split between the validator and their delegators. The validator can apply a commission on the part of the revenue that goes to their delegators. This commission is set as a percentage. Each validator is free to set their initial commission, maximum daily commission change rate and maximum commission. The Provenance blockchain network enforces the parameters that each validator sets. Only the commission rate can change after the validator is established.

The commission-rate value **Ci**:

* Must be between 0 and the validator's commission-max-rate **Ci,max** 
* Must not exceed the validator's commission-max-change-rate **Ci,max-change** which is maximum percent change rate **per day**. In other words, a validator can only change its commission once per day and within commission-max-change-rate bounds.
* Can be 100%, which implies that that validator is not welcoming outside delegators since it pockets all rewards based on its bonded stake.

### Validator earned fees

Fees are similarly distributed with the exception that the block proposer can get a bonus on the fees of the block they propose if they include more than the strict minimum of required precommits.

When a validator is selected to propose the next block, they must include at least 2/3 precommits of the block. However, there is an incentive to include more than 2/3 precommits in the form of a bonus. The bonus is linear: it ranges from 1% if the proposer includes 2/3rd precommits \(minimum for the block to be valid\) to 5% if the proposer includes 100% precommits. Of course the proposer should not wait too long or other validators may timeout and move on to the next proposer. As such, validators have to find a balance between wait-time to get the most signatures and risk of losing out on proposing the next block. This mechanism aims to incentivize non-empty block proposals, create better networking between validators and mitigate censorship.

To summarize earned fee rules:

* **proposer is deterministically chosen from validators in the Active Set with frequency proportional to its total stake**
* **the proposer gets a 1-5% bonus of its normal share based on included extra precommiters above 2/3rd voting power** 
* **all validators in the Active Set receive a share of the fee proportional to their total stake**
* **Validators takes commissions before distributing fees to their delegators**

### Fee calculation example

To better illustrate how validator rewards work, let us assume there are 10 validators in the Active Set and all have an equal stake. Each of the validators applies a 10% commission rate and has 20% of self-delegated Hash. Assume there is a successful block added to the chain and a total of 1025.51020408 Hash was collected in fees.

First, 2% of the fee goes to the Provenance Foundation’s community pool. The community pool is used to fund the foundation’s day-to-day operations as well as the grant program.

* 2% \* 1025.51020408 = 20.51020408 Hash go to the community pool.

1005 Hash now remain. Assume that the proposer included 100% of the precommitters’ signatures in its block. The proposer thus obtains the full bonus of 5%. The reward R for each validator in the Active Set is determined by this simple equation:

* 9_R + R + R_5% = 1005 ⇔ R = 1005/10.05 = 100

For the proposer validator:

* The pool for that proposer obtains R + R \* 5%: 105 Hash
* Commission: 105  _80%_  10% = 8.4 Hash
* Validator's reward: 105 \* 20% + Commission = 29.4 Hash
* Delegators' rewards: 105 \* 80% - Commission = 75.6 Hash \(each delegator will be able to claim its portion of these rewards in proportion to their stake\)

For each of the other 9 non-proposer validators:

* The pool for the validator obtains R: 100 Hash
* Commission: 100  _80%_  10% = 8 Hash
* Validator's reward: 100 \* 20% + Commission = 28 Hash
* Delegators' rewards: 100 \* 80% - Commission = 72 Hash \(each delegator will be able to claim their portion of these rewards in proportion to their stake\)

### Initial validator incentive

The network will provide an additional incentive for validators for the first six months after mainnet launch. This incentive will be 5% of the transaction fee - generally increasing validator returns by 50% assuming an average commission rate of 10%.

The way this will be administered is the community pool charge in the first six months will be 7% rather than the normal 2%. This 5% will then be distributed to all validators in the Active Set.

## Inflation \(none\)

The number of minted Hash remains constant, and no new Hash is minted as reward for work. As a result, there is no inflation of the Hash value through the new influx of Hash.

## Gas and transaction fees

Transactions on the Provenance network need to include a transaction fee in order to be processed. This fee pays for the gas required to run the transaction. The formula is the following:

* fees = ceil\(gas \* gasPrices\)

The amount of gas needed depends on the transaction as different types of transactions \(e.g. making a payment or loading a loan\) require different amounts of resources to process. The gas amount for a transaction is calculated as it is being processed, but there is a way to estimate it beforehand by using the auto value for the gas flag. Users can adjust this estimate with the flag --gas-adjustment \(default 1.0\) if they want to be sure to provide enough gas for the transaction.

The gasPrice is the price of each unit of gas. Transaction fees are the product of gas and gasPrice. Users must include the fee being paid and the rate the gas will be converted with each transaction they send to the network. Too low a gasPrice or gas could result in the transaction not being included at all.

Each validator sets a min-gas-price value, and will only include transactions that have a gasPrice greater than their min-gas-price. These full-nodes keep unconfirmed transactions in their mempools. In order to protect nodes from spam, it is best for validators to set a min-gas-price that the transaction must meet in order to be accepted in their node's mempool. This parameter can be set as a flag in the file %USER\_OS\_CONFIG\_DIR%/Provenance/config/app.toml or as a parameter in the `provenanced` process. \

At mainnet launch the recommended gasPrice is 1904 nHash where 1Hash = 1,000,000,000nHash

More details about gas:

* Validators set their own respective minimum gas prices. This value can range from infinitesimally small \(inexpensive\) to relatively large \(expensive\).
* When a transaction is broadcast to a node, it is run through the given validator's CheckTx function. If this passes, the transaction is added to the node’s mempool and broadcast to other nodes and validators.
* This transaction will sit in the node’s mempool until it is accepted by a proposer. 
* The transaction will be accepted by a proposer if it meets the given proposer's minimum gas price. 
* If the transaction’s gas price is below the amount required by the current proposer, the transaction stays in the mempool until another proposer accepts it \(in the worst case this would be the validator that initially accepted the transaction when that validator is selected as the proposer\).

