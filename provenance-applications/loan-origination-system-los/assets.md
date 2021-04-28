# Figure Loan Model

## Loans

Figure loan data at origination is stored as a single scope in the P8e Encrypted Object Store. Servicing data is recorded in a separate scope\(s\). Data inputs and outputs of P8e contracts are defined using [Google Protocol Buffers](https://developers.google.com/protocol-buffers) \(protobufs\). Each contract execution operates on a single loan scope, consisting of key-value pairs where the value object is a protobuf. All identifiers in the Figure loan data model are Universally Unique Identifiers \(UUIDs\).  The Figure loan scope model is also used by Figure Servicing and the Figure Portfolio Manager, allowing joint contract execution and data sharing between these applications.

| Fact Name | Value Data Type | Description |
| :--- | :--- | :--- |
| **additional\_parties** | `PartiesList` | Additional co-borrowers or co-signers on the loan |
| **credit\_reports** | `CreditReportsList` | Borrower credit reports |
| **documents** | `DocumentList` | Metadata about documents associated with the loan. Actual documents are stored separately. |
| **funding** | `Funding` | Information about disbursement of funds to borrower |
| **income\_records** | `IncomeRecordsList` | Borrower Income sources |
| **loan** | `Loan` | Metadata about the loan |
| **primary\_party** | `Party` | Primary borrower on the loan |
| **digital\_signature\_packets** | `DocumentWithDataList` | Structure data from third-party data providers that is digitally signed by source |
| **servicing** | `LoanServicing` | Information about the servicer for the loan |
| **signed\_prom\_note** | `Disclosure` | Signed promissory note |
| **tri\_merg\_reports** | `TriMergeReportsList` | Consolidated report from the three major credit bureaus |
| **underwriting\_packet** | `UnderwritingPacket` | Underwriting data and offers |
| **lien\_property** | `LienProperty` | If a property-based loan, information about the property on which a lien is filed |



