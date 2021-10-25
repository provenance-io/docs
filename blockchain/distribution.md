---
description: >-
  Explanation of how fees are collected and distributed among the many network
  participants.
---

# Fees/Distribution

## What is gas? 

Gas is a consumable that is used to power the Provenance blockchain. Each execution of the blockchain requires enough gas to complete the requires reads, writes, and computation encompassed by the submitted transaction\(s\). When using gas you need to know two things:

* How much gas do I need to process my transaction? 
* What is the current price of gas?

Before each transaction invoked on Provenance an estimate of the amount gas you need is made. The estimated gas needed is the maximum you'll pay for the given transaction. Over estimating will lead to purchasing more gas than needed and under estimating will cause the submitted transaction to fail due to running out of gas. 

### Meter 

When a transaction starts processing the meter is set at to the estimated amount of gas and the measurement of usage decreases the amount of available gas for the transaction based on network usage. If the transaction completes without running out the meter it will be committed to the blockchain. If the meter arrives at zero before the transaction is complete the blockchain will reject the transaction and the gas has been consumed. In this case it is beneficial to be slightly higher on gas estimates to avoid paying for a transaction that is rejected.

### Price

Gas is purchased with Hash, the Provenance utility token. 

### Operation Gas Estimates

| Operation | Low | High |
| :--- | :--- | :--- |
| transfer coins | 70000 | 70000 |
| name\_bound | 64000 | 64000 |
| account\_attribute\_added | 70000 | 70000 |
| account\_attribute\_deleted | 55000 | 55000 |
| contract spec loaded | 2800000 | 3600000 |
| scope created | 233000 | 1000000 |
| scope updated | 90000 | 110000 |
| scope change owner | 75000 | 75000 |
| marker created | 75000 | 75000 |
| marker supply | 60000 | 60000 |
| marker permissions | 60000 | 60000 |
| marker finalize | 60000 | 60000 |
| marker activate | 100000 | 100000 |
| marker mint | 85000 | 90000 |
| marker burn | 85000 | 90000 |
| bilateral exchange | 105000 | 105000 |
| multilateral exchange | 105000 | 1050000 |
| smart contract create \(Require governance vote\) | 6320000 | 20000000 |
| smart contract init | 1000000 | 1000000 |
| smart contract exec | 140000 | 140000 |

## Fees 

Fees are the total amount of Hash collected for consumed gas during the processing and minting of a block on Provenance Blockchain. **Community** and **Block Proposer** fees are set by governance proposals allowing Hash holders to vote to adjust these fee percentages as necessary. Delegators can choose validators that have a lower commission to optimize the amount of fees collected from the network. 

## Distribution

All fees that are collected by the validator network are distributed to rewarding Provenance participants for providing value in the various [Roles]().

### Fee Distribution Hierarchy

* Community - pool of Hash that can be used to enhance and maintain Provenance
* Block Proposer - an additional bonus given to the validator that proposed the block
* Validator Commission - percentage of the fee that is taken as commission by a validator
* Delegators 

### Current Fees

| Type | Percentage |
| :--- | :--- |
| Community | 5% |
| Block Proposer | 1% to 6% |
| Validator Commission | 5% to 100% |
| Delegators | Remaining |

### **Example 1**

Here is an example distribution of Hash collected during a transaction. 

**Validators:**

* Validator A - 20% commission
* Validator B - 50% commission
* Validator C - 1% commission

A proposed transaction uses 1000 gas and each unit of gas costs 0.5 Hash. In this scenario we can expect the network to collect 500 Hash in fees that need to be distributed. When the distribution takes place it goes through the fee distribution process and allocates: 

* **Community** would receive 5% of the total transaction fees \(500 x .05\) or 25 Hash.
* **Validator A** proposed the block and receives a 6% bonus because all validator signatures were collected. The remaining Hash available after the community pool allocation was 475 Hash. **Validator A** would collect \(475 \* .06\) or 28.5 Hash.
* Validators now receive the remaining 446.5 Hash split among them or 148.333 Hash each. 
* **Validator A** charges a commission of 20% - \(148.333 \* .2\) or 29.6666 Hash 
* **Validator B** charges a commission of 50% - \(148.333 \* .5\) or 74.1665 Hash 
* **Validator C** charges a commission of 1% - \(148.333 \* .01\) or 1.48333 Hash 
* **Delegators** receive the remaining portion of Hash after commission on the validator they have delegated to on a pro rata basis.

