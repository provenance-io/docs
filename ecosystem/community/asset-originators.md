# Asset Creators

### Overview

Provenance Blockchain acts as ledger, registry and exchange for digital assets. Asset creators can register and custody digital assets on the Provenance Blockchain through the use of [client-side contracts](../../p8e/overview/). Asset value-related operations may be achieved by using a stablecoin backed by an [Omnibus Bank](). Provenance client-side contracts take encrypted data from the asset creator and transform that information to encrypted data in the creator's own private [object store](../../p8e/overview/encrypted-object-store/) with object hashes recorded on the blockchain through the [metadata](../../modules/metadata-module.md) module. In addition to boarding digital assets, asset creators may fund, finance, sell, and service assets on the blockchain. 

### Boarding Assets

Digital assets are "boarded to the blockchain" by the execution of contracts that the originator writes to take origination data, verify its completeness, and output data as recorded facts in an encrypted object store private to and hosted by the originator. The [Contract Execution Environment](../../p8e/overview/) creates hashed representations of all documents, data, transactions and client-side contract executions that are recorded to the blockchain. Modification or updates of the data only occur through further contract execution on the data, with checks on the input hash of data from the object store against the blockchain to ensure no external data modification has taken place. In this way, the truth of the data is verified without the need for trust in the individual originator's data store.

### Asset Ownership

Ownership of a digital asset on Provenance is defined by a [Marker](../../modules/marker-module.md) structure on chain. During the creation of a scope by the Contract Execution Environment, a default address is created to represent ownership of the asset by the asset creator. Provenance Blockchain distinguishes between the "data owner\(s\)" and the "value owner\(s\)" of any scope. Data owners have permission to perform read/write operations on the scope. Value owners hold rights to the actual ownership of the asset and any value the digital asset holds. Data owners and value owners may or may not be the same entities. For example, a digital loan asset may have data owners including the originator and servicer while the value owner may be an investor who has purchased the loan.

### Value Operations

Value-related operations for digital assets can be achieved using [coins](../../blockchain/basics/stablecoin.md). For example, originators can funds their loans at time of boarding using stablecoin issued by an [Omnibus Bank](). When the Omnibus Bank receives funds from the originator, it mints a corresponding amount of stablecoin using the [Marker Module](../../modules/marker-module.md) and an [account](../../blockchain/basics/accounts.md) representing the originator’s funding source. Loans are funded by transferring coin to the Omnibus Bank's blockchain address. When instructed by the originator, the Omnibus Bank will convert the borrower’s coin to fiat and send funds to the borrower’s bank account. The coin used is burned in the process of conversion to fiat.

