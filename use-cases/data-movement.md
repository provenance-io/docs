# Data Movement

It is important to understand how data moves throughout the system. For purposes of this section, we will look at how an originator onboards a loan to the blockchain and the impact of the data when that loan is later sold.

## Onboarding Data

Loans are onboarded by originators onto Provenance by utilizing the Contract Execution Environment to execute client contracts. When a contract is successfully executed, the loan data is encrypted and saved in the originator's Encrypted Object Store. For single-party contracts, the originator has the only copy of the data. For multi-party contracts, each party participating in the contract \(i.e. a custodian\) will have a copy of the data in their Encrypted Object Store. Information about the loan is memorialized to the blockchain, including loan ownership. However, loan data is not included in this information, only the hashed results of the client contract execution are included.

## Adding Servicer

When the loan has reached the need to service the loan phase in the loan lifecycle, the servicer is permissioned to the data by the loan owner. An encrypted copy of the data is saved in the servicer's Encrypted Object Store. To update the data with servicing information, the servicer follows a similar process to how the loan was originated. The servicer utilizes their Contract Execution Environment to execute servicing related client contracts. These contracts will be multi-party contracts which the loan owner will also participate in. This will ensure the originator's copy of the data will also be updated with servicing information. Also similar to the origination process, only encrypted servicing information is saved to the originator's and servicer's Encrypted Object Store. Only the hashed results of the client contracts are memorialized to the blockchain.

## Transferring Ownership

As previously mentioned, loan ownership is tracked and maintained on the blockchain. When loan ownership is transferred on Provenance, the loan owner is updated on the blockchain. For example, when a loan is sold the encrypted loan data stored in the Encrypted Object Store does not need to change. Only the ownership on the blockchain needs to be updated.

The owner taking control of the loan has a few options for handling the loan data.

### Receive the asset

The new owner can choose to receive a copy of the loan data. This data will be saved to their Encrypted Object Store within their Contract Execution Environment.

### Receive no data

Since ownership information is tracked on the blockchain, loan owners are not required to maintain a copy of the data. They are not required to have an Encrypted Object Store nor a Contract Execution Environment. If the owner receiving the loan chooses not to maintain a copy of the data, the ownership is still updated on the blockchain. The encrypted data remains with the previous owner and with the servicer in their Encrypted Object Store. Note when ownership of a loan is transferred, the prior owner's data will no longer receive updates. The prior owner's data will remain intact but will become stale as updates to the loan are made \(i.e. when the servicer applies payments\). The servicer will have the only copy of the data currently being maintained.

### Designated data holder

The new owner can also designate another party to maintain their data for them \(i.e. custodian\). In this case, the ownership is sill updated on the blockchain but the designated party will maintain the data in their Encrypted Object Store.

## Transfer the asset off-chain

If the current owner desires, a loan can be off-boarded from Provenance at anytime. When a loan is off-boarded, the encrypted data will remain in the owner's Encrypted Object Store but will become stale since it will no longer receive updates. Another important note to consider before off-boarding any loan is the benefits of Provenance will be lost since the loan is no longer on-chain.

