---
description: >-
  Benefits, risks, and other considerations for network users that wish to
  become a validating node on the Provenance network.
---

# Validators

Validators perform the critical function of proposing and validating transactions on the Provenance network. A strong network of validators ensures Provenance security is maintained. Validators stake Hash™ to become part of the active validators on the network and are a foundational element for Hash™ holders that want to delegate their stake and share in rewards produced by the network's fee distribution framework. 

### Benefits

Validators participate in the security and control of the network and are able to earn additional staking rewards through commissions charged as well as bonuses for proposing transactions to be accepted by the network.

### Requirements

Hosting a Provenance validator has some requirements since stakeholders will likely not want to delegate their stake to a validator that isn't reliable and secure. Here are some guidelines to consider when determining whether hosting a validator is beneficial for your use case:

* Reliable, highly available, and secure computing infrastructure with excellent network connectivity
* Minimum Self-Delegation \(Hash™\) - To host a validator you have to ensure you have the minimum amount of Hash™ to delegate. 
* Minimum level of staking delegation - the top 50 validator nodes within the network by total stake are selected as the active set of validators
* Commitment to maintaining a current release of the blockchain software and perform software upgrades as required
* Commitment to vote on governance proposals

### Risks

Validators can earn rewards for assuming partial responsibility for the network, but this responsibility comes with some risks that should be considered before deciding to participate as a validator on the network. 

#### Uptime requirements

The provenance network requires that all active validators sign a minimum of XXXX blocks out of the last 10,000.  This requirement enforces a basic level of availability.  Active validators that are not available to sign blocks slow down the overall network response times.

#### Proper signing and endorsement

A validator node must only sign valid blocks and must not issue more than one signature per block.  These constraints are vital to the overall security of the network.

#### Penalties

Active validators that fail to meet the minimum availability requirements or are otherwise flagged for inappropriate behavior are subject to having a portion of their bonded stake slashed/burned.  This penalty enforcement ranges from XX% to XX% and includes a permanent ban for the most severe behavior.  Delegators that pledge stake to the validator node will also be penalized which is additional incentive to validators to protect their networks to maintain the stability of Provenance.

### Considerations

For the majority of stakeholders the most appropriate way to participate in the network and earn rewards is to delegate their stake to one of the existing validators

Securing the validator node key is extremely important and use of an HSM \(hardware security module\) is strongly recommended.  Note that at present no major cloud infrastructure providers have a compatible HSM solution that supports the required signing algorithm.

