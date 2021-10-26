# Blockchain FAQ

## What’s the difference between Provenance Blockchain, the Cosmos Network, and the Cosmos Hub? <a id="whats-the-difference-between-provenance-the-cosmos-network-and-the-cosmos-hub"></a>

The Cosmos Network is a network of heterogeneous blockchains connected via the Cosmos Hub \(i.e. a router\) and a standard communication layer called the Inter-Blockchain Communication protocol \(i.e. TCP/IP\).

The Cosmos Hub was one of the first blockchains to be launched on the Cosmos Network. It acts as a router that facilitates token transactions between future Cosmos blockchains. To ensure security and to prevent double spending, the Cosmos Hub keeps track of the state of all connected blockchains. Its native utility-token is the ATOM.

The Provenance Blockchain is built with the Cosmos SDK and is therefore an application-specific Cosmos blockchain. Provenance Blockchain’s blockchain is constructed with specific modules that enable the Provenance Blockchain ecosystem to support sophisticated DeFi-applications. Provenance Blockchain’s native utility token is called Hash.

## On what consensus algorithm does Provenance Blockchain run? <a id="on-what-consensus-algorithm-does-provenance-run"></a>

The Provenance Blockchain uses Tendermint consensus algorithm and BPoS \(Bonded Proof of Stake\) to secure transactions.

## Can I store big files on the blockchain? <a id="how-can-i-store-big-files-on-the-blockchain"></a>

The size of the blocks on the Provenance Blockchain is limited to 22020096 bytes or about 20MB.

## Is Provenance Blockchain based on Bitcoin? <a id="is-provenance-based-on-bitcoin"></a>

No. Bitcoin is a different blockchain technology.

## Is Provenance Blockchain based on Ethereum 2.0? <a id="is-provenance-based-on-ethereum-2-0"></a>

No. Ethereum is a different blockchain technology.

## Is Provenance Blockchain based on Cosmos/Tendermint? <a id="is-provenance-based-on-cosmos-tendermint"></a>

Yes.The Provenance Blockchain network is built with the Cosmos SDK and therefore fully implements the Cosmos/Tendermint interfaces, RPCs, protocols, and consensus mechanism.

## What makes a Provenance Blockchain instance? <a id="what-makes-a-provenance-blockchain-instance"></a>

`provenanced` is the name of the Provenance Blockchain network. It is both the Provenance Blockchain Daemon and command-line interface \(CLI\). It runs a full node of the Provenance Blockchain application. `provenanced` is built on the Cosmos SDK using the following modules:

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

These Cosmos SDK modules are augmented by the following Provenance Blockchain custom module implementations:

x/attribute: Tagging addresses with information.

x/marker: Value and ownership structures.

x/metadata: NFT and asset-backed contract agreements.

x/name: Human readable address c\`ross-reference.

x/wasm: Smart Contracts.

Note: the list of those modules may change with future releases. For an up-to-date list refer to the following: [https://github.com/provenance-io/provenance/blob/main/app/app.go](https://github.com/provenance-io/provenance/blob/main/app/app.go)

In addition to the modules, so-called `genesis.json` and `config.toml` configuration files are required in which service rates are set and where references to seed-node are used to sync the blockchain state with other Provenance Blockchain instances.

