---
description: The evolution of the Provenance Blockchain from 1.0 to 2.0
---

# History

## Provenance Blockchain 1.0

Provenance Blockchain is the leading production blockchain protocol for the financial services industry. It is a permissioned, proof of stake protocol that acts as a global ledger, registry and exchange across assets and markets.

Provenance Blockchain Platform builds on a strong core of open source including a diverse set of messaging, networking, systems orchestration and blockchain technology. The initial process of developing the platform included an in depth evaluation of many different open source projects for potential use in the core of Provenance Blockchain.

Software projects were evaluated against several different criteria including:

* Health and vibrancy of the open source community surrounding the project
* Source code quality and software engineering principles
* Flexibility of the software for handling current and future business needs
* Secure, scalable infrastructure capable of providing permissioning, privacy, and integrity for a diverse set of stakeholder transactions.

While the industry has made advancements in the last 18 months since our choices were made, the core differences are still present.

### Quorum

The Quorum blockchain project is a fork of the open source Ethereum blockchain. The Quorum platform retains close alignment with its public upstream parent in order to leverage the large Ethereum community.\[1\] The decision to base the project off of a large and well tested public blockchain is intended to inherit the careful security testing and high code quality of the parent project. As a high profile public blockchain Ethereum is subject to all of the idiosyncrasies of various user groups, high transaction volumes, and potentially malicious actors.

With such a powerful open source community at its heart Quorum would seem to be a strong choice for a private financial platform. Unfortunately this strong core can result in conflicting design choices if the goals between the two are not well aligned. An evaluation of the technical underpinnings reveals potential concerns to this effect.

#### Private Transactions

A global financial ledger must accommodate separate contexts for different user groups that ensure privacy and security of data. As a blockchain the permanent immutable record further places constraints on how data is stored and protected long term. As an additional complexity consumers may demand certain personally identifiable information is purgeable regardless of the immutable nature of a global ledger.

These privacy requirements for transactions are a unique constraint that Quorum must satisfy above and beyond what the Ethereum project provides. To satisfy these requirements Quorum has built infrastructure to support private encrypted data stored off chain. As the Ethereum core does not include an interface to integrate these features, Quorum was forced to inject their feature through a special signature id value\[2\]. Further, in the interest of keeping these two projects in close alignment the private transaction feature does not make any changes to the basic structure of a transaction forsaking the opportunity to relax the data storage constraints.

#### Data Storage and Memory

The Ethereum virtual machine concept was designed to allow participants in the Ethereum public network to run smart contracts containing arbitrary code from other users in a safe way on their systems. There are many restrictions in place to prevent malicious actors from performing denial of service attacks or otherwise abusing the system. These constraints require software developers to understand the limits and how to work within them \[4\].

The nature of Ethereum and its control over the Quorum project has resulted in bug fix and change requests to make improvements or at least allow process limits to be configurable; requests languish for years without resolution \[5\]. The overall transaction size limit for Quorum was doubled from 32KB in Ethereum to 64KB. \[6\] This limit is still quite low for operations such as a loan sale where thousands of loans may be aggregated in a single transaction. It is also worth noting that any change to the core Ethereum code base that must be done in the Quorum fork invalidates many of the unit tests and security checks that are part of the core benefit to building on the Ethereum blockchain base.

#### Review

The Quorum blockchain project is an interesting approach to solving the needs of a distributed financial ledger. The effort involved in creating a full private transaction system for smart contracts is commendable and well constructed. Unfortunately this project is hampered by its roots on the public Ethereum project which drives requirements towards the public blockchain and has few resources in design or maintenance towards supporting the private network needs and requirements.

### Corda / R3

If Quorum is an example of leveraging a powerful open-source blockchain and bending it towards the needs of a private permissioned network, Corda and the R3 consortium makes a good example of the opposite approach. Starting with a committee of industry members, Corda has built a custom solution from the ground up. \[7\] To further distinguish Corda from other projects in the space, it is not actually a blockchain at all. The premise of Corda is a system for independent ledgers maintained by each entity to transact together on an as needed basis. \[8, p7\]

#### Determinism

