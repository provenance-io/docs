---
description: >-
  Provenance Blockchain is an ecosystem for application-specific financial services
  blockchain implementations.
---

# Application Architecture

When developing applications on Provenance Blockchain it is important to understand the underlying application architecture.  

### High Level Architectural Components

As is typical with most application architectures there are 3 well-defined layers: a user-facing interface, middleware to implement the application business components, and a foundational blockchain network infrastructure that supports the sophisticated requirements of the higher layers.

![](../../.gitbook/assets/image%20%281%29.png)



#### Interface Layer

This layer provides the traditional user interface for interacting with Provenance-based applications.  Marketplaces and Exchanges are typical use cases where users buy, sell, and trade things of value.  These "things of value" include asset-backed securities, cryptocurrency, or tokenized assets.

An important component of the interface layer is the [Wallet](../../contributing/adr/100-blockchain-configuration-and-concepts/101-hd-wallets-key-pairs-addresses.md).  Entities \(organizations, systems, or users\) must have an _account_ to conduct business on the Provenance Blockchain. An account is represented by the public key portion of a public and private key-pair. Accounts contain a uniquely identified address, which simply a string value derived from the entity’s public key following the [Bech32](https://en.bitcoin.it/wiki/Bech32) format, thus providing standard blockchain pseudonymity.

Consider a Hash transfer transaction where Alice is a Hash holder and Bob is the recipient.  Alice holds 100 Hash at address `tp1kaczxflvhq4700r0ntdnqlxpu80xdp869seh9e` \(again, address is derived from the public key portion of a key pair that Alice holds\).  Bob also has generated a key pair and an address from the public key portion of the key pair `tp18839rhfk0ql7mdqgn27eeaesmfr9ckpajssuc4`.  

Before Alice can send a transfer transaction request to the Provenance Blockchain she must sign the transfer transaction.  Alice uses the private key portion of her key pair to sign the transaction and submits the transaction to the blockchain.  Provenance Blockchain validates the transfer transaction signature against Alice's public key, verifies that she has an account on Provenance Blockchain, and that the address holds enough Hash to pay transaction fees and to transfer to Bob.

A Wallet is where Alice and Bob store their key pairs. The Wallet is used to manage key pairs, addresses, and the token-values the addresses hold.  Typically, and when using the Provenance Blockchain Wallet, [HD Wallets](https://en.bitcoin.it/wiki/BIP_0032) are used.  HD Wallets allow entities to create a root mnemonic seed and then derive child keys from the root that can be used to hold value on Provenance Blockchain.  For example, Alice's address `tp1kaczxflvhq4700r0ntdnqlxpu80xdp869seh9e` is one of many keys in her HD Wallet.  Entities can import, export, or regenerate their Wallet \(and any subsequent derived addresses\) using the root mnemonic seed.  The root mnemonic seed is a secret value that the entity controls and never exposes to the Provenance Blockchain ecosystem.

#### Middleware

The middleware layer is where organizational business processes are defined.  These business processes include the establishment and orchestration of business value.  For example, originating a loan is an action that creates a value marker.  Utilizing a stable coin marker for a payment application also creates and uses value markers.

The middleware layer is the bridge between the Provenance Blockchain and the financial services business logic.  The middleware layer connects directly to a Provenance Blockchain node to invoke transactions, query transactions, and listen for events.

The middleware layer is also where entities leverage the Provenance Blockchain client-side [Contract Execution Environment](../../p8e/overview/) for the onboarding and management of private and sensitive information.  The Contract Execution Environment is a client-side solution that provides the ability for entities to exchange private data yet still leverage the ownership, immutability and value benefits provides by the Provenance Blockchain.

#### Provenance

The Provenance Blockchain layer is the Provenance Blockchain network and provides:

* A persistent, distributed, immutable, and replicated deterministic state machine
* The networking and consensus layers of the Provenance Blockchain including transaction ordering and consensus
* Value and ownership markers leveraged middleware business applications including coins, cryptocurrency, and tokenization
* Exchange, trading, and settlement of value markers and bridges to fiat currency
* Bi-lateral exchange implementations using Smart Contracts
* Blockchain primitives such as account authorization, governance, staking, voting, gas and fee processing, telemetry, and node configuration.

