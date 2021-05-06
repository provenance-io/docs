# Blockchain FAQ

## What’s the difference between Provenance, the Cosmos Network, and the Cosmos Hub? <a id="whats-the-difference-between-provenance-the-cosmos-network-and-the-cosmos-hub"></a>

The Cosmos Network is a network of heterogeneous blockchains connected via the Cosmos Hub \(i.e. a router\) and a standard communication layer called the Inter-Blockchain Communication protocol \(i.e. TCP/IP\).

The Cosmos Hub was one of the first blockchains to be launched on the Cosmos Network. It acts as a router that facilitates token transactions between future Cosmos blockchains. To ensure security and to prevent double spending, the Cosmos Hub keeps track of the state of all connected blockchains. Its native utility-token is the ATOM.

The Provenance blockchain is built with the CosmosSDK and therefore an application-specific Cosmos blockchain. Provenance’s blockchain is constructed with specific modules that enable the Provenance ecosystem to support sophisticated DeFi-applications. Provenance’s native utility-token is the Hash.

## On what consensus algorithm does Provenance run? <a id="on-what-consensus-algorithm-does-provenance-run"></a>

Tendermint is the consensus engine that powers Cosmos BPoS \(Bonded Proof of Stake\) and therefore the Provenance blockchain network. \(ref?\)

## Can I store big files on the blockchain? <a id="how-can-i-store-big-files-on-the-blockchain"></a>

The size of the blocks on the Provenance blockchain is limited to 22020096 bytes or about 20MB.

## Is Provenance based on Bitcoin? <a id="is-provenance-based-on-bitcoin"></a>

No. Bitcoin is a different blockchain technology.

## Is Provenance based on Ethereum 2.0? <a id="is-provenance-based-on-ethereum-2-0"></a>

No. Ethereum is a different blockchain technology.

## Is Provenance based on Cosmos/Tendermint? <a id="is-provenance-based-on-cosmos-tendermint"></a>

Yes.The Provenance blockchain network is built with the Cosmos SDK and therefore fully compatible with Cosmos/Tendermint interfaces, RPCs, protocols, and consensus mechanism.

## What makes a Provenance blockchain instance? <a id="what-makes-a-provenance-blockchain-instance"></a>

provenanced is the name of the Provenance SDK application for the Provenance blockchain network, a Cosmos Zone. It is both the Provenance Daemon and command-line interface \(CLI\). It runs a full-node of the provenance application. provenanced is built on the Cosmos SDK using the following modules:

x/auth: Accounts and signatures.

x/auth/ante

x/bank: Token transfers.

x/capability

x/crisis

x/distribution: Fee distribution logic.

x/evidence

x/genutil

x/gov: Governance logic.

x/ibc: Inter-blockchain communication.

x/mint: Inflation logic.

x/params: Handles app-level parameters.

x/slashing: Slashing logic.

x/staking: Staking logic.

x/upgrade

These Cosmos SDK modules are augmented by the following Provenance custom module implementations:

x/attribute: Tagging addresses with information.

x/marker: Value and ownership structures.

x/metadata: NFT and asset-backed contract agreements.

x/name: Human readable address cross-reference.

x/wasm: Smart Contracts.

Note: the list of those modules may change with every new release. For an up to date list refer to the Github repo code: “[https://github.com/provenance-io/provenance/blob/main/app/app.go](https://github.com/provenance-io/provenance/blob/main/app/app.go)”

In addition to the modules, so-called genesis.json and config.toml configuration files are required in which Hash is defined as the gas-currency and the service rates are set, and where references to seed-node are used to sync the blockchain state with other provenance-blockchain instances.

