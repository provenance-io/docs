# Transactions

A transaction is how everything gets committed to the blockchain. From transfers of Hash to [Smart Contract](../../../modules/provwasm-smart-contracts.md) executions, it all gets recorded to the blockchain ledger via transactions.

## Listview

![A list of the most recent transactions committed to the blockchain](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 5.29.24 PM.png>)

A transaction can contain many messages within, all considered applied to the blockchain with the success of the transaction as a whole. The submitter of the transaction must have permissions to sign for all messages within the transaction, and either pay the gas fees to commit it to the blockchain, or have a fee grant that applies to the transaction.

Filters exist to make finding certain transactions easier.&#x20;

* **Type**: this is the message type for a submitted transaction. They are grouped by generally accepted criteria. Select a single type to filter
* **Status**: Success or Failed
* **From **& **To**: specify a date range to filter transactions down to a time period. Can include one or both

Information to note:

* **Tx Hash**: the unique hash of the transaction. Can be used as a reliable identifier&#x20;
* **Tx Type**: a list of message types contained within the transaction. Can be any combination of message types
* **Fee**: the fee paid to submit the transaction to the blockchain. The higher the fee, the more data was processed for the transaction to be successful
* **Signer**: one or more addresses that submit the transaction, and sign off on the changes that take place. If no feepayer grant is present, then the first signer pays the fee
* **Status**: **Success** if no errors occurred processing the transaction, or **Failed** if an error occurred processing the transaction
* **Timestamp**: the timestamp taken from the block that holds the transaction

## Detail

![Transaction Detail overview](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 8.09.39 PM.png>)

Each transaction row in the listview links through to a detail page. Here you will see more information about the transaction, including additional signers, memo information, and message details.

Information to note:

* **Messages**: the messages are an infinite scrolling listview. Each message shows the messgae object as it was submitted with the transaction.&#x20;
* **Tx Type**: a filter exists to show only specific message types within the transaction, as a transaction can have one to many messages
* **"Show full transaction JSON"**: this is a clickable message that opens a window showing the full JSON for the transaction. Here you can dive deeper into the resulting logs of a transaction. Example:

![Collapsible box showing the full JSON for a transaction](<../../../.gitbook/assets/Screen Shot 2021-11-15 at 8.09.52 PM.png>)

## Forthcoming

As we glean more information from the blockchain, additional features may include:

* A feepayer field to show who actually paid the transaction fee
* Resulting event logs to show results of a transaction message
