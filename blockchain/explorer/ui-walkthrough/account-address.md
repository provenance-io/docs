# Account (Address)

An address is your identifier on the blockchain. You can think or it as an account number, but more difficult to guess. By searching for a specific address, you can view assets held by the address, proposals voted on, and delegations made.

Addresses are only searchable by specific address via the search function, or by following through a linked address.

## Detail

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 11.14.20 PM.png>)

Information to note:

* **Account Name**: currently only applies to **ModuleAccount** types, this helps identify what the account is for
* **Sequence**: the current number of transactions the address has submitted to the blockchain. This helps maintain order of transactions and acts as a check when an address is submitting a lot of transactions in short order
* **Fungible Tokens**: the number of distinct tokens the address holds a balance for
* **Non-Fungible Tokens**: the number of NFTs the address holds. Currently seen as [Scopes](../../../modules/metadata-module.md)

## Account Assets

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 11.14.58 PM.png>)

A list of fungible tokens held by the address.&#x20;

## Account Transactions

![](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 11.15.09 PM.png>)

A list of transactions that involve the address, either as a party within the transaction, or as the signer on a transaction. The list is filterable by **Tx Message Type** and **Status**.&#x20;

## Forthcoming

As we glean more information from the blockchain, additional features may include:

* Delegations
* Unbondings
* Rewards
* Attributes



