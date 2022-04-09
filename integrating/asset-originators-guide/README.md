---
description: >-
  A suggested process for getting started with loading assets onto Provenance
  Blockchain
---

# Asset Originator's Guide

This guide walks through some key concepts and practical steps to begin your journey in originating and managing assets on Provenance Blockchain. You will learn how to use the [P8e Contract Execution Environment (p8e)](https://docs.provenance.io/p8e/overview) to manage private, sensitive data while maintaining a public record of changes, comprised of hashes of the data stored in p8e.

As we step through the guide, we will follow the journey of a digital mortgage as it gets onboarded, funded with stablecoin, validated by third parties, and registered in multiple downstream applications that create visibility and allow users to take further actions with their digital assets, such as selling them to investors.

{% hint style="info" %}
This guide uses a mortgage as an example digital asset, but the concepts and components can be applied to any type of asset.
{% endhint %}

## Why Onboard Loans to Provenance?

Before we get into the "how", let's talk about the "why".

If you've read the [Intro](../../ecosystem/financial-services-blockchain/), you know that Provenance Blockchain performs three key functions: ledger, registry, and exchange. Together these functions can reduce the cost and risk of participating in the lending industry. By onboarding loans to Provenance, originators gain access to a suite of tools that make tracking, validating, and transferring control and/or ownership of assets frictionless.&#x20;

Due to the decentralized nature of the Provenance Blockchain ecosystem, this suite of tools is ever-expanding. New decentralized applications can be built and operated by anyone, giving loan originators choices in how they manage their assets. Here are a couple of examples of the tools that are already or soon to be available:

**Portfolio Manager** - Manage and see the real-time performance of loans on Provenance Blockchain. Allows users to list, pool, pledge and sell loans to an ever-growing group of warehouse lenders and accredited investors. The user interface allows buyers and sellers to review up-to-date loan data and documents, and perform bi-lateral exchange of assets, reducing counter-party risk and time to close deals.

**Digital Asset Registry Technologies (DART)** - Allows owners, servicers, and eNote controllers to track and update loan ownership, servicing rights, and control of eNotes associated with mortgages registered on Provenance. Any changes made by other decentralized applications, such as Portfolio Manager, are automatically captured because they all share one source of truth: the Provenance Blockchain ledger. This eliminates the need to update a separate, centralized registry after pledging or selling loans to investors or warehouse lenders.

**Validation Oracle** - Request and receive third-party loan validation services. The Validation Oracle service helps originators and investors manage permissions to loan data and loan validation results, sharing it with trusted third-party validation service providers and select buyers.

On top of this suite of tools, the Provenance Blockchain ecosystem opens loan originators to the USDF payment rails. Imagine that each party in a loan sale has an account funded with both Hash, the Provenance Blockchain utility token, and USDF, the USD-backed stablecoin minted on Provenance by the USDF consortium of omnibus banks. The seller uses Portfolio Manager to create a pool of loans represented as a [Marker](../../modules/marker-module.md) on Provenance and proposes the sale of that Marker to an investor. If the investor agrees to the terms, the buyer and seller sign and submit a transaction proposal to the Provenance Blockchain network for validation. As the transaction proposal gets picked up by a validator, the gas fee for recording the transaction is debited from the buyer's account, while the loan pool Marker is transferred to the buyer's account and USDF is transferred to the seller's account. The entire transaction takes place in one block recorded to the Provenance Blockchain ledger. No waiting for a wire transfer. No waiting for loan documents to show up in the mail. Just $$t+0$$ bi-lateral settlement, where $$t=time$$.

## Asset Life Cycle

Like any digital asset, there are many paths a mortgage could take within the lending ecosystem. A non-exhaustive list of actions a loan originator could take in the Provenance ecosystem includes:

* **Onboard the eNote only** - A loan originator may simply want to take advantage of the [Digital Asset Registry Technologies (DART)](https://www.dartinc.io) asset registry. By boarding the data associated with an electronic promissory note (eNote) to Provenance, DART allows users to track and make changes to who controls the eNote and can automatically reconcile changes in ownership and control of that asset when those changes are made in other applications that interact with Provenance Blockchain, such as Figure's Portfolio Manager.
* **Pledge interest to Warehouse Lenders** - By boarding an eNote or funded loan to Provenance, loan originators can use Figure's Portfolio Manager to pledge loan payments to Warehouse Lenders in exchange for cash or stablecoin. If enabled, changes in control of the eNote are automatically picked up by DART, removing the need for reconciliation.
* **Fund the loan with stablecoin** - When partnered with an Omnibus Bank and/or a Warehouse Lender, loan originators can fund loans using stablecoin. As lenders deposit funds with the Omnibus Bank, stablecoin is minted into a Marker account representing the originator's funding source. Loan interest can be pledged pre- or post-funding, offering loan originators the chance to receive stablecoins from a Warehouse Lender in exchange for an eNote prior to disbursing funds to borrowers. When using stablecoin, settlement occurs bi-laterally, removing risk for both parties.
* **Validation** - Loan validation may occur before, during, or after close, and may be performed by the originator itself or a third-party validation service provider. As results become available, they can be pushed to Provenance to be combined with loan origination data and made available to investors in applications such as Portfolio Manager. Control over who can view validation results is controlled by the party that requested the validation services.
* **Onboard the entire loan** - After loans have been closed and funded, a digital record can be created or updated (in the eNote/validation first approach). Originators have the choice to permission as many downstream services, such as DART and Portfolio Manager, as they desire.
* **Pool loans and sell them** - Within Portfolio Manager, loan originators or current owners can pool loans and find investors ready to buy them. When combined with validation services connected to Provenance, owners and investors can share data with third-party reviewers and see results grouped by loan pools. Loan sales funded by stablecoin use bi-lateral settlement and any transfers of ownership are automatically reconciled by DART, assuming the asset originator chose to permission both Portfolio Manager and DART as they boarded the loan.

## Participation Models

There is more than one way to participate in the Provenance ecosystem as an asset originator. Some originators will want to simply send data to a technology service provider (such as Figure) and allow them to orchestrate the process of pushing data to the Encrypted Object Store and Provenance Blockchain ledger. In this model, the technology service provider likely also hosts and operates the P8e Contract Execution Environment and/or [Provenance Blockchain Nodes](https://docs.provenance.io/blockchain/introduction/major-components#provenance-blockchain-node) used to store loan data and submit transactions to the network, respectively.

Other originators will want full control over their data and will choose to operate their own interfaces, middleware, Object Stores, or Provenance Blockchain Nodes. Ultimately, the originator can decide which components they are comfortable operating. The more control over the running applications and data stores, the more flexibility the originator has to build out new use cases with business partners within the ecosystem, extending the loan data model as needed.

To support these choices, Figure has both deployed a white-label loan onboarding service - where orchestration including key management is completely handled on behalf of loan originators - and published an open-source asset onboarding service that uses the open-source Provenance Software Development Kit (SDK) and can be extended by any asset originator.

{% hint style="info" %}
Loan originators looking to take the white-label approach to onboard loans can skip ahead to the [API Usage Guide](loan-onboarding-service/api-usage-guide.md), which when combined with the [API Specification](loan-onboarding-service/api-specification.md) and [Data Mapping](loan-onboarding-service/data-mapping.md) pages will tell you all you need to know to get started.
{% endhint %}

The rest of this guide will walk through all of the steps required to deploy and operate each of the components listed in the next section.

## Components

As described in the Asset Life Cycle section above, there are many paths an asset can take after being boarded to the Provenance Blockchain and many applications that can be permissioned to read data from the Encrypted Object Store. The rest of this guide will focus on how to deploy, configure, and interact with the components required to onboard assets in such a way that they are consumable by applications downstream.

For sake of brevity, this guide will not describe how to deploy and configure a Provenance Blockchain Node. It will, however, guide users in how to spin up a local network for testing, and how to configure which node is used when submitting the transaction proposal message to the network.

{% hint style="info" %}
Figure Tech can provide Provenance Blockchain Nodes as a service for organizations that want to deploy, manage, and monitor their own nodes. Check out the [Figure Tech](https://www.figure.tech) website for more information.
{% endhint %}

Components covered by this guide include:

* the P8e Contract Execution Environment, including:
  * the  Encrypted Object Store and
  * a variation of the asset onboarding service called the loan onboarding service, which implements the [p8e-scope-sdk](https://github.com/provenance-io/p8e-scope-sdk) and uses the open-source loan data model and associated standards,
* key management solutions, and
* p8e contracts.

## Environments

This guide will touch on the differences between connecting to a local network for development, the Provenance Blockchain public [testnet](https://github.com/provenance-io/testnet), and the public production Provenance Blockchain [mainnet](https://github.com/provenance-io/mainnet).
