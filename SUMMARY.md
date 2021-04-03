# Table of contents

* [The Provenance Blockchain](README.md)

## Blockchain

* [Introduction](blockchain/introduction/README.md)
  * [Financial Services Blockchain](blockchain/introduction/financial-services-blockchain.md)
  * [Application Architecture](blockchain/introduction/application-architecture.md)
  * [Major Components](blockchain/introduction/major-components.md)
* [Basics](blockchain/basics/README.md)
  * [Anatomy of the Provenance Application](blockchain/basics/anatomy-of-a-provenance-application.md)
  * [Transaction Lifecycle](blockchain/basics/transaction-lifecycle.md)
  * [Query Lifecycle](blockchain/basics/query-lifecycle.md)
  * [Accounts](blockchain/basics/accounts.md)
  * [Coin](blockchain/basics/stablecoin.md)
  * [Gas and Fees](blockchain/basics/gas-and-fees.md)
* [Installing Provenance](blockchain/running-a-node/README.md)
  * [Running a Node](blockchain/running-a-node/running-a-node-1/README.md)
    * [Joining Testnet](blockchain/running-a-node/running-a-node-1/join-provenance-testnet.md)
    * [Become a Validator](blockchain/running-a-node/running-a-node-1/become-a-validator.md)
    * [Configure a Sentry](blockchain/running-a-node/running-a-node-1/configure-a-sentry.md)
* [Using Provenanced](blockchain/using-provenance/README.md)
  * [Query Command](blockchain/using-provenance/query-command.md)
  * [Tx Command](blockchain/using-provenance/tx-command.md)

## Ecosystem

* [Provenance](ecosystem/provenance/README.md)
  * [Value Proposition](ecosystem/provenance/value-proposition.md)
  * [History](ecosystem/provenance/mission.md)
  * [Hash](ecosystem/provenance/hash-2.0.md)
  * [Governance](ecosystem/provenance/governance/README.md)
    * [Voting](ecosystem/provenance/governance/voting/README.md)
      * [Software Upgrade Proposal](ecosystem/provenance/governance/voting/software-upgrade-proposal.md)
  * [Fees/Distribution](ecosystem/provenance/distribution.md)
  * [Legal Standing](ecosystem/provenance/legal-standing.md)
* [The Foundation](ecosystem/the-p-foundation.md)
* [The Community](ecosystem/the-p-community/README.md)
  * [Validator](ecosystem/the-p-community/validator.md)
  * [Delegator](ecosystem/the-p-community/delegator.md)
  * [Participants](ecosystem/the-p-community/participants/README.md)
    * [Asset Originators](ecosystem/the-p-community/participants/asset-originators.md)
    * [Omnibus Banks](ecosystem/the-p-community/participants/omnibus-banks.md)
* [The Funding Program](ecosystem/the-p-funding-program.md)
* [The Ecosystem](ecosystem/the-p-ecosystem.md)

## Modules

* [Inherited Modules](modules/inherited-modules.md)
* [Metadata](modules/metadata-module.md)
* [Marker](modules/marker-module.md)
* [Attribute](modules/account.md)
* [Name](modules/name-module.md)
* [Smart Contracts \(ProvWasm\)](modules/provwasm-smart-contracts.md)

## Contract Execution Environment \(P8e\) <a id="p8e"></a>

* [Provenance \(P8e\) Contract Execution Environment](p8e/overview.md)
* [Components](p8e/components-1/README.md)
  * [Encrypted Object Store](p8e/components-1/encrypted-object-store/README.md)
    * [Encryption Scheme](p8e/components-1/encrypted-object-store/encryption-scheme.md)
    * [DIME \(Encryption Envelope Specification\)](p8e/components-1/encrypted-object-store/dime-encryption-envelope-specification.md)
  * [P8e API](p8e/components-1/api/README.md)
    * [Reapers](p8e/components-1/api/reapers.md)
    * [Authentication](p8e/components-1/api/authentication.md)
    * [Contract Life Cycle](p8e/components-1/api/contract-life-cycle.md)
    * [Index Engine](p8e/components-1/api/index-engine.md)
  * [P8e UI](p8e/components-1/p8e-ui.md)
* [Security](p8e/security.md)
* [Integration](p8e/untitled/README.md)
  * [3rd Party Digital Signatures](p8e/untitled/3rd-party-digital-signatures.md)
