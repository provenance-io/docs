# P8e Transition Flow

The following diagram outlines the flow of data for typical transactions proposed to Provenance Blockchain.

![](../../.gitbook/assets/provenance-p8e-txn-flow%20%284%29.png)

1. Affiliates utilize the Contract Execution Environment to submit contracts to create single or multi-party agreements.
2. The contracts and asset data are forwarded to all affiliates participating in the contract.
3. Each affiliate’s Encrypted Object Store is updated with the encrypted asset data resulting in each participating affiliate having a copy of the asset data.
4. Using their copy of the asset data, each affiliate participating in the transaction executes the contracts.
5. The hashed results of the contract executions are returned to the submitting affiliate’s execution environment.
6. All hashed execution results are sent to Provenance Blockchain.
7. The hashed results are validated by the validators. If the majority agree, the transaction is proposed to the blockchain.
8. Provenance Blockchain emits events notifying affiliates the contract has been committed on the blockchain.
9. The index is updated with the new contract information. This information is used later for searching and querying the data.
10. Affiliates acknowledge the completion event.

For certain transactions, affiliates can also interact directly with Provenance Blockchain without utilizing or even needing a Contract Execution Environment. For example, [markers](../../modules/marker-module.md) are managed by directly interacting with Provenance Blockchain instead of executing contracts.

