# Assets

## Loans

Figure loan data at origination is stored as a single scope in the P8e Encrypted Object Store. Servicing data is recorded in separate scopes. The loan scope consists of key-value data pairs, where the key is the Fact name, and the value is a Google Protocol Buffer \(protobuf\). 



| Fact Name | Value Data Type | Description |
| :--- | :--- | :--- |
| additional_parties | PartiesList | Additional co-borrowers or co-signers on the loan |
| credit_reports | CreditReportsList | Borrower credit reports |
| documents | DocumentList | Metadata about documents associated with the loan. Actual documents are stored separately. |
| funding | Funding | Information about disbursement of funds to borrower |
| income_records | IncomeRecordsList | Borrower Income sources |
| loan | Loan | Metadata about the loan |
| primary_party | Party | Primary borrower on the loan |
| digital_signature_packets | DocumentWithDataList | Structure data from third-party data providers that is digitally signed by source |
| servicing | LoanServicing | Information about the servicer for the loan |
| signed_prom_note | Disclosure | Signed promissory note |
| tri_merg_reports | TriMergeReportsList | Consolidated report from the three major credit bureaus |
| underwriting_packet | UnderwritingPacket | Underwriting data and offers |
| lien_property | LienProperty | If a property-based loan, information about the property on which a lien is filed |