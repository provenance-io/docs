---
description: >-
  Benefits, risks, and other considerations for network users that wish to
  become a validating node on the Provenance network.
---

# Become a Validator

* Benefits
  * Validators particpate in the security and control of the network
  * Validators can earn additional staking rewards though commissions and bonuses
* Requirements
  * Reliable, highly available, and secure computing infrastructure with excellent network connectivity
  * Minimum Self-Delegation \(hash\)
  * Minimum level of staking delegation \(the top 50 validator nodes within the network by total stake are selected as the active set of validators\)
  * Commitment to maintaining a current release of the blockchain software and perform software upgrades as required
  * Commitment to vote on governance proposals
* Risks
  * Uptime requirements
    * The provenance network requires that all active validators sign a minimum of XXXX blocks out of the last 10,000.  This requirement enforces a basic level of availablilty.  Active validators that are not available to sign blocks slow down the overall network response times.
  * Proper signing and endorsement
    * A validator node must only sign valid blocks and must not issue more than one signature per block.  These constraints are vital to the overall secuirty of the network.
  * Penalties
    * Active nodes that fail to meet the minimum availability requirements or are otherwise flagged for inappropriate behavior are subject to having a portion of their bonded stake slashed/burned.  This penalty enforcement ranges from XX% to XX% and includes a permanent ban for the most severe behavior.  Delegators that pledge stake to the validator node will also be penalized.
* Considerations
  * For the majority of stake holders the most appropriate way to participate in the network and earn rewards is to delegate their stake to one of the existing validators
  * Securing the validator node key is extremely important and use of a HSM \(hardware security module\) is strongly recommended.  Note that at present no major cloud infrastructure providers have a compatible HSM solution that supports the required signing algorthim.

