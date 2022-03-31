---
description: >-
  A suggested process for getting started with loading assets onto Provenance
  Blockchain
---

# Asset Originator's Guide

This guide walks through some key concepts and practical steps to begin your journey originating and managing assets on Provenance Blockchain. We will learn how to use the [P8e Contract Execution Environment (p8e)](https://docs.provenance.io/p8e/overview) to manage private, sensitive data while maintaining a public record of changes, comprised of hashes of the data stored in p8e. As we step through the guide, we will follow the journey of a digital mortgage as it gets boarded, funded with stablecoin, validated by third parties, and registered in multiple down stream applications that create visibility and allow users to take further actions with their digital assets, such as selling them to investors.

{% hint style="info" %}
This guide uses a mortgage as the example digital asset, but the concepts and components can be applied to any type of asset.
{% endhint %}

## Asset Life Cycle

Like any digital asset, there are many paths a mortgage could take within the lending ecosystem. A non-exhaustive list actions a loan originator could take in the Provenance ecosystem includes:

* **Onboard the eNote only** - A loan originator may simply want to take advantage of the [Digital Asset Registry Technologies (DART)](https://www.dartinc.io) asset registry. By boarding the data associated with an electronic promissory note (eNote) to Provenance, DART allows users to track and make changes to who controls the eNote, and can automatically reconcile changes in ownership and control of that asset when those changes are made in other applications that interact with Provenance Blockchain, such as Figure's Portfolio Manager.
* **Pledge interest to Warehouse Lenders** - By boarding an eNote or funded loan to Provenance, loan originators can use Figure's Portfolio Manager to pledge loan payments to Warehouse Lenders in exchange for cash or stablecoin. If enabled, changes in control of the eNote are automatically picked up by DART, removing the need for reconciliation.
* **Fund the loan with stablecoin** - When partnered with an Omnibus Bank and/or a Warehouse Lender, loan originators can fund loans using stablecoin. As lenders deposit funds with the Omnibus Bank, stablecoin is minted into a Marker account representing the originator's funding source. Loan interest can be pledged pre- or post-funding, offering loan originators the chance to receive stablecoins from a Warehouse Lender in exchange for an eNote prior to disbursing funds to borrowers. When using stablecoin, settlement occurs bi-laterally, removing risk for both parties.
* **Validation** - Loan validation may occur prior, during, or after close, and may be performed by the originator itself or third party validation service provider. As results become available, they can be pushed to Provenance to be combined with loan origination data and made available to investors in applications such as Portfolio Manager. Control over who can view validation results is controlled by the party that requested the validation services.
* **Onboard the entire loan** - After loans have been closed and funded, a digital record can be created or updated (in the eNote/validation first approach). Originators have the choice to permission as many downstream services, such as DART and Portfolio Manager, as they desire.
* **Pool loans and sell them** - Within Portfolio Manager, loan originators or current owners can pool loans and find investors ready to buy them. When combined with validation services connected to Provenance, owners and investors can share data with third party reviewers and see results grouped by loan pools. Loan sales funded by stablecoin use bi-lateral settlement, and any transfers of ownership are automatically reconciled by DART, assuming the asset originator chose to permission both Portfolio Manager and DART as they boarded the loan.

## Participation Models

There is more than one way to participate in the Provenance ecosystem as an asset originator. Some originators will want to simply send data to a technology service provider (such as Figure) and allow them to orchestrate the process of pushing data to the Encrypted Object Store and Provenance Blockchain ledger. In this model, the technology service provider likely also hosts and operates the P8e Contract Execution Environment and/or [Provenance Blockchain Nodes](https://docs.provenance.io/blockchain/introduction/major-components#provenance-blockchain-node) used to store loan data and submit transactions to the network, respectively.

Other originators will want full control over their data, and choose to operate their own interfaces, middleware including their own P8e Contract Execution Environments, or even their own Provenance Blockchain Nodes. Ultimately, the originator can decide which components they are comfortable operating. Obviously the more control over the running applications and data stores, the more flexibility the originator has to build out new use cases with business partners within the ecosystem, defining their own data models as needed.

To support these choices, Figure has both deployed a white-label loan onboarding service - where orchestration including key management is completely handled on behalf of loan originators - and published an open source asset onboarding service that uses the open source Provenance Software Development Kit (SDK) and can be extended by any asset originator. This guide will prioritize walking through the open source asset onboarding service in its examples, and touch on the white-label service when in comes to the data model and API specification.

## Components

As described in the Asset Life Cycle section above, there are many paths an asset can take after being boarded to the Provenance Blockchain and many applications that can be permissioned to read data from the Encrypted Object Store. The rest of this guide will focus on how to deploy, configure, and interact with the components required to onboard assets in such a way that they are consumable by applications down stream.

For sake of brevity, this guide will not describe how to deploy and configure a Provenance Blockchain Node. It will, however, guide users in how to spin up a local network for testing, and how to configure which node is used when submitting the transaction proposal message to the network.

{% hint style="info" %}
Figure Tech can provide Provenance Blockchain Nodes as a service for organizations that want to deploy, manage, and monitor their own nodes. Check out [figure.tech](https://www.figure.tech) for more information.
{% endhint %}

Components covered by this guide include:

* the P8e Contract Execution Environment, including the  Encrypted Object Store,
* a variation of the asset onboarding service called the loan onboarding service, which implements the [p8e-scope-sdk](https://github.com/provenance-io/p8e-scope-sdk) and uses the open source loan data model and associated standards,
* key managements solutions, and
* p8e contracts.

## Environments

A Provenance Blockchain application has several options for interfacing with the blockchain during development and for production usage. This guide will touch on:

1. Running a blockchain localnet for easy local development,
2. Connecting to the Provenance Blockchain public [testnet](https://github.com/provenance-io/testnet),
3. Connecting to the public production Provenance Blockchain [mainnet](https://github.com/provenance-io/mainnet).
