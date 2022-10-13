---
description: Modules inherited from the community
---

# Inherited Modules

The Cosmos SDK, on which the Provenance Blockchain application is based, provides several core modules.  Those modules define most of the logic of blockchain applications. Developers compose modules together using the Cosmos SDK to build their custom application-specific blockchains - our Provenance Blockchain is such an application-specific blockchain. 

The Provenance Blockchain inherits the following core Cosmos SDK Modules:

* [**Auth**](https://docs.cosmos.network/main/modules/auth/) - Authentication of accounts and transactions.
* \*\*\*\*[**Bank**](https://docs.cosmos.network/main/modules/bank/) - Token transfer functionalities.
* \*\*\*\*[**Capability**](https://docs.cosmos.network/main/modules/capability/) - Object capability implementation.
* \*\*\*\*[**Crisis**](https://docs.cosmos.network/main/modules/crisis/) - Halting the blockchain under certain circumstances \(e.g. if an invariant is broken\).
* \*\*\*\*[**Distribution**](https://docs.cosmos.network/main/modules/distribution/) - Fee distribution, and staking token provision distribution.
* \*\*\*\*[**Evidence**](https://docs.cosmos.network/main/modules/evidence/) - Evidence handling for double signing, misbehavior, etc.
* \*\*\*\*[**Governance**](https://docs.cosmos.network/main/modules/gov/) - On-chain proposals and voting.
* \*\*\*\*[**IBC**](https://docs.cosmos.network/main/modules/ibc/) - IBC protocol for transport, authentication and ordering.
* \*\*\*\*[**IBC Transfer**](https://docs.cosmos.network/main/modules/ibc/) - Cross-chain fungible token transfer implementation through IBC.
* \*\*\*\*[**Mint**](https://docs.cosmos.network/main/modules/mint/) - Creation of new units of staking token \(currently set to 0% inflation\).
* \*\*\*\*[**Params**](https://docs.cosmos.network/main/modules/params/) - Globally available parameter store for modules that supports Governance based configuration and management.
* \*\*\*\*[**Slashing**](https://docs.cosmos.network/main/modules/slashing/) - Validator punishment mechanisms.
* \*\*\*\*[**Staking**](https://docs.cosmos.network/main/modules/staking/) - Proof-of-Stake layer for public blockchains.
* \*\*\*\*[**Upgrade**](https://docs.cosmos.network/main/modules/upgrade/) - Software upgrades handling and coordination.

