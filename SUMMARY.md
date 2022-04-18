# Table of contents

* [Provenance Blockchain](README.md)

## Ecosystem

* [Provenance Blockchain: Financial Services Blockchain](ecosystem/financial-services-blockchain/README.md)
  * [Token Economics](ecosystem/financial-services-blockchain/token-economics.md)
  * [Fees/Distribution](ecosystem/financial-services-blockchain/distribution.md)
* [Governance](ecosystem/governance/README.md)
  * [Voting](ecosystem/governance/voting.md)
  * [Software Upgrade Proposals](ecosystem/governance/software-upgrade-proposal.md)
* [Foundation](ecosystem/foundation.md)
* [Community](ecosystem/community/README.md)
  * [Participants](ecosystem/community/participants.md)
  * [Validator](ecosystem/community/validator.md)
  * [Delegator](ecosystem/community/delegator.md)
  * [Asset Creators](ecosystem/community/asset-originators.md)
* [Funding Program](ecosystem/funding-program/README.md)
  * [Core Infrastructure Details](ecosystem/funding-program/core-infrastructure-details.md)
  * [Grants Program Details](ecosystem/funding-program/grants-program-details.md)

## Blockchain

* [Introduction](blockchain/introduction/README.md)
  * [Application Architecture](blockchain/introduction/application-architecture.md)
  * [Major Components](blockchain/introduction/major-components.md)
* [Basics](blockchain/basics/README.md)
  * [Anatomy of the Provenance Blockchain Application](blockchain/basics/anatomy-of-a-provenance-application.md)
  * [Transaction Lifecycle](blockchain/basics/transaction-lifecycle.md)
  * [Query Lifecycle](blockchain/basics/query-lifecycle.md)
  * [Accounts](blockchain/basics/accounts.md)
  * [Coin](blockchain/basics/stablecoin.md)
  * [Gas and Fees](blockchain/basics/gas-and-fees.md)
* [Installing Provenanced](blockchain/running-a-node/README.md)
  * [Running a Node](blockchain/running-a-node/running-a-node-1/README.md)
    * [Joining Testnet](blockchain/running-a-node/running-a-node-1/join-provenance-testnet.md)
      * [Running a mainnet node](blockchain/running-a-node/running-a-node-1/join-provenance-testnet/running-a-mainnet-node.md)
    * [Become a Validator](blockchain/running-a-node/running-a-node-1/become-a-validator.md)
    * [Configure a Sentry](blockchain/running-a-node/running-a-node-1/configure-a-sentry.md)
* [Using Provenanced](blockchain/using-provenance/README.md)
  * [Query Command](blockchain/using-provenance/query-command.md)
  * [Tx Command](blockchain/using-provenance/tx-command.md)
* [Explorer](blockchain/explorer/README.md)
  * [Walkthrough](blockchain/explorer/ui-walkthrough/README.md)
    * [Dashboard](blockchain/explorer/ui-walkthrough/dashboard/README.md)
      * [Blocks](blockchain/explorer/ui-walkthrough/dashboard/blocks.md)
    * [Staking (Validators)](blockchain/explorer/ui-walkthrough/staking-validators/README.md)
      * [Validator Details](blockchain/explorer/ui-walkthrough/staking-validators/validator-details.md)
    * [Transactions](blockchain/explorer/ui-walkthrough/transactions.md)
    * [Assets](blockchain/explorer/ui-walkthrough/assets.md)
    * [Governance](blockchain/explorer/ui-walkthrough/governance.md)
    * [Account (Address)](blockchain/explorer/ui-walkthrough/account-address.md)
    * [Forthcoming](blockchain/explorer/ui-walkthrough/forthcoming/README.md)
      * [Params](blockchain/explorer/ui-walkthrough/forthcoming/params.md)
      * [IBC](blockchain/explorer/ui-walkthrough/forthcoming/ibc.md)
      * [NFTs](blockchain/explorer/ui-walkthrough/forthcoming/nfts.md)
      * [Blockchain Statistics](blockchain/explorer/ui-walkthrough/forthcoming/blockchain-statistics.md)
  * [Wallet](blockchain/explorer/wallet.md)
  * [Explorer as a Service](blockchain/explorer/explorer-as-a-service/README.md)
    * [Ingestion](blockchain/explorer/explorer-as-a-service/ingestion.md)

## Modules

* [Inherited Modules](modules/inherited-modules.md)
* [Metadata](modules/metadata-module.md)
* [Marker](modules/marker-module.md)
* [Attribute](modules/account.md)
* [Name](modules/name-module.md)
* [Smart Contracts (ProvWasm)](modules/provwasm-smart-contracts.md)

## Contract Execution Environment <a href="#p8e" id="p8e"></a>

* [P8e Contract Execution Environment](p8e/overview/README.md)
  * [SDK](p8e/overview/api.md)
  * [Encrypted Object Store](p8e/overview/encrypted-object-store/README.md)
    * [EOS Encryption Scheme](p8e/overview/encrypted-object-store/encryption-scheme.md)
    * [DIME (Encryption Envelope Specification)](p8e/overview/encrypted-object-store/dime-encryption-envelope-specification.md)
