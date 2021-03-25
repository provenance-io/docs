# Asset Originators

Assigned @VDub

Provenance Blockchain acts as ledger, registry and exchange for digital assets. Asset originators register and custody loans and other digital assets on the Provenance Blockchain through the use of [smart contracts](../../../p8e/overview.md) and stablecoin backed by an [Omnibus Bank](omnibus-banks.md). Provenance contracts take encrypted data from the originator and transform that information to encrypted data in the originator's own private [object store](../../../p8e/components-1/encrypted-object-store/) with object hashes recorded on the blockchain. In addition to onboarding digital assets, originators may fund, finance, sell, and service assets on the blockchain. 

## Onboarding

Digital assets are "onboarded to the blockchain" by the execution of contracts that the originator writes to take origination data, verify its completeness, and output data as recorded facts in an encrypted object store private to and hosted by the originator. The Provenance Contract Execution Environment records hashed representations of all documents, data, transactions and smart contracts \(for validating documents, data and transactions\) that are added to the blockchain. Modification or updates of the data  only occur through further contract execution on the data, with checks on the input hash of data from the object store against the blockchain to ensure no external data modification has taken place. In this way, the truth of the data is verified without the need for trust in the individual originator's data store.

Provenance contracts can be crafted to require participation by multiple parties, such as an originator and an auditor. For example, an "assign to servicer" contract could involve the originator and the servicer, both verifying the data is complete enough to be transferred to the servicer. All parties participating in a contract execution are able to read the data, and the resulting data facts are copied to each participant's encrypted data store.

## Funding





