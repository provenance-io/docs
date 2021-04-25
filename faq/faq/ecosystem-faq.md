# Ecosystem FAQ

## **What is a hash-id**

**When external documents are referred to by the cryptographic hash of their contents in on-chain, immutable transactions, it extends the immutability to that off-chain information. The hash value of a document’s content is used as a globally unique identifier \(URI\) or name \(URN\) for that content and is typified by ‘hash-id’.**  


## **What’s the difference between Provenance, the Cosmos Network, and the Cosmos Hub?**

**The Cosmos Network is a network of heterogeneous blockchains connected via the Cosmos Hub \(i.e. a router\) and a standard communication layer called the Inter-Blockchain Communication protocol \(i.e. TCP/IP\).**  


**The Cosmos Hub was one of the first blockchains to be launched on the Cosmos Network. It acts as a router that facilitates token transactions between future Cosmos blockchains. To ensure security and to prevent double spending, the Cosmos Hub keeps track of the state of all connected blockchains. Its native utility-token is the ATOM.**  


**The Provenance blockchain is built with the CosmosSDK and therefore an application-specific Cosmos blockchain. Provenance’s blockchain is constructed with specific modules that enable the Provenance ecosystem to support sophisticated DeFi-applications. Provenance’s native utility-token is the Hash.** 

## **Who is the team developing the Provenance Network?**

**The Provenance Network is a project that is supported by the Provenance Foundation. Most of the initial software development was done by the team from Figure Technology Inc. Over time, in the spirit of community building, the development will become more decentralized. Please check out Provenance’s grants program and apply for funding to develop one of the PIPs.**

## **On what consensus algorithm does Provenance run?**

**Tendermint is the consensus engine that powers Cosmos BPoS \(Bonded Proof of Stake\) and therefore the Provenance blockchain network. \(ref?\)**

## **Where is the code?**

**All the Provenance ecosystem code is maintained within Github \(ref?\)**

## **What is the open source license used for the code?**

**The open source license for all the code is the Apache 2.0 license \(ref?\)**

## **How does Provenance fit into Cosmos’ Internet of Blockchains?**

**Cosmos’ FAQ reads:** 

**Cosmos is the blockchain pioneer of the Hub & Spoke architecture. Hubs act as routers for Zones. “Zones” are Cosmos-speak for “application-specific blockchains”. Each Zone is a spoke connected to a hub. Hubs can be connected with other hubs. Together, this ecosystem of connected Hubs and Zones makes up the Cosmos Network, or what is commonly referred to as the ‘Internet of the Blockchains’. Note that the Cosmos ecosystem is entirely permission-less, meaning that anyone can create a Hub or a Zone, and each blockchain is free to accept or refuse connections with other blockchains.**  


**Within this Cosmos Network, the Provenance blockchain network should be considered a “Zone”, i.e. application-specific blockchain. Note: in April 2021, the Inter-Blockchain Protocol \(ICB\), was truly bleeding edge technology, and Provenance was letting others kick the tires first, before truly connecting to the Cosmos Hub as a Zone through IBC.** 

## **Where can I find a tutorial of how to build a Provenance app?**

**Visit the Provenance Tutorials website to learn how to build your own Provenance app in minutes. \(ref?\)**

## **How can I become a Validator?**

**If you would like to become a validator for the Provenance blockchain network, please see the comprehensive guide in the Provenance blockchain network documentation. \(ref?\)**

## **How can I become a Delegator?**

**If you would like to become a delegator for the Provenance blockchain network, please see the comprehensive guide in the Provenance blockchain network documentation. \(ref?\)**

## **What is the Provenance SDK?**

**The Provenance SDK is a modular development framework that allows developers to easily build application-specific blockchains, i.e. sidechains \(‘Zones’ in Cosmos terminology\).**

## **Read more about the Provenance SDK here \(ref?\).**

**What is the difference between Hash, Hash 2.0 and Hash 1.0, and Provenance, Provenance 2.0 and Provenance 1.0?**

**Hash is short for Hash 2.0, and Provenance is short for Provenance 2.0. When Hash is referred to with a capital “H”, it’s the utility-token used to pay for the Provenance blockchain related services or the gas as it’s commonly called. Hash is equivalent to, but not the same as, the Atom tokens used for the CosmosHub services.  
Provenance’s first generation blockchain service was based on HyperLedger and is referred to as Provenance 1.0. Hash 1.0 was the legacy currency used in that Provenance 1.0 blockchain. The terms Provenance 1.0 and Hash 1.0 will soon have only historical significance and probably will make its original developers a bit teary eyed.**

## **What is the EOS or Encrypted Object Store?**

**For many financial applications, many of the relevant documents, data and contracts are confidential and private. To facilitate that requirement, the Provenance ecosystem includes services that make it easy to refer to confidential documents by its hash-id, and encrypt and store those documents in a database where they are indexed by their hash-id. The encryption-key is specific for the owner or client of that document. When a contract requires the exchange of such a document, the service will re-encrypt the document with a key associated with the business partner receiving that document.**   


## **What is the Client Contract Execution Environment or CCEE or P8E?**

**The Client Contract Environment \(CCEE or P8E\) is a framework that is part of the Provenance ecosystem. The CCEE includes components that facilitate the management of contracts and its associated documents both on-chain and off-chain. One component deals with the secure communication between business partners to exchange information required to fulfill the contract. This communication flows through an asynchronous mail-box system where all messages are authenticated and encrypted by the two parties’ keys.**