* [Archive](p8e/archive/README.md)
  * [Data Retrieval](p8e/archive/data-retrieval.md)
  * [Building New Contracts](p8e/archive/building-new-contracts.md)

## Integrating

* [Quick Start](integrating/quick-start/README.md)
  * [Explanation](integrating/quick-start/hello-world-example.md)
* [Assets](integrating/assets.md)
* [SDK](integrating/sdk.md)
* [Error Codes](integrating/error-codes.md)

## Apps Built on Provenance <a id="provenance-applications"></a>

* [Apps Built On Provenance](provenance-applications/apps-powered-by-provenance.md)
* [Loan Origination System \(LOS\)](provenance-applications/loan-origination-system-los.md)
* [Portfolio Manager](provenance-applications/portfolio-manager-market-place.md)
* [Marketplace](provenance-applications/marketplace-digital-funds-services.md)
* [Adnales](provenance-applications/adnales.md)
* [Figure Pay](provenance-applications/figure-pay.md)
* [eVault](provenance-applications/evault.md)

## Contributing

* [ADR](contributing/adr/README.md)
  * [Template](contributing/adr/template.md)
  * [100 Blockchain Configuration and Concepts](contributing/adr/100-blockchain-configuration-and-concepts/README.md)
    * [100 - Genesis Network Configuration](contributing/adr/100-blockchain-configuration-and-concepts/100-genesis-network-configuration.md)
    * [101 - HD Wallets, Key Pairs, Addresses](contributing/adr/100-blockchain-configuration-and-concepts/101-hd-wallets-key-pairs-addresses.md)
    * [102 - Markers, Tokens, and Coins](contributing/adr/100-blockchain-configuration-and-concepts/102-markers-tokens-and-coins.md)
    * [103 - Transaction Fees and Gas](contributing/adr/100-blockchain-configuration-and-concepts/103-transaction-fees-and-gas.md)
  * [200 Base Infrastructure](contributing/adr/200-base-infrastructure/README.md)
    * [200 - Name Service](contributing/adr/200-base-infrastructure/200-name-service.md)
    * [201 - Account Metadata](contributing/adr/200-base-infrastructure/201-account-metadata.md)
  * [300 Core Concepts](contributing/adr/300-core-concepts/README.md)
    * [300 - Identity](contributing/adr/300-core-concepts/300-identity.md)
    * [301 - Hash](contributing/adr/300-core-concepts/301-hash.md)
  * [400 Smart Contracts](contributing/adr/400-smart-contracts/README.md)
    * [400 - Smart Contracts](contributing/adr/400-smart-contracts/400-smart-contracts-1.md)
    * [401 - P8e Metadata](contributing/adr/400-smart-contracts/401-p8e-metadata.md)
    * [402 - P8e Specifications](contributing/adr/400-smart-contracts/402-p8e-specifications.md)
    * [403 - P8e Smart Contracts](contributing/adr/400-smart-contracts/403-p8e-smart-contracts.md)
    * [404 - Omnibus](contributing/adr/400-smart-contracts/404-omnibus.md)
  * [500 Administration](contributing/adr/500-administration.md)
  * [600 Governance](contributing/adr/600-governance.md)
  * [700 Business Applications](contributing/adr/700-business-applications.md)
  * [800 System Migration](contributing/adr/800-system-migration.md)
* [Documentation Standards](contributing/documentation-standards.md)

## License

* [Apache 2.0](license/untitled.md)

## Glossary

* [Definitions](glossary/untitled.md)

## FAQ

* [Validator FAQ](faq/validator-faq.md)
* [Delegator FAQ](faq/delegator-faq.md)
* [Contact](faq/contact.md)

## Appendix

* [Blockchain Comparison](appendix/blockchain-comparison.md)
* [Contract Execution Examples](appendix/contract-execution-examples/README.md)
  * [Cross Scope Contract Example](appendix/contract-execution-examples/cross-scope-contract-example.md)
  * [Multi-Contract Example](appendix/contract-execution-examples/multi-contract-example.md)
  * [Multi-Step Contract Example](appendix/contract-execution-examples/multi-step-contract-example.md)

---

* [Use Cases](use-cases/README.md)
  * [Data Movement](use-cases/data-movement.md)
  * [External System Integration](use-cases/external-system-integration.md)
  * [Servicer Conversion](use-cases/servicer-conversion.md)