* [P8e Usage](p8e/p8e-usage/README.md)
  * [Architecture](p8e/p8e-usage/architecture.md)
  * [Specifications](p8e/p8e-usage/specifications.md)
  * [Building New P8e Contracts](p8e/p8e-usage/building-new-contracts.md)
  * [Data Retrieval](p8e/p8e-usage/data-retrieval.md)
  * [Cross Scope (Update) Contract](p8e/p8e-usage/cross-scope-contract-example.md)
  * [Multi-Party Contract](p8e/p8e-usage/multi-contract-example.md)
  * [Multi-Step Contract](p8e/p8e-usage/multi-step-contract-example.md)
  * [Next Steps](p8e/p8e-usage/next-steps.md)

## Integrating

* [Integrating with p8e](integrating/integrating-with-p8e/README.md)
  * [Lending Ecosystem](integrating/integrating-with-p8e/lending-ecosystem/README.md)
    * [Life Cycle of a Loan](integrating/integrating-with-p8e/lending-ecosystem/life-cycle-of-a-loan.md)
    * [Data Mapping](integrating/integrating-with-p8e/lending-ecosystem/data-mapping.md)
  * [P8e Contract Execution Environment (p8e)](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/README.md)
    * [Deploying the Encrypted Object Store](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/deploying-the-encrypted-object-store/README.md)
      * [Configuring Replication](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/deploying-the-encrypted-object-store/configuring-replication.md)
    * [P8e Contracts](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/p8e-contracts/README.md)
      * [Onboard Loan Contract](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/p8e-contracts/example-loan-contracts.md)
    * [Key Management](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/key-management/README.md)
      * [Permissioning Others](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/key-management/permissioning-others.md)
    * [Summary](integrating/integrating-with-p8e/p8e-contract-execution-environment-p8e/summary.md)
  * [p8e CEE API](integrating/integrating-with-p8e/loan-onboarding-service/README.md)
    * [API Usage Guide](integrating/integrating-with-p8e/loan-onboarding-service/api-usage-guide/README.md)
      * [Loan Onboarding](integrating/integrating-with-p8e/loan-onboarding-service/api-usage-guide/loan-onboarding.md)
      * [Validation Request](integrating/integrating-with-p8e/loan-onboarding-service/api-usage-guide/validation-request.md)
      * [Validation Response](integrating/integrating-with-p8e/loan-onboarding-service/api-usage-guide/validation-response.md)
      * [Error Handling](integrating/integrating-with-p8e/loan-onboarding-service/api-usage-guide/error-handling.md)
    * [API Specification](integrating/integrating-with-p8e/loan-onboarding-service/api-specification/README.md)
      * [Configuration Endpoints](integrating/integrating-with-p8e/loan-onboarding-service/api-specification/configuration-endpoints.md)
      * [Object Store Endpoints](integrating/integrating-with-p8e/loan-onboarding-service/api-specification/object-store-endpoints.md)
      * [p8e Endpoints](integrating/integrating-with-p8e/loan-onboarding-service/api-specification/p8e-endpoints.md)

## Apps Built on Provenance Blockchain <a href="#provenance-applications" id="provenance-applications"></a>

* [Apps Overview](provenance-applications/apps-powered-by-provenance.md)
* [Figure LOS](provenance-applications/loan-origination-system-los/README.md)
  * [Figure Loan Model](provenance-applications/loan-origination-system-los/assets.md)
  * [Onboarding](provenance-applications/loan-origination-system-los/onboarding-contract.md)
  * [Funding](provenance-applications/loan-origination-system-los/funding.md)
  * [Validation](provenance-applications/loan-origination-system-los/loan-validation/README.md)
    * [3rd Party Digital Signatures](provenance-applications/loan-origination-system-los/loan-validation/3rd-party-digital-signatures.md)
  * [Onboard to Servicing](provenance-applications/loan-origination-system-los/loan-servicing.md)
  * [Data Sharing with Portfolio Manager](provenance-applications/loan-origination-system-los/data-sharing-with-portfolio-manager.md)
* [Figure eVault](provenance-applications/evault.md)

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

## Whitepapers

* [Loan Participation On Provenance](whitepapers/loan-participation-on-provenance.md)
* [Securitization On Provenance](whitepapers/securitization-on-provenance.md)
* [Blockchain Impact On Credit Ratings](whitepapers/blockchain-impact-on-credit-ratings.md)
* [Investment Fund Services on Provenance](whitepapers/investment-fund-services-on-provenance.md)
* [Supply Chain Finance on Provenance](whitepapers/supply-chain-finance-on-provenance.md)

## Provenance Blockchain FAQ <a href="#faq" id="faq"></a>

* [Foundation FAQ](faq/foundation-faq.md)
* [Grants Program FAQ](faq/grants-program-faq.md)
* [Ecosystem FAQ](faq/ecosystem-faq.md)
* [Blockchain FAQ](faq/blockchain-faq.md)
* [Hash Utility Token FAQ](faq/hash-utility-token-faq.md)
* [Validator FAQ](faq/validator-faq.md)
* [Delegator FAQ](faq/delegator-faq.md)
* [Client Contract Execution Environment (P8e) FAQ](faq/client-contract-execution-environment-faq.md)
* [Smart Contract FAQ](faq/smart-contract-faq.md)

## Appendix

* [History](appendix/blockchain-comparison.md)

## License

* [Apache 2.0](license/untitled.md)
