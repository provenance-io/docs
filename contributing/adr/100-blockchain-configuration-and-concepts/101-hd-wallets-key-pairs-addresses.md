---
description: >-
  An overview of the specific encryption curves and structures used in the
  blockchain platform. Covers how addresses are derived from public keys and the
  structure of recording addresses on chain.
---

# 101 - HD Wallets, Key Pairs, Addresses

## Changelog

* 20200416: Initial version.

## Context

Within the blockchain platform there are an extensive number of public/private key pairs used to perform signatures and encryption. These key pairs could easily become incredibly difficult to manage due to proliferation. With this in mind we adopt the techniques of Hierarchical Deterministic wallets which allows a single seed to be used with an established set of key derivation processes to yield a large collection of crypto material to support required security contexts without needing to secure each generated key individually. Further each lower level key is severable into separate security contexts when a need for delegated control is encountered.

In addition to internal uses for keys there is a requirement to support non-custodial wallet applications. In these contexts an external entity will provide their own security for their private keys. In order to support this process they may wish to delegate an isolated key pair to a system to use on their behalf potentially without the ability to sign transactions at all.

The Provenance Blockchain platform will require an address for each user or context as well as the ability for these users to generate additional addresses easily to provide isolation and separation of their transactions and assets on the blockchain.

## Decision

Provenance Blockchain will use the HD wallet standards established and proven by the Bitcoin and Ethereum communities for the creation of all user keys. No secp256k1 key pairs will be created that are standalone outside of this process. Provenance Blockchain will use the 24 word mnemonic standard length \(256byte\) to generate seed values for deriving all secp256k1 keys. The specific industry standards used for this process will be outlined in the [wallet](../spec/wallets.md) and [address](../address.md) specification documents.

The default user/context level account will be the first \(`0`\) account in the heirarchy and the associated address for this user/context will be the first address level key pair \(`0`\) within the wallet. This address is addressable via \[`m/44'/505'/0'/0/0`\]. While the standards support the concept of external and internal \(or change\) address level keys for the Provenance Blockchain only the external address level keys will be used.

With the standardization of key pairs and generation, each security/user context will be proved through the use of digital signatures.

The unique identifier for each user/context is implicitly the public key of the signature used. As the public key types used may change over time a special derivative identifer called an `address` based on the public key will be used. This specially formatted address replaces the use of UUID/GUID values for identity due to the intrinsic link between the signing key and the identifier which can not be reassigned.

## Status

**ACCEPTED**

_Note: The hierarchical wallet specifications are considered accepted as their use is required for signing all transactions within the Provenance Blockchain._

## Consequences

External systems that work with the Provenance Blockchain will need to evolve to use a new concept of identity and security contexts. These contexts will rely on security controls based on cryptography using processes that may ceed control back to the external users is some cases.

### Positive

* Keys can be created by a client and held within their owned hardware wallet.
* An address can be created by a client deterministically without calling the blockchain.
* Unlike UUIDs the address identifier is intrinsically tied to a specific key pair/security context.

### Negative

* Significant additional complexity required to support offline transaction signing.
* The compromise of a Seed value or one of the higher level extended key pairs can result in the compromise of all the user/content key pairs.
* Incorrect use and sharing of lower level "unhardened" public keys can reveal the private key when combined with certain extended public key information.

### Neutral

* Requires refactoring existing systems to rely on 'addresses' as the unique identifier pointing to a key pair context.

