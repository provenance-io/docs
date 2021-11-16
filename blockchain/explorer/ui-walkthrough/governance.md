# Governance

Governance of the blockchain doesn't happen off-chain. Every change to network must have consensus and approval of stakeholders on the chain itself. That's where governance proposals come in.&#x20;

More than just software upgrades, they can include new governance-restricted tokens, how to spend the community pool, and parameter updates to keep the blockchain up to date.&#x20;

See [here](../../../ecosystem/governance/#governance-proposal-types) for more detailed information about proposals.



## Listview

![A list of governance proposals on chain](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.40.02 PM.png>)

You can read more about proposals [here](../../../ecosystem/governance/). They allow stakeholders to vote on and enact changes to the blockchain that keep it updated and moving forward.

Information to note:

* **Id**: this is the unique ID of the proposal on chain
* **Deposit Percentage**: when a proposal is submitted, in order to be eligible for passing, it needs to be supported by a deposit of `hash`. There is a minimum amount required, but that can be surpassed if someone wants to add more
* **Submit Time**: the timestamp of when the proposal was submitted. Proposals have time limits on them, and they are calculated based on this initial submit time
* **Deposit End Time**: the proposal must have the minimum deposit amount applied by this timestamp, or else it fails automatically
* **Voting End Time**: the proposal must have enough votes accounted for to show a passing majority by this timestamp, or else it fails

## Detail

![An example of a proposal's detail](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.49.20 PM.png>)

The proposal should include details about whats it's supposed to enact upon the blockchain, as well as the programmatic details for anyone to look at and verify.

Information to note:

* **Type**: the type of proposal to be enacted. See [here](../../../ecosystem/governance/#governance-proposal-types-1) for some common types&#x20;
* **Description**: a description of what the proposal should do to the chain. Although proposals are usually enacted in good faith, its still a good habit to verify the description matches what the proposal actually does
* **Details**: the core of the proposal. This is the proposal object that contains the code changes that will be enacted upon the blockchain. Example:

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 11.00.10 PM.png>)

### Proposal Timing

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.49.38 PM.png>)

Reiterating what the timings on this proposal are, in case you missed them in the listview.

### Proposal Deposits

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.49.54 PM.png>)

The proposal can be submitted with an initial deposit, or additional deposits can be made against the proposal to bring the deposit amount up the minimum required. The minimum is required by the **Deposit End Time** as a prerequisite for the proposal passing.

### Proposal Votes

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 10.50.04 PM.png>)

Once the proposal is submitted, the voting period will open according to the schedule, allowing stakeholders (ie, holders of the blockchain's utility token) to vote on the proposal.&#x20;

A vote from a standard account applies their held stake to the vote bucket of choice. A vote from a validator applies their total delegated stake to the vote bucket of choice.

There are thresholds to be met for the proposal to pass or be vetoed.&#x20;

