---
description: A suggested process for getting started with loading assets onto Provenance
---

# Asset Originator's Guide

{% hint style="info" %}
This guide uses loans as the example digital asset, but the process can be applied to any type of asset.
{% endhint %}

This guide walks through the steps necessary to create an application using Provenance Blockchain and the [P8e Contract Execution Environment ](../p8e/overview/)to load and fund digital assets on chain. In the example, digital loans are boarded to Provenance, funded with stablecoin, and listed on the Figure Portfolio Manager \(PM\).

## Example Application

The application exists in the `Interface` layer of the [Application Architecture](../blockchain/introduction/application-architecture.md), and makes use of a [hybrid model ](../blockchain/introduction/major-components.md)of on-chain and off-chain \(client-side\) data.

In our example, the borrower applies \(and is approved\) for a loan through the Originator's own Loan Origination System \(LOS\). The loan is boarded to Provenance only when the loan is ready for immediate funding, at which point the application will:

1. **Record the loan data** to the originator's private local Encrypted Object Store \(EOS\) through the execution of a [P8e Contract.](../p8e/overview/#p-8-e-client-side-contracts) This establishes a record of the assets and its [ownership](../modules/marker-module.md) \(the originator\) on the blockchain, while preserving the privacy and security of the data under the originator's control.
2. **\(Optionally\) Fund the loan** using stablecoin issued by an Omnibus Bank.
3. **Run a validation P8e contract** over the loan to ensure the loan was issued according to the rules and underwriting guidelines that the originator set.
4. **Permission** Figure's Portfolio Manager to read the loan data.
5. **Provide loan tapes** to the Portfolio Manager over the lifecycle of the loan through P8e contract execution.

The guide will also outline the optional further steps of querying the loan data, updating or correcting data, and removing a loan from the system.

## Development Process

The development process for creating this loan boarding application will include:

1. Mapping the originator's loan data to the [Figure Loan Data Model](https://github.com/provenance-io/docs/tree/8cd22b3275410450484ff5db5dc973c6474eac77/integrating/assets.md).
2. Setting up the P8e environment, including a local node of the blockchain \(see below\).
3. If required, setting up an Omnibus Bank application to mint and burn stablecoin, as well as manage the associated fiat cash movement into and out of the system.
4. Writing P8e contracts:
   1. To record the initial loan data
   2. To track funding information during the stablecoin funding process
   3. To validate the loan data
   4. To update the loan tape for PM as needed
   5. To update \(or correct\) loan data \(optional\)
   6. To remove a loan from the system \(optional\)
5. Developing an application to orchestrate the execution of the P8e contracts and upload loan documents \(e.g. PDF of the signing note\), and to use the Provenance SDK to manage the flow of stablecoin, asset ownership, and data sharing on the blockchain.

A Provenance application has several options for interfacing with the blockchain during development and for production usage:

1. [Run a blockchain localnet](../blockchain/using-provenance/) in situ for easy local development;
2. Connect to the Provenance public [testnet](https://github.com/provenance-io/testnet);
3. Connect to the public production Provenance [mainnet](https://github.com/provenance-io/mainnet).

