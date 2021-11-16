# Blocks

Blocks are the "building blocks" (ha!) of the Provenance Blockchain. All transactions are committed to the chain and agreed upon within the context of a block. As a timestamp can be variable, depending on a machine's internal clock, a block height is a better measure of how far along a blockchain is.

Chain consensus is determined by validator's signing, and thus agreeing, on each block, and all transactions that are committed within it. This is how you keep a blockchain going, and maintain a constant agreed-upon ledger state.

## Listview

![A list of recent blocks](<../../../../.gitbook/assets/Screen Shot 2021-11-15 at 4.55.09 PM.png>)

Information to note:

* **Proposer**: the validator that proposes the block, and all transactions contained therein. As the proposer, they get an initial cut of any transaction fees. See [here](../../../../ecosystem/financial-services-blockchain/distribution.md)
* **Validators**: of the active validator set, how many validators signed consensus on the block
* **Voting Power**: corresponding to the **Validators** ratio, the percentage of voting power that signed consensus on the block
* **Timestamp**: the time the block was committed to the chain. This is the timestamp used for all transactions that occur within the block, and can be used to approximate a time until an expected block is cut

## Detail

![The block detail page](<../../../../.gitbook/assets/Screen Shot 2021-11-15 at 5.11.07 PM.png>)

Information to note:

* **Block Validators**
  * **Proposer Priority**: TBD