**In addition, the CCEE includes facilities to easily refer to documents by their hash-id, and to encrypt and store those docs in an Encrypted Object Store \(EOS\). Those encrypted documents are indexed by their hash-id and optionally by selected document attribute values. The latter allows one to aggregate documents based on common attributes without revealing their sensitive content.**

## **What does P8E stand for?**

**P8E is the internal code name for Provenance’s Client Contract Execution Environment. The abbreviation is trivial for all those who worked on it, but to spell it out for the less fortunate: the word ProvenancE starts with a “P”, then “8” letters and ends with “E” - “P8E”.**

## **How secure is this Provenance blockchain network?**

**Although there are no guarantees in security, the Provenance developers put a lot of effort in following best practices in coding and operations. In addition, NCCGroup did a security assessment and audit of the Provenance services, and all its critical findings have been addressed. NCCGroup’s report can be read here \(ref?\). The Cosmos SDK is scheduled to go through a similar review \(ref?\). The Provenance funding program will sponsor ongoing security assessments and code audits on a regular basis to adhere to security best practices. The foundation will also sponsor a bug bounty program to find security vulnerabilities and further improve the code.**  


## **The Provenance Grants Program**

**How much money is available for the grants program?**

**There are no upper or lower limits set on the funding of individual improvement proposals. The governing board and members of the foundation will look at the proposal’s merit and vote on its funding.**

## **What is the decision process for awarding a project with a grant?**

**The exact decision process is still TBD.**  


## **Are the grants limited to US-only organizations?**

**The grants are not limited to US-only organizations.**  


## **The Provenance Foundation**

## **How is the foundation funded?**

**…**  


## **Where is the foundation incorporated?**

**The Provenance foundation is incorporated as a non-profit in Delaware.**  


## **Who are the foundation’s members?**

**Any entity with staked Hash, is a member of the foundation.**  


## **Where can I find the bylaws?**

**The foundation’s bylaws are here. \(ref?\)**  


## **Where can I get Developer support?**

**Check out the Provenance Developer Chat and the Provenance Github repos for technical support from the growing community of Provenance developers.**  


## **How can I get in touch with the Provenance Foundation?**

**Please email hello@provenance.io**  


## **What is the Provenance roadmap? What are the phases of Provenance?**

**Can I “buy” Provenance’s Hash utility-tokens?**

## **Who is developing Provenance?**

## **How can I store big files on the blockchain?**

**The size of the blocks on the Provenance blockchain is limited to 22020096 bytes or about 20MB.**

## **Is Provenance based on Bitcoin?**

**No. Bitcoin is a different blockchain technology.**

## **Is Provenance based on Ethereum 2.0?**

**No. Ethereum is a different blockchain technology.**

## **Is Provenance based on Cosmos/Tendermint?**

**Yes.The Provenance blockchain network is built with the Cosmos SDK and therefore fully compatible with Cosmos/Tendermint interfaces, RPCs, protocols, and consensus mechanism.**

## **What makes a Provenance blockchain instance?**

**provenanced is the name of the Provenance SDK application for the Provenance blockchain network, a Cosmos Zone. It is both the Provenance Daemon and command-line interface \(CLI\). It runs a full-node of the provenance application. provenanced is built on the Cosmos SDK using the following modules:**  


**x/auth: Accounts and signatures.**

**x/auth/ante**

**x/bank: Token transfers.**

**x/capability**

**x/crisis**

**x/distribution: Fee distribution logic.**

**x/evidence**

**x/genutil**

**x/gov: Governance logic.**

**x/ibc: Inter-blockchain communication.**

**x/mint: Inflation logic.**

**x/params: Handles app-level parameters.**

**x/slashing: Slashing logic.**

**x/staking: Staking logic.**

**x/upgrade**  


**These Cosmos SDK modules are augmented by the following Provenance custom module implementations:**  


**x/attribute: Tagging addresses with information.**

**x/marker: Value and ownership structures.**

**x/metadata: NFT and asset-backed contract agreements.**

**x/name: Human readable address cross-reference.**

**x/wasm: Smart Contracts.**  


**Note: the list of those modules may change with every new release. For an up to date list refer to the Github repo code: “**[**https://github.com/provenance-io/provenance/blob/main/app/app.go**](https://github.com/provenance-io/provenance/blob/main/app/app.go)**”**  


**In addition to the modules, so-called genesis.json and config.toml configuration files are required in which Hash is defined as the gas-currency and the service rates are set, and where references to seed-node are used to sync the blockchain state with other provenance-blockchain instances.**

## **What’s the difference between account and “wallet contract”?**

**...**

## **Are keyfiles only accessible from the computer you downloaded the client on?**

**...**

## **How long should it take to download the blockchain?**

**...**

## **How do I get a list of transactions into/out of an address?**

**...**

## **Smart Contracts**

## **Does the  Provenance blockchain support smart-contracts?**

**Yes, the Provenance blockchain includes ProvWasm, an enhanced version of the CosmWasm smart-contract module. It allows smart-contract authors to use the Rust language to code their smart-contract, which subsequently gets compiled to Wasm and executed. For details see here. \(ref?\)**

## **What kind smart-contracts does Provenance support?**

## **Where do the contracts reside?**

## **Can a contract pay for its execution?**

## **Can a contract call another contract?**

## **Can a transaction be signed offline and then submitted on another online device?**

## **Can a transaction be sent by a third party? i.e can transaction broadcasting be outsourced**

## **Can Provenance smart-contracts pull data using third-party APIs?**

## **Is the content of the data and contracts sent over the Provenance network encrypted?**

## **Can I store secrets or passwords on the Provenance network?**

## **How will Provenance ensure the network is capable of making 10,000+ transactions-per-second?**Can I become who I want to be?





