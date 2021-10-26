# Delegator

To understand delegation it's important to understand the difference between Hash ownership and Hash staked on the Provenance Blockchain network.

**Hash** - Utility token used by Provenance Blockchain for staking and transaction fee payment. 

**Stake** - Hash that has been delegated to a validator to share in the risks and rewards of participating in the PoS consensus mechanism on the blockchain network. 

Hash will become stake at the point in time that it is delegated to a validator to be used as voting power on the network. 

**Delegation** - staking Hash with a validator to be used by that validator as voting power. For most holders of Hash delegation is an easy and relatively safe method of sharing in the ownership and rewards of the network.

_See_ [_Voting_](../governance/voting.md) _for more information on how to vote on_ [_Governance_](../governance/) _proposals._

**Bonded** - The state of your Hash at the time when it is delegated to a validator. All bonded Hash is required to go through a 21 day unbonding period that locks the usage of the portion of Hash that has been removed from delegation. During this lock-up period:

* you won't share in the risks or rewards of the network
* your Hash won't be available for transacting
* your Hash will be safe from malfeasance by a bad acting validator.

**Re-delegation** - Re-delegation of Hash to a different validator occurs immediately, without incurring the 21 day unbonding period. This is incredibly important because it allows the Hash holder to secure their Hash with a trusted validator if the current one has been deemed untrusted.

Re-delegation is also the recommended way to move your stake from a validator that has or is going off line for extended maintenance to avoid an availability penalty being assessed against your stake.

### Rewards

#### Commission

Validators are able to set a commission percentage that directly impacts the amount of rewards you are able to collect from delegating to that validator. 

#### Voting

Once you have staked Hash you are able to vote on Governance proposals that ultimately determine the direction of the network's development, usage, security, etc... It can't be stressed enough how important staking Hash is to the Provenance Blockchain network. This is the true power of a proof-of-stake blockchain and allows the the network to operate independent of any controlling entity.

#### Fee Distributions

Staked Hash entitles you to a portion of the fees collected by the network. Care should be taken to delegate to a secure validator and understanding the commission rate that the validator charges since it will directly impact the amount of rewards that you are able receive. See [Distribution](../financial-services-blockchain/distribution.md) for more information on how fees are collected and distributed.

### Risks

#### Unbonding

Bonded Hash can't be immediately transacted due to the unbonding period. When Hash is unbonded from a validator it is held by the network until the unbonding period \(21 days\) has completed. Hash can be re-delegated to a different validator immediately to mitigate the risk of a bad-actor validator causing slashing of your stake.

#### Slashing

In order to keep validators accountable for running the blockchain securely the network will slash stake in certain scenarios. It is good to note that delegators share in this risk because they too have Hash staked on Provenance Blockchain. 

