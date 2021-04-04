# Asset Originators

Provenance Blockchain acts as ledger, registry and exchange for digital assets. Asset originators register and custody loans and other digital assets on the Provenance Blockchain through the use of [client-side contracts](../../p8e/overview/) and stablecoin backed by an [Omnibus Bank](omnibus-banks.md). Provenance contracts take encrypted data from the originator and transform that information to encrypted data in the originator's own private [object store](../../p8e/overview/encrypted-object-store/) with object hashes recorded on the blockchain. In addition to onboarding digital assets, originators may fund, finance, sell, and service assets on the blockchain. 

### Onboarding

Digital assets are "onboarded to the blockchain" by the execution of contracts that the originator writes to take origination data, verify its completeness, and output data as recorded facts in an encrypted object store private to and hosted by the originator. The [Contract Execution Environment](../../p8e/overview/) records hashed representations of all documents, data, transactions and client-side contract executions \(for validating documents, data and transactions\) that are added to the blockchain. Modification or updates of the data only occur through further contract execution on the data, with checks on the input hash of data from the object store against the blockchain to ensure no external data modification has taken place. In this way, the truth of the data is verified without the need for trust in the individual originator's data store.

Client-side contracts can be crafted to require participation by multiple parties, such as an originator and an auditor. For example, an "assign to servicer" contract could involve the originator and the servicer, both verifying the data is complete enough to be transferred to the servicer. All parties participating in a contract execution are able to read the data, and the resulting data facts are copied to each participant's encrypted data store.

### Funding

Figure, for example, funds its loans at time of onboarding using stablecoin issued by an [Omnibus Bank](omnibus-banks.md). When the Omnibus Bank receives funds from the originator, it mints a corresponding amount of stablecoin using the [Marker Module](../../modules/marker-module.md) and an [account](../../blockchain/basics/accounts.md) representing the originator’s funding source. As loans are onboarded, an address is established on the blockchain representing the borrower. Loans are funded by transferring coin to the borrower’s blockchain address. When instructed by the originator, the Omnibus Bank will convert the borrower’s coin to fiat and send funds to the borrower’s bank account. The coin used is burned in the process of conversion to fiat.