As Corda is not a distributed blockchain it cannot rely upon multiple parties to perform parallel executions of smart contracts to ensure consistency. Corda attempts to ensure determinism through a special limited JVM. \[8, p44\] This approach strikes a balance between flexible implementation and constraints to ensure reliable deterministic smart contract execution. In contrast Hyperledger Fabric relies on Kernel level process isolation in the form of containers to provide a much stronger guarantee of separation than the JVM. The container isolation combined with distributed parallel execution achieves determinism guarantees without resorting to limiting development to programs that run on a special limited JVM.

The Corda consensus model is implemented through Notaries with two different levels of transaction safety. At the lowest level a Notary simply determines that the input states for a transaction have not been previously used \(aka double spend in a UTXO\). A higher level of safety can be achieved with a ‘validating notary’ with the forfeit of transaction privacy. \[9\]

#### System Architecture

With the Corda architecture starting from a single entity and progressing towards collaboration it is not surprising that design constraints associated with multiple parties and scalable cloud infrastructure have a lower priority. For components of the system that perform scalable conflict resolution \(within the notaries\) the distributed consensus algorithms remain experimental and not suitable for production use. \[10\] The implementation of the Corda node itself lacks fault tolerance and high availability features \[11\] which have remained on the long term planning roadmap as future items.

When building a distributed application it is important to adopt many patterns and practices from the start to ensure a successful overall system. \[12\] Currently Corda lacks a standard cloud native deployment configuration. A cloud native application architecture must account for unreliable instances that can come or go at any time leading to a stronger focus on consistency and reliability engineering. If Corda deployments targeted a standard cloud native platform such as Kubernetes then testing against the constraints of a system running in the cloud would take a higher priority. Presently these issues \[11\] must be resolved entirely by the implementer of the system until such time as the Corda project matures and implements these pieces of their roadmap.

#### Review

Corda represents a unique approach to solving the ledger problem by building many distributed ledgers and working towards an ad hoc aggregation to create an emergent global blockchain. Cloud native infrastructure constraints and the creation of a larger multi-stakeholder network remain future items or are not considered priorities at all favoring a robust single client node approach.

### Hyperledger Fabric

The Hyperledger group was founded in 2016 under the Linux Foundation with a number of industry partners. \[13\] \[14\] As part of Hyperledger, Fabric is developed to serve as a flexible enterprise blockchain platform for many different uses including supply chain and financial systems. As part of supporting diverse use cases, Fabric is focused on a flexible and configurable platform for building permissioned distributed ledgers. Hyperledger Fabric uses a unique approach of ‘execute, order, validate’ to validate transactions which allows for multiple parties to execute a contract to ensure determinism and to increase performance by distributing execution.

#### Flexible Architecture

One of the most important features for developing the Provenance Blockchain platform is the ability to rapidly build diverse solutions in response to business opportunities. The platform must be adaptable through configuration and custom extensions in a maintainable fashion. \[16, p3\] An additional consideration to the flexible architecture are the requirements for developing applications on the blockchain using smart contracts. Fabric allows many different languages to be used with major SDKs for Go, Java, and NodeJS as official offerings as well as a well defined communication layer supporting the community development of additional languages.

#### Roadmap

The Fabric project has a well defined set of community guidelines for steering the project including a defined release cadence and the concept of long term releases for production stability. \[17\] The Fabric project has been making significant progress on expanding both new reference applications such as FabToken as well streamlining administration of smart contracts and system versioning. \[18\] The combination of new features as well as a dedicated approach to refining administration tasks required for large scale deployments indicate a stable direction for enterprises to adopt.

#### Review

The Hyperledger Fabric platform provides a modular system with a diverse set of applications supported as primary use cases. The unique execute-order model streamlines the scaling of smart contract execution while resolving issues of determinism without requiring users to redevelop their business logic in a limited environment or translation into a special purpose language. The project roadmap shows near-term and long-term features are centered around stability and streamlining the management of large scale enterprise networks.

### Provenance Blockchain 1.0 Platform

The Provenance Blockchain Platform is more than just another blockchain; it is an entire system architecture to support financial industry transactions. Provenance Blockchain has built up infrastructure to monitor, manage, and maintain a large scale distributed platform across several major cloud providers. Expert integration of many different open source products along with the expertise to directly extend these capabilities beyond what is directly offered has resulted in a uniquely capable and stable blockchain platform.

#### Extensions

A global distributed system for financial transactions on blockchain requires more than just installing software and turning it on. A comprehensive system design must account for many different factors including; monitoring, anomaly detection, incident response, software deployment, failover testing, security audits, and capacity planning constraints among many others. Engineering solutions for these constraints requires specific targeted extensions for any basic blockchain software installation.

