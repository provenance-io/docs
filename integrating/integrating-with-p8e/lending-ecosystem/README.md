---
description: >-
  The guide uses the lending ecosystem as a very real example of building out a
  use case on top of Provenance Blockchain
---

# Lending Ecosystem

The lending industry has been looking at distributed ledger technology for a while now. A quick search for uses of blockchain technology in the lending industry brings up a laundry list of great ideas. Tamper resistant loan files, title registries, bilateral settlement of Mortgage Backed Securities to name a few.

The Provenance Blockchain is purpose-built for all of these use cases and so much more.

## Why Onboard Loans to Provenance?

If you've read the [Intro](../../../ecosystem/financial-services-blockchain/), you know that Provenance Blockchain performs three key functions: ledger, registry, and exchange. Together these functions can reduce the cost and risk of participating in the lending industry. By onboarding loans to Provenance, originators gain access to a suite of tools that make recording, funding, tracking, validating, and transferring control and/or ownership of loans frictionless.

Here are some examples:

**Portfolio Manager** - Manage and see the real-time performance of loans on Provenance Blockchain. Allows users to list, pool, pledge and sell loans to an ever-growing group of warehouse lenders and accredited investors. The user interface allows buyers and sellers to review up-to-date loan data and documents, and perform bi-lateral exchange of assets, reducing counter-party risk and time to close deals.

**Digital Asset Registry Technologies (DART)** - Allows owners, servicers, and eNote controllers to track and update loan ownership, servicing rights, and control of eNotes associated with mortgages registered on Provenance. Any changes made by other decentralized applications, such as Portfolio Manager, are automatically captured because they all share one source of truth: the Provenance Blockchain ledger. This eliminates the need to update a separate, centralized registry after pledging or selling loans to investors or warehouse lenders.

**Validation Oracle** - Request and receive third-party loan validation services. The Validation Oracle service helps originators and investors manage permissions to loan data and loan validation results, sharing it with trusted third-party validation service providers and select buyers. Validation results become part of the digital asset for the remainder of its life.

Due to the decentralized nature of the Provenance Blockchain ecosystem, this suite of tools is ever-expanding. New decentralized applications can be built and operated by anyone, giving participants choices in how they manage their assets.

On top of this suite of tools, the Provenance Blockchain ecosystem opens loan originators to the USDF payment rails. Imagine that each party in a loan sale has an account funded with USDF, the USD-backed stablecoin minted on Provenance by the USDF consortium of omnibus banks. A seller uses Portfolio Manager to create a pool of mortgages represented as a [Marker](../../../modules/marker-module.md) on Provenance and proposes the sale of that Marker to an investor. The investor uses Portfolio Manager to review loans in the pool, accessing third-party validators through the use of Validation Oracle, if needed. If the investor agrees to the seller's terms, the buyer and seller sign and submit a transaction proposal to the Provenance Blockchain network for validation. As the transaction proposal gets executed, the loan pool Marker is transferred to the buyer's account and USDF is transferred to the seller's account. Assuming these loan were registered with DART, the records of who controls the eNote and who services the loans are automatically picked up and reflected to all parties involved.

The entire transaction takes place in one block recorded to the Provenance Blockchain ledger. No waiting for a wire transfer. No waiting for loan documents to show up in the mail. No notifying a centralized eNote registry of the sale. Just $$t+0$$ bi-lateral settlement, where $$t=time$$.

Combine bi-lateral settlement with a suite of tools for managing your assets, and you've got a fully digital lending ecosystem with zero counter-party risk.

## Explore the Lending Ecosystem Use Case

On the following pages, we will explore how the Figure Tech team translated the typical post-close events to a blockchain-enabled world. By understanding those activities, you can quickly see how the products mentioned above came to fruition.

After that, we will dive into the Loan Package data model. Understanding the data model is step one in integrating with the Provenance Blockchain network as a loan originator or other data provider.
