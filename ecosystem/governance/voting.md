# Voting

Proposals are initiatives put to vote by stake holders to perform actions using a consensus of votes. This process is called governance. The proposals themselves may be simple statements (text proposals) or initiatives to change configuration parameters (configuration change proposals), software upgrades, and more.

Voting is the process of assigning active stake to one of the four different states for a proposal \[yes, no, abstain, veto]. Only actively staked hash can be used for a vote. Each actively staked Hash corresponds to one vote.\
\
The governance voting process is broken down into three stages; Deposit, Voting, and Tallying results.  Proposals must clear the deposit stage before they can proceed to voting and finally have their results tallied.

### Deposit

Anyone can submit a proposal to the Provenance network for stake holders to review.  In order for the proposal to proceed to a vote it must meet the minimum required deposit.  The minimum deposit required is 1,000 HASH.  A proposal has up to 48 hours to collect enough deposits to become eligible for voting. A proposal will immediately proceed to voting when the threshold is met.  If the proposal does not achieve quorum or the proposal is vetoed the deposits are forfeit.

### Voting Period

The voting period for a governance proposal is fixed at 48 hours.  During this time delegators and validators may place their votes of Yes, No, Abstain, or No with Veto.  When a validator votes the total of their delegations is applied as weight to their choice.  Their individual delegators can accept the vote of the validator they have staked against or explicitly vote their intention which will take precedence over the validator's vote.

### Tallying Votes

At the end of the voting period the cast votes are tallied according to the following thresholds to determine if a proposal passes or not.

* Quorum: At least 33.3% of the active state in the network must vote on a proposal for the vote to be considered valid.  If quorum is not reached the proposal fails
* Threshold: Of the votes cast, at least 50% must be `Yes` votes for the proposal to pass
* No with Veto: If more than 33.3% of the votes are cast as a No with Veto the measure fails regardless of if over 50% of the votes were`Yes`.  This last option allows a minority stakeholder to prevent a measure from passing even if the majority endorses it.

If the majority of a proposal vote exceeds quorum and is not vetoed then the proposal is passed at the end of the voting period.  For certain proposals such as software upgrades their effects are applied at this time.