The first step to leveraging the Hyperledger Fabric project was the creation of an entirely new cloud native smart contract execution management system allowing for secure deployment inside Kubernetes clusters. This was combined with strong encryption controls based on client managed keys for control of data access and long term secure storage on chain.

* Kubernetes Cloud Platform – The default scheduling components within Hyperledger Fabric are not well integrated with cloud native platforms such as Kubernetes. While future support is on the long term roadmap, Provenance Blockchain has leveraged the modular Fabric architecture to integrate a more secure Kubernetes scheduler that streamlines software release workflows and allows distribution of workloads across nodes in the cluster.
* Secure private networking for public clouds–The distribution of the Provenance Blockchain network between stakeholders in many different public clouds would traditionally require exposing services over the internet along with a complex series of networking rules and configuration. Through the use of a novel Wireguard networking setup Provenance Blockchain member nodes can communicate securely as one large blockchain network internally while exposing business focused applications externally for a streamlined architecture and minimized public attack surface.
* Client to client private transactions on the global ledger–Building on the Execute-Order model of Hyperledger Fabric, Provenance Blockchain has developed a unique approach to private transaction execution allowing specially instrumented transactions to execute in parallel between private parties securely for deterministic behavior before the results are forwarded to the global ledger. This approach provides benefits for privacy, data control and increased performance while maintaining the consistency controls of a global ledger.

#### Production Environment

The Provenance Blockchain platform has been developed with a strong focus on reliability and availability. Continuous improvement in software and the rapid release of new capabilities are made possible through the use of best practices such as Infrastructure as Code. \[19\] Provenance Blockchain has developed repeatable processes for all of the management activities associated with the upgrade and rollout of enterprise blockchain components. The Provenance Blockchain environment includes redundant systems for monitoring and responding to infrastructure issues from the routine –managing disk space, to advanced tasks such as deploying a smart contract hotfix quickly across all network nodes.

### Stellar

While the Provenance Blockchain Platform, Ethereum/Quorum, and Corda/R3 are all focused on building a general-purpose platform for smart contract execution there are other blockchains that focus on only part of the ecosystem. Stellar is one example of a single purpose blockchain focused on value exchange using a custom token known as Lumen \(XLM\).

#### Assets and Tokens

Within the Stellar network the concept of an asset \[20\] is represented by tokens issued from an account and is limited to numerations of value held by parties associated with these accounts. Stellar proclaims that any type of asset can be tracked or transferred on the platform however this capability is limited to issuing and exchanging value between Stellar accounts.

#### Smart Contracts

Stellar supports a limited form of smart contract that allows value to be programmatically exchanged between multiple parties. These Stellar smart contracts are not intended to be a Turing complete language for codifying business processes. A Stellar smart contract is based on 13 different operation types that the blockchain supports. \[21\] These limitations on smart contracts prevent the extension of the Stellar platform to support the various custom business logic processes that Provenance Blockchain leverages to implement products on blockchain.

### Inherited Blockchain

Another popular way of extending limited ‘token focused’ blockchains is through the use of metadata or memo fields to contain references to an external chain and thus inherit the security benefits of the parent chain. The Bitcoin network has a special transaction field called OP\_RETURN which has been leveraged to build highly secure Proof of ‘Proof of Work’ chain \[22\]. Unfortunately, the Stellar blockchain has a limit of 32 bytes of metadata on a given transaction which renders the approaches used for Bitcoin impossible.

#### Review

The Stellar blockchain provides a useful method of exchanging currencies and provides methods to extend the concept of a currency to an asset. The Provenance Blockchain Platform provides a similar set of features for fiat exchange through the use of an Omnibus bank system.

Unfortunately due to limits in the design of Stellar the capabilities beyond an Omnibus required for the Provenance Blockchain platform are not possible. The Provenance Blockchain platform can not be built on a public network focused on token exchange as these features represent only a small part of the Provenance Blockchain ecosystem.

### Conclusions

Provenance Blockchain includes a core built on strong open source technologies combined with engineering expertise to extends these capabilities far beyond the sum of their parts. The unique extensions and combination of tools yield a best of breed financial services blockchain platform. Individually the major blockchain projects mentioned in this paper have a limited capability for maintaining themselves in isolation or in some cases for small network deployments.

