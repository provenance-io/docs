---
description: >-
  Benefits, risks, and other considerations for network users that wish to
  become a validating node on the Provenance Blockchain network.
---

# Validator

Validators perform the critical function of proposing and validating transactions on the Provenance Blockchain network. A strong network of validators ensures Provenance Blockchain security is maintained. Validators stake Hash™ to become part of the active validators on the network and are a foundational element for Hash™ holders that want to delegate their stake and share in rewards produced by the network's fee distribution framework. 

### Requirements

Hosting a Provenance Blockchain validator has some requirements since stakeholders will likely not want to delegate their stake to a validator that isn't reliable and secure. Here are some guidelines to consider when determining whether hosting a validator is beneficial for your use case:

* Reliable, highly available, and secure computing infrastructure with excellent network connectivity.
* Minimum Self-Delegation \(Hash™\) - To host a validator you have to ensure you have the minimum amount of Hash™ to delegate. 
* Minimum level of staking delegation - the top 50 validator nodes within the network by total stake are selected as the active set of validators.
* Commitment to maintaining a current release of the blockchain software and perform software upgrades as required.
* Commitment to vote on governance proposals.

### Benefits

Validators participate in the security and control of the network and are able to earn additional staking rewards through commissions charged as well as bonuses for proposing transactions to be accepted by the network. 

#### Commissions

Each Provenance Blockchain validator can set a commission rate to charge on fees collected. This directly allows validators to charge up to 100% of the fees received during distribution. However, any validator choosing to charge a 100% commission is highly unlikely to receive delegate Hash. 

#### Bonus Proposer Awards

Active validators are each given a turn to propose a block to be committed using a round-robin assignment. During the proposal process the validator selected earns additional rewards based on being the proposer as well as awaiting all possible signatures.

#### Voting 

Voting within the Provenance Blockchain ecosystem allows Hash holders to direct the development of the network. When governance proposals are being voted upon, validators will vote with the full weight of not only the validator's Hash, but the delegator's Hash as well, if those delegators haven't voted. Since delegators will not always vote for proposals on the network, this gives validator's more voting power to determine the direction of Provenance Blockchain. 

#### Gas Price

Based on 2/3s majority, validators can choose the minimum gas price that they will accept in order to process a proposed transaction. This allows validators to organically modify the cost to use the network.

### Responsibilities in Hosting a Validator

Validators can earn rewards for assuming partial responsibility for the network, but this responsibility comes with some risks that should be considered before deciding to participate as a validator on the network. 

#### Uptime requirements

The provenance network requires that all active validators sign a minimum of XXXX blocks out of the last 10,000.  This requirement enforces a basic level of availability.  Active validators that are not available to sign blocks slow down the overall network response times.

#### Proper signing and endorsement

A validator node must only sign valid blocks and must not issue more than one signature per block.  These constraints are vital to the overall security of the network. [https://blog.cosmos.network/the-4-classes-of-faults-on-mainnet-bfabfbd2726c](https://blog.cosmos.network/the-4-classes-of-faults-on-mainnet-bfabfbd2726c)

#### Penalties

Active validators that fail to meet the minimum availability requirements or are otherwise flagged for inappropriate behavior are subject to having a portion of their bonded stake slashed/burned.  This penalty enforcement ranges from XX% to XX% and includes a permanent ban for the most severe behavior.  Delegators that pledge stake to the validator node will also be penalized which is additional incentive to validators to protect their networks to maintain the stability of Provenance Blockchain.

### Considerations

For the majority of stakeholders the most appropriate way to participate in the network and earn rewards is to delegate their stake to one of the existing validators

Securing the validator node key is extremely important and use of an HSM \(hardware security module\) is strongly recommended.  Note that at present no major cloud infrastructure providers have a compatible HSM solution that supports the required signing algorithm.

