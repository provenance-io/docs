# Staking (Validators)

The **Staking** page shows all validators on the Provenance Blockchain. They are filtered by **Active**, **Candidate**, and **Jailed**.&#x20;

![Validator list, defaulted to "Active"](<../../../../.gitbook/assets/Screen Shot 2021-11-15 at 2.57.39 PM.png>)

The Provenance Blockchain has a parameter that defines the size of the **Active** validator set. An **Active** validator is:

* Bonded;
* Signing transactions;
* Participating in consensus;
* In the top X number of validators by voting power, where X is the size of this active validator set as defined by the blockchain

A **Candidate** validator is:

* Either:
  * Bonded;
  * Just outside the size of the active validator set
* Or:
  * Unbonded
* Or:&#x20;
  * Recovering from being **Jailed**.

A **Jailed** validator has:

* Participated in bad behavior, earning them a slashing penalty;
* Missed too many blocks in consensus voting, earning them a slashing penalty

## Active Validators

These are the validators that are actively processing transactions to add to the blockchain ledger.&#x20;

Information to note:

* **Commission**: this is the percentage of a block's fees taken by the validator before the remaining is distributed to delegators. See[ ](../../../../ecosystem/financial-services-blockchain/distribution.md)[#distribution](../../../../ecosystem/financial-services-blockchain/distribution.md#distribution "mention") for more information. Ideally, the lower the Commission, the better for delegators
* **Voting Power**: based on the percentage of total delegation on the blockchain, the higher the Voting Power, the more influence the validator will have on the blockchain
* **Uptime**: the percentage of blocks the validator has be "up" since they were added to the validator set. 100% = the validator has missed a negligible amount of or no blocks
* **Self Bonded: **the amount of Hash the validator's **Operator Account** has delegated to the validator. This shows the operator has skin in the game

## Candidate and Jailed Validators

Information to note:

* **Unbonding Height**: the block height where the validator started the unbonding process, either due to being jailed, or being moved out of the **Active** validator set

