---
description: Modules inherited from the community
---

# Inherited Modules

The Cosmos SDK, on which Provenance is based, provides several core modules.  Modules define most of the logic of SDK applications. Developers compose module together using the Cosmos SDK to build their custom application-specific blockchains - to which Provenance is an application-specific blockchain. 

Provenance inherits the following core Cosmos SDK Modules:

* [**Auth**](https://docs.cosmos.network/master/modules/auth/) - Authentication of accounts and transactions.
* \*\*\*\*[**Bank**](https://docs.cosmos.network/master/modules/bank/) - Token transfer functionalities.
* \*\*\*\*[**Capability**](https://docs.cosmos.network/master/modules/capability/) - Object capability implementation.
* \*\*\*\*[**Crisis**](https://docs.cosmos.network/master/modules/crisis/) - Halting the blockchain under certain circumstances \(e.g. if an invariant is broken\).
* \*\*\*\*[**Distribution**](https://docs.cosmos.network/master/modules/distribution/) - Fee distribution, and staking token provision distribution.
* \*\*\*\*[**Evidence**](https://docs.cosmos.network/master/modules/evidence/) - Evidence handling for double signing, misbehavior, etc.
* \*\*\*\*[**Governance**](https://docs.cosmos.network/master/modules/gov/) - On-chain proposals and voting.
* \*\*\*\*[**IBC**](https://docs.cosmos.network/master/modules/ibc/) - IBC protocol for transport, authentication and ordering.
* \*\*\*\*[**IBC Transfer**](https://docs.cosmos.network/master/modules/ibc/) - Cross-chain fungible token transfer implementation through IBC.
* \*\*\*\*[**Mint**](https://docs.cosmos.network/master/modules/mint/) - Creation of new units of staking token \(currently set to 0% inflation\).
* \*\*\*\*[**Params**](https://docs.cosmos.network/master/modules/params/) - Globally available parameter store for modules that supports Governance based configuration and management.
* \*\*\*\*[**Slashing**](https://docs.cosmos.network/master/modules/slashing/) - Validator punishment mechanisms.
* \*\*\*\*[**Staking**](https://docs.cosmos.network/master/modules/staking/) - Proof-of-Stake layer for public blockchains.
* \*\*\*\*[**Upgrade**](https://docs.cosmos.network/master/modules/upgrade/) - Software upgrades handling and coordination.

