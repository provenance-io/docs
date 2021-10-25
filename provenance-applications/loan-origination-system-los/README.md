---
description: Figure Loan Origination System
---

# Figure LOS

### Overview

The Figure Loan Origination System \(LOS\) is built on top of Provenance Blockchain, making use of Provenance modules, stablecoin, and the [Contract Execution Environment](../../p8e/overview/). Figure applications that consume loan data use the [Figure Loan Model](assets.md). The following sections describe how any loan originator may make use of Provenance using the Figure LOS as a reference implementation.

For a general overview of how digital assets \(such as all-digital loans\) are handled on Provenance Blockchain, see [Asset Creators](../../ecosystem/community/asset-originators.md).

### Loan Origination System

Before onboarding a loan to the Provenance Blockchain, a loan originator creates a digital loan packet plus digital source documents \(e.g., title, credit, income, etc\) for validation purposes. Where possible, data should be digitally signed by the provider. This loan packet is typically created through a loan origination system integrated with Provenance Blockchain. The originator requires the borrower to sign the loan agreement, delivers the required disclosures \(Truth-in-Lending Act, etc.\), and waits out the rescission period \(if relevant\).  The originator may define any schema for their loan data; however, a common data model must be used if the loans are to interact with other Provenance-powered applications, such as Figure’s [Portfolio Manager ]()or Marketplace.

### Onboarding Loans to Provenance

Digital assets can be "onboarded to the blockchain" by the execution of client-side contracts that the originator writes to consume origination data, verify its completeness, and output data as recorded facts in an encrypted object store private to and hosted by the originator. The Provenance Contract Execution Environment records hashed representations of all documents, data, transactions and client-side contracts to the blockchain. Modification or updates of the data  only occur through further contract execution on the scope, with checks on the input hash of data from the object store against the blockchain to ensure no external data modification has taken place. In this way, the truth of the data is verified without the need for trust in the individual originator's data store.

Client-side Provenance contracts can be crafted to require participation by multiple parties, such as an originator and an auditor. For example, an "onboard-to-servicer" contract could involve the originator and the servicer, both verifying the data is complete enough to be transferred to the servicer. All parties participating in a contract execution are able to read the data. Either party may reject the contract execution. If all parties are in agreement, the resulting data facts are copied to each participant's encrypted data store as part of the head state of the scope.

### Gas

While usage of the P8e execution environment itself does not incur any fees directly, P8e scope metadata is persisted to the blockchain through a transaction. This transaction will incur a gas fee \(paid in Hash\). The originator’s local instance of the execution environment must be configured with an appropriate Hash account containing sufficient Hash to cover the transaction fee per contract execution.     