Public blockchains or projects heavily derived from these such as Quorum are constrained by a conflicting set of goals between the primary public uses and the derivative private blockchain. Alternative projects similar to Corda that build up a solution focused on the individual users leave hard problems of global consensus, networking, and cloud reliability engineering for future resolution or users to solve themselves. In contrast the Provenance Blockchain platform builds on the flexibility of the Hyperledger Fabric platform with carefully engineered extensions to offer a global platform for financial blockchain services. Provenance Blockchain continues to invest in the platform to ensure long term reliability and capability for current and future financial applications.

1. Quorum Whitepaper; [https://github.com/jpmorganchase/quorum/blob/master/docs/Quorum%20Whitepaper%20v0.2.pdf](https://github.com/jpmorganchase/quorum/blob/master/docs/Quorum%20Whitepaper%20v0.2.pdf)
2. Quorum Privacy Manager Design; [https://goquorum.readthedocs.io/en/latest/\#design](https://goquorum.readthedocs.io/en/latest/#design)
3. Quorum Roadmap; [https://github.com/jpmorganchase/quorum/wiki/Product-Roadmap](https://github.com/jpmorganchase/quorum/wiki/Product-Roadmap)
4. Ethereum VM Memory Usage; [https://ethereum.stackexchange.com/questions/23720/usage-of-memory-storage-and-stack-areas-in-evm](https://ethereum.stackexchange.com/questions/23720/usage-of-memory-storage-and-stack-areas-in-evm)
5. Ethereum Contract Code Size Limit; [https://github.com/ethereum/EIPs/issues/659](https://github.com/ethereum/EIPs/issues/659)
6. Quorum Transaction Size Increase; [https://github.com/jpmorganchase/quorum/pull/636](https://github.com/jpmorganchase/quorum/pull/636)
7. R3 History; [https://www.r3.com/history/](https://www.r3.com/history/)
8. Corda Technical Whitepaper; [https://www.r3.com/wp-content/uploads/2019/08/corda-technical-whitepaper-August-29-2019.pdf](https://www.r3.com/wp-content/uploads/2019/08/corda-technical-whitepaper-August-29-2019.pdf)
9. Validating Notaries; [https://docs.corda.net/releases/release-M8.2/key-concepts-consensus-notaries.html\#validation](https://docs.corda.net/releases/release-M8.2/key-concepts-consensus-notaries.html#validation)
10. Corda Experimental; [https://docs.corda.net/running-a-notary.html\#byzantine-fault-tolerant-experimental](https://docs.corda.net/running-a-notary.html#byzantine-fault-tolerant-experimental)
11. Corda High Availability; [https://docs.corda.net/design/hadr/design.html](https://docs.corda.net/design/hadr/design.html)
12. Reliable Distributed Systems; [https://blog.pragmaticengineer.com/operating-a-high-scale-distributed-system/](https://blog.pragmaticengineer.com/operating-a-high-scale-distributed-system/)
13. Hyperledger Founded; [https://www.hyperledger.org/announcements/2016/02/09/linux-foundations-hyperledger-project-announces-30-founding-members-and-code-proposals-to-advance-blockchain-technology](https://www.hyperledger.org/announcements/2016/02/09/linux-foundations-hyperledger-project-announces-30-founding-members-and-code-proposals-to-advance-blockchain-technology)
14. Hyperledger Charter; [https://www.hyperledger.org/about/charter](https://www.hyperledger.org/about/charter)
15. What is Fabric; [https://hyperledger-fabric.readthedocs.io/en/latest/whatis.html\#hyperledger-fabric](https://hyperledger-fabric.readthedocs.io/en/latest/whatis.html#hyperledger-fabric)
16. Fabric Whitepaper; [https://arxiv.org/abs/1801.10228v1](https://arxiv.org/abs/1801.10228v1)
17. Fabric Contributions; [https://hyperledger-fabric.readthedocs.io/en/latest/CONTRIBUTING.html](https://hyperledger-fabric.readthedocs.io/en/latest/CONTRIBUTING.html)
18. Fabric 2.0 [https://hyperledger-fabric.readthedocs.io/en/latest/whatsnew.html](https://hyperledger-fabric.readthedocs.io/en/latest/whatsnew.html)
19. Infrastructure as Code; [https://en.wikipedia.org/wiki/Infrastructure\_as\_code](https://en.wikipedia.org/wiki/Infrastructure_as_code)
20. Stellar Assets [https://www.stellar.org/developers/guides/concepts/assets.html](https://www.stellar.org/developers/guides/concepts/assets.html)
21. Stellar Operations [https://www.stellar.org/developers/guides/concepts/list-of-operations.html](https://www.stellar.org/developers/guides/concepts/list-of-operations.html)
22. VeriBlock Whitepaper [https://www.veriblock.org/wp-content/uploads/2018/03/PoP-White-Paper.pdf](https://www.veriblock.org/wp-content/uploads/2018/03/PoP-White-Paper.pdf)



## Provenance Blockchain 2.0 Evolution

The blockchain space has rapidly evolved in the two years since the launch of the Provenance Blockchain 1.0 blockchain. Industry wide there is an increasing focus on proof-of-stake networks and the use of rollups, peg zones, and similar methods for integrating different blockchains. Major public networks centered around Ethereum are seeing increasing interest and load as more financial applications are developed in the Distributed Financial \(DeFi\) space.

Based on the lessons learned running Provenance Blockchain 1.0, an updated Provenance Blockchain version is expected to provide enhancements in the following key areas: 

### Streamline Operations 

Provenance Blockchain 1.0 requires many dedicated components to handle the orderer network and member organization peers \(Nodes\). As a private network, administration of the VPN endpoints and routing is a burden. 

Avoid use of certificates for identity. Within the Provenance Blockchain 1.0 network the use of certificates has been a source of friction for new users and represents a substantial operational overhead for management of renewals. 

Remove barriers of entry for participants to develop and run applications on self managed infrastructure within the Provenance Blockchain network. 

Provide flexible extension options for supporting current and future Provenance Blockchain applications. The existing infrastructure for on chain and off chain transactions must be supportable. 

### Decentralized Digital Identity

Organizations and entities that operate on the blockchain can do so without the authority of a centralized entity or governing body. Identities will follow industry standards, like BIP32, that empower users to directly control their keys.\[6\] 

### Focus on Networking between Blockchains

Future applications in the blockchain space will involve more coordination between blockchain networks as scalability concerns mean that not every transaction can be directly recorded on a global public ledger. Support for interchain standards like IBC.\[7\]

### Capacity for Growth

The network must support fast finality and have sufficient capacity to support the increasing load of transactions as the use of Provenance Blockchain grows. Further the network must provide a flexible set of available tools for scaling to support additional workloads as the initial versions of the network reach their capacity. 

Technical approaches that allow the blockchain to fork are unacceptable as the soft definition of finality introduces complexity and a large amount of latency when high levels of assurance are required. These concerns are extremely acute when performing transactions between one or more blockchains. 

### Encourage Use Case Growth

Provide an ecosystem that can be leveraged, publicly, to develop use cases outside of the Provenance Blockchain 1.0 financial services domain.

### The Provenance Blockchain Balance Point 

Provenance Blockchain 2.0 strikes a careful balance between many different criteria among the varied approaches of blockchain software. Unlike many blockchains that are focused solely on payments, the Provenance Blockchain network is built around digital assets. This distinction shifts the system design firmly towards general purpose designs versus those that are highly optimized for small transactions.

The following criteria are carefully considered when evaluating the adoption of or migration to any current or future blockchain software:

On complex, dedicated infrastructure with explicit configuration requirements versus flexible, ad hoc connectivity:

* Provenance Blockchain prefers a flexible approach. One of the important lessons learned from the first generation of the Provenance Blockchain network is that even though operational aspects of a network can be optimized and streamlined through the use of automation tools, they still have a real non-trivial impact on the overall maintainability and cost to run the network. A complex, explicit networking configuration also tends to be brittle in response to failure when compared to a dynamic peer-to-peer configuration. For the second generation of the network, Provenance Blockchain is focused on lowering these barriers to run the network as this increases adoption and improves the overall cost effectiveness of the system.

On specialized structure versus general purpose approaches:

* Provenance Blockchain heavily favors a general purpose approach. The ability for developers to implement features rapidly in environments they are comfortable with has been an important factor in the current success of the Provenance Blockchain both for migrating business processes to the blockchain as well as refreshing the blockchain as the platform evolves. While specialized approaches including custom domain specific languages \(DSLs\), restrictive APIs or execution environments, exotic languages, and more may be selected for certain specific benefits, each of these techniques have extensive external costs that can negate any perceived benefits.

On methods of integration and extensions of a blockchain node

* Provenance Blockchain prefers flexible platforms that support multiple types of composable extension over those that require all functionality to be implemented through a single interface. A flexible extension model allows appropriate components to be selected to extend the core of the blockchain and extend or replace any other component that does not match the requirements. This same idea extends to governance decisions where not all types of extension and modification have the same level of impact. A flexible platform can adapt and respond to the needs of the application and is not forced to match a more generic general case.

On distributed control and ownership of a network versus centralized models

* Provenance Blockchain favors a sovereign approach where individual networks are left to choose the approaches to governance and interoperability that match their specific needs. The Provenance Blockchain network has always maintained a strong focus on the requirements and concerns specific to the financial uses cases it supports. This focus is shared by stakeholders that participate in the network. The considerations and views of the Provenance Blockchain stakeholders are not expected to perfectly match up with those of other blockchain networks, even those Provenance Blockchain chooses to partner with directly. Provenance Blockchain prefers approaches to federation of blockchains where explicit connections between networks are curated according to the requirements of the partnership and not dictated by the overall ecosystem.

On support for non-custodial identities

* Provenance Blockchain prefers that clients maintain the ability to "bring their own keys" to the blockchain without requiring centralized control or approval. Approaches that rely on certificates issued by a certificate authority or global governance board to control access are incompatible with the intended direction of Provenance Blockchain towards a decentralized public network.

### Ethereum 

The Ethereum blockchain remains the first and most successful programmable blockchain. As a proof-of-work network using a single global ledger, the Ethereum platform struggles to meet the demand of the many DeFi applications. Blocks are often 100% full of transactions leading to surges in gas prices and fees for transaction records. The highly speculative nature of the existing ETH token has resulted in a large swing in the overall price of the token that can severely impact the costs to a business running recording transactions to the blockchain.

The Ethereum Foundation has recognized the significant shortcomings of using a single ledger and a proof-of-work method for consensus, both of which are significant limits to the scalability of the current network. In response Ethereum is developing a 2.0 version that moves to a proof-of-stake solution along with sharding data between zones to facilitate increased transaction volume. The initial version of the Ethereum 2.0 network was successfully staked in December 2020 with the fully operational network planned for 2022\[1\].

Ultimately Provenance Blockchain requires proven solutions to these problems today and cannot afford to wait more than a year for a potential resolution. In addition the design of the public network itself means that governance and control of the underlying network is not intended to support Provenance Blockchain's requirements directly and is by definition targeted at the lowest common denominator for all global uses.

### Solana

The Solana blockchain is a proof-of-stake \[history\] network built around concepts supporting very small and high speed transactions of value between accounts using special on chain programs.\[2\] This relatively narrow definition of what a blockchain can use for communication allows the Solana to handle extremely high transaction volumes with low latency at the cost of general purpose program features and additional developer complexity for adapting to the unique design constraints of the system.

Any distributed system with a high transaction rate will see large variations in volume based on bandwidth utilization. Developers implementing solutions on Solana must contend with 4 kilobyte stack pages, limits on cross program calls, and overall compute budgets that keep on chain programs small and fast to execute\[3\]. These design trade-offs create a network that is not intended for complex processing or larger datasets. As the Provenance Blockchain is focused on providing detailed digital asset provenance, the blockchain must support larger data volumes in order to represent the digital asset history as well as potentially time consuming validation processes to ensure a complete and correct asset.

Highly specialized approaches such as those used by the Solana blockchain do not provide the flexibility that Provenance Blockchain values for rapid delivery of features. If Provenance Blockchain were to adopt the Solana execution module the lessons learned would not be readily transferable to other future blockchain approaches. General interoperability with other blockchains would also be more complex due to the unique execution models employed.

### Polkadot 

The Polkadot network design aims to address the many shortcomings of proof-of-work and single global ledger systems while simultaneously allowing for more general purpose development use cases to be supported than a bare metal focused design such as the one Solana uses. The proof-of-stake concepts for Polkadot were heavily influenced by the Tendermint protocol \(also referenced by Solana and used as the core consensus protocol of Cosmos\).

Polkadot uses a central relay chain and leverages the concept of Parachains\[4\] for the utmost in flexibility in implementation of transaction handling while focusing on a system of centralized validators and enforced governance. The central relay chain validates state transitions for all connected Parachains. If the state of the relay chain requires a rollback for any reason all connected Parachains would also be forced to rollback in unison.

The centralized relay chain model requires that the existing network endorse and explicitly include any new entrants to the network. Due to the processing limits of the relay chain there is currently a limit of 100 Parachains for the Polkadot network. This 100 Parachain limit forces new members to compete with other candidates for the right to join the network by winning an auction for one of the 24 month Parachain connection leases.

The Polkadot software is licensed under the GPL 3.0 which presents significant potential concerns for integration with other existing business processes and development.

For Provenance Blockchain 2.0 the arguments for a mandatory centralized validator network and inheriting the ability to seamlessly interoperate among all the other Parachains, including those that have no useful business applications for the Provenance Blockchain uses cases, is a less compelling than a substantially similar approach that allows elective coordination and integration between chains as agreed to by the stakeholders of each network. The additional costs and fees paid to be a member of the Polkadot network represent a cost for little to no direct benefit.

### Cosmos SDK 

Similar to the Polkadot network’s method for scaling beyond Ethereum, Cosmos is founded on the concept of using many different proof-of-stake blockchains connected together to support diverse and scalable workloads. Unlike Polkadot, each blockchain created with the SDK is completely sovereign and autonomous while maintaining the ability to selectively connect with other chains through the use of IBC \(inter-blockchain communication\).\[7\] This additional flexibility means that a given application is allowed to build up its own dedicated network according to the requirements of its own applications supporting the concepts of using many different blockchain instances in aggregate if the application requires it using as few as a single computer/single node up through a full on public global network.

Cosmos/Tendermint blockchain supports multiple types of integration, both application and smart contract based providing opportunities for high performance or high flexibility depending on the requirements. The Cosmos SDK has been developed in Golang using Protobuf with a straight forward message passing and state machine model that provides extension points that are useful for migrating Provenance Blockchain 1.0 features. 

### Provenance Blockchain 2.0

As Provenance Blockchain moves forward into 2021 the platform itself must continue to change and evolve in response to the growth of the industry. The evolution of the blockchains and their increasing use for financial services have resulted in a significantly expanded set of available technologies compared to those available when Provenance Blockchain 1.0 was launched in 2018.

For Provenance Blockchain 2.0 the platform is evolving to use a more flexible proof-of-stake consensus method based on the Cosmos SDK. This updated platform will allow Provenance Blockchain to maintain its network sovereignty, facilitate interaction with other blockchains, and increase scalability. Ultimately the flexible peer-to-peer networking model used by Tendermint will greatly streamline the operational aspects of the Provenance Blockchain network while creating a more fault tolerant network through the use of redundant network paths in the peer-to-peer architecture.

The generic ABCI interface of the Tendermint network and the pluggable nature of the Cosmos SDK allows Provenance Blockchain 2.0 to be built as a streamlined standard application written in Go for increased developer productivity. The inclusion of WASM \(web assembler\) environment allows smart contracts to be created for the ultimate deployment flexibility at the expense of performance and ease of development.

With these improvements the second generation of the Provenance Blockchain network will be faster, easier and more cost effective to maintain and deploy, and more accessible for third parties to integrate with while maintaining the sovereignty and control that has allowed stakeholders to direct the delivery of cutting edge financial blockchain solutions.

1. Ethereum 2.0 Timeline; [https://ethereum.org/en/eth2/](https://ethereum.org/en/eth2/) 
2. Solana Whitepaper; [https://solana.com/solana-whitepaper.pdf](https://solana.com/solana-whitepaper.pdf) 
3. Solana Compute Budget; [https://docs.solana.com/developing/programming-model/runtime\#compute-budget](https://docs.solana.com/developing/programming-model/runtime#compute-budget) 
4. Polkadot Whitepaper; [https://polkadot.network/PolkaDotPaper.pdf](https://polkadot.network/PolkaDotPaper.pdf) 
5. Cosmos Introduction; [https://cosmos.network/intro](https://cosmos.network/intro) 
6. Hierarchical Deterministic Wallets; [https://en.bitcoin.it/wiki/BIP\_0032](https://en.bitcoin.it/wiki/BIP_0032) 
7. Interchain Standards and IBC; [https://github.com/cosmos/ics](https://github.com/cosmos/ics)

