# Blockchain Comparison

## PROVENANCE BLOCKCHAIN

Provenance is the leading production blockchain protocol for the financial services industry. It is a permissioned, proof of stake protocol that acts as a global ledger, registry and exchange across assets and markets.

Provenance Platform builds on a strong core of open source including a diverse set of messaging, networking, systems orchestration and blockchain technology. The initial process of developing the platform included an in depth evaluation of many different open source projects for potential use in the core of Provenance.

Software projects were evaluated against several different criteria including:

* Health and vibrancy of the open source community surrounding the project
* Source code quality and software engineering principles
* Flexibility of the software for handling current and future business needs
* Secure, scalable infrastructure capable of providing permissioning, privacy, and integrity for a diverse set of stakeholder transactions.

While the industry has made advancements in the last 18 months since our choices were made, the core differences are still present.

## QUORUM

The Quorum blockchain project is a fork of the open source Ethereum blockchain. The Quorum platform retains close alignment with its public upstream parent in order to leverage the large Ethereum community.\[1\] The decision to base the project off of a large and well tested public blockchain is intended to inherit the careful security testing and high code quality of the parent project. As a high profile public blockchain Ethereum is subject to all of the idiosyncrasies of various user groups, high transaction volumes, and potentially malicious actors.

With such a powerful open source community at its heart Quorum would seem to be a strong choice for a private financial platform. Unfortunately this strong core can result in conflicting design choices if the goals between the two are not well aligned. An evaluation of the technical underpinnings reveals potential concerns to this effect.

### Private Transactions

A global financial ledger must accommodate separate contexts for different user groups that ensure privacy and security of data. As a blockchain the permanent immutable record further places constraints on how data is stored and protected long term. As an additional complexity consumers may demand certain personally identifiable information is purgeable regardless of the immutable nature of a global ledger.

These privacy requirements for transactions are a unique constraint that Quorum must satisfy above and beyond what the Ethereum project provides. To satisfy these requirements Quorum has built infrastructure to support private encrypted data stored off chain. As the Ethereum core does not include an interface to integrate these features, Quorum was forced to inject their feature through a special signature id value\[2\]. Further, in the interest of keeping these two projects in close alignment the private transaction feature does not make any changes to the basic structure of a transaction forsaking the opportunity to relax the data storage constraints.

### Data Storage and Memory

The Ethereum virtual machine concept was designed to allow participants in the Ethereum public network to run smart contracts containing arbitrary code from other users in a safe way on their systems. There are many restrictions in place to prevent malicious actors from performing denial of service attacks or otherwise abusing the system. These constraints require software developers to understand the limits and how to work within them \[4\].

The nature of Ethereum and its control over the Quorum project has resulted in bug fix and change requests to make improvements or at least allow process limits to be configurable; requests languish for years without resolution \[5\]. The overall transaction size limit for Quorum was doubled from 32KB in Ethereum to 64KB. \[6\] This limit is still quite low for operations such as a loan sale where thousands of loans may be aggregated in a single transaction. It is also worth noting that any change to the core Ethereum code base that must be done in the Quorum fork invalidates many of the unit tests and security checks that are part of the core benefit to building on the Ethereum blockchain base.

### Review

The Quorum blockchain project is an interesting approach to solving the needs of a distributed financial ledger. The effort involved in creating a full private transaction system for smart contracts is commendable and well constructed. Unfortunately thisproject is hampered by its roots on the public Ethereum project which drives requirements towards the public blockchain and has few resources in design or maintenance towards supporting the private network needs and requirements.

## CORDA / R3

If Quorum is an example of leveraging a powerful open-source blockchain and bending it towards the needs of a private permissioned network, Corda and the R3 consortium makes a good example of the opposite approach. Starting with a committee of industry members, Corda has built a custom solution from the ground up. \[7\] To further distinguish Corda from other projects in the space, it is not actually a blockchain at all. The premise of Corda is a system for independent ledgers maintained by each entity to transact together on an as needed basis. \[8, p7\]

### Determinism

As Corda is not a distributed blockchain it cannot rely upon multiple parties to perform parallel executions of smart contracts to ensure consistency. Corda attempts to ensure determinism through a special limited JVM. \[8, p44\] This approach strikes a balance between flexible implementation and constraints to ensure reliable deterministic smart contract execution. In contrast Hyperledger Fabric relies on Kernel level process isolation in the form of containersto provide a much stronger guarantee of separation than the JVM. The container isolation combined with distributed parallel execution achieves determinism guarantees without resorting to limiting development to programs that run on a special limited JVM.

The Corda consensus model is implemented through Notaries with two different levels of transaction safety. At the lowest level a Notary simply determines that the input states for a transaction have not been previously used \(aka double spend in a UTXO\). A higher level of safety can be achieved with a ‘validating notary’ with the forfeit of transaction privacy. \[9\]

### System Architecture

With the Corda architecture starting from a single entity and progressing towards collaboration it is not surprising thatdesign constraints associated with multiple parties and scalable cloud infrastructure have a lower priority. For components of the system that perform scalable conflict resolution \(within the notaries\) the distributed consensus algorithms remain experimental and not suitable for production use. \[10\] The implementation of the Corda node itself lacks fault tolerance and high availability features \[11\] which have remained on the long term planning roadmap as future items.

When building a distributed application it is important to adopt many patterns and practices from the start to ensure a successful overall system. \[12\] Currently Corda lacks a standard cloud native deployment configuration. A cloud native application architecture must account for unreliable instances that can come or go at any time leading to a stronger focus on consistency and reliability engineering. If Corda deployments targeted a standard cloud native platform such as Kubernetes then testing against the constraints of a system running in the cloud would take a higher priority. Presently these issues \[11\] must be resolved entirely by the implementer of the system until such time as the Corda project matures and implements these pieces of their roadmap.

### Review

Corda represents a unique approach to solving the ledger problem by building many distributed ledgers and working towards an ad hoc aggregation to create an emergent global blockchain. Cloud native infrastructure constraints and the creation of a larger multi-stakeholder network remain future items or are not considered priorities at all favoring a robust single client node approach.

## HYPERLEDGER FABRIC

The Hyperledger group was founded in 2016 under the Linux Foundation with a number of industry partners. \[13\] \[14\] As part ofHyperledger, Fabric is developed to serve as a flexible enterprise blockchain platform for many different uses including supply chain and financial systems. As part of supporting diverse use cases, Fabric is focused on a flexible and configurable platform for building permissioned distributed ledgers. Hyperledger Fabric uses a unique approach of ‘execute, order, validate’ to validate transactions which allows for multiple parties to execute a contract to ensure determinism and to increase performance by distributing execution.

### Flexible Architecture

One of the most important features for developing the Provenance platform is the ability to rapidly build diverse solutions in response to business opportunities. The platform must be adaptable through configuration and custom extensions in a maintainable fashion. \[16, p3\] An additional consideration to the flexible architecture are the requirements for developing applications on the blockchain using smart contracts. Fabric allows many different languages to be used with major SDKs for Go, Java, and NodeJS as official offerings as well as a well defined communication layer supporting the community development of additional languages.

### Roadmap

The Fabric project has a well defined set of community guidelines for steering the project including a defined release cadence and the concept of long term releases for production stability. \[17\] The Fabric project has been making significant progress on expanding both new reference applications such as FabToken as well streamlining administration of smart contracts and system versioning. \[18\] The combination of new features as well as a dedicated approach to refining administration tasks required for large scale deployments indicate a stable direction for enterprises to adopt.

### Review

The Hyperledger Fabric platform provides a modular system with a diverse set of applications supported as primary use cases. The unique execute-order model streamlines the scaling of smart contract execution while resolving issues of determinism without requiring users to redevelop their business logic in a limited environment or translation into a special purpose language. The project roadmap shows near-term and long-term features are centered around stability and streamlining the management of large scale enterprise networks.

## PROVENANCE PLATFORM

The Provenance Platform is more than just another blockchain; it is an entire system architecture to support financial industry transactions. Provenance has built up infrastructure to monitor, manage, and maintain a large scale distributed platform across several major cloud providers. Expert integration of many different open source products along with the expertise to directly extend these capabilities beyond what is directly offered has resulted in a uniquely capable and stable blockchain platform.

### Extensions

A global distributed system for financial transactions on blockchain requires more than just installing software and turning it on. A comprehensive system design must account for many different factors including; monitoring, anomaly detection, incident response, software deployment, failover testing, security audits, and capacity planning constraints among many others. Engineering solutions for these constraints requires specific targeted extensions for any basic blockchain software installation.

The first step to leveraging the Hyperledger Fabric project was the creation of an entirely new cloud native smart contract execution management system allowing for secure deployment inside Kubernetes clusters. This was combined with strong encryption controls based on client managed keys for control of data access and long term secure storage on chain.

* Kubernetes Cloud Platform – The default scheduling components within Hyperledger Fabric are not well integrated with cloud native platforms such as Kubernetes. While future support is on the long term roadmap, Provenance has leveraged the modular Fabric architecture to integrate a more secure Kubernetes scheduler that streamlines software release workflows and allows distribution of workloads across nodes in the cluster.
* Secure private networking for public clouds–The distribution of the Provenance network between stakeholders in many different public clouds would traditionally require exposing services over the internet along with a complex series of networking rules and configuration. Through the use of a novel Wireguard networking setup Provenance member nodes can communicate securely as one large blockchain network internally while exposing business focused applications externally for a streamlined architecture and minimized public attack surface.
* Client to client private transactions on the global ledger–Building on the Execute-Order model of Hyperledger Fabric, Provenance has developed a unique approach to private transaction execution allowing specially instrumented transactions to execute in parallel between private parties securely for deterministic behavior before the results are forwarded to the global ledger. This approach provides benefits for privacy, data control and increased performance while maintaining the consistency controls of a global ledger.

### Production Environment

The Provenance Blockchain platform has been developed with a strong focus on reliability and availability. Continuous improvement in software and the rapid release of new capabilities are made possible through the use of best practices such as Infrastructure as Code. \[19\] Provenance has developed repeatable processes for all of the management activities associated with the upgrade and rollout of enterprise blockchain components. The Provenance environment includes redundant systems for monitoring and responding to infrastructure issues from the routine –managing disk space, to advanced tasks such as deploying a smart contract hotfix quickly across all network nodes.

## STELLAR

While the Provenance Platform, Ethereum/Quorum, and Corda/R3 are all focused on building a general-purposeplatform for smart contract execution there are other blockchains that focus on only part of the ecosystem. Stellar is one example of a single purpose blockchain focused on value exchange using a custom token known as Lumen \(XLM\).

### Assets and Tokens

Within the Stellar network the concept of an asset \[20\] is represented by tokens issued from an account and is limited to numerations of value held by parties associated with these accounts. Stellar proclaims that any type of asset can be tracked or transferred on the platform however this capability is limited to issuing and exchanging value between Stellar accounts.

### Smart Contracts

Stellar supports a limited form of smart contract that allows value to be programmatically exchanged between multiple parties. These Stellar smart contracts are not intended to be a Turing complete language for codifying business processes. A Stellar smart contract is based on 13 different operation types that the blockchain supports. \[21\] These limitations on smart contracts prevent the extension of the Stellar platform to support the various custom business logic processes that Provenance leverages to implement products on blockchain.

## Inherited Blockchain

Another popular way of extending limited ‘token focused’ blockchains is through the use of metadata or memo fields to contain references to an external chain and thus inherit the security benefits of the parent chain. The Bitcoin network has a special transaction field called OP\_RETURN which has been leveraged to build highly secure Proof of ‘Proof of Work’ chain \[22\]. Unfortunately,the Stellar blockchain has a limit of 32 bytes of metadata on a given transaction whichrenders the approaches used for Bitcoin impossible.

### Review

The Stellar blockchain provides a useful method of exchanging currencies and provides methods to extend the concept of a currency to an asset. The Provenance Platform provides a similar set of features for fiat exchange through the use of an Omnibus bank system.

Unfortunately due to limits in the design of Stellar the capabilities beyond an Omnibus required for the Provenance platform are not possible. The Provenance platform can not be built on a public network focused on token exchange as these features represent only a small part of the Provenance ecosystem.

## CONCLUSIONS

Provenance includes a core built on strong open source technologies combined with engineering expertise to extends these capabilities far beyond the sum of their parts. The unique extensions and combination of tools yield a best of breed financial services blockchain platform. Individually the major blockchain projects mentioned in this paper have a limited capability for maintaining themselves in isolation or in some cases for small network deployments.

Public blockchains or projects heavily derived from these such as Quorum are constrained by a conflicting set of goals between the primary public uses and the derivative private blockchain. Alternative projects similar to Corda that build up a solution focused on the individual users leave hard problems of global consensus, networking, and cloud reliability engineering for future resolution or users to solve themselves. Incontrast the Provenance platform builds on the flexibility of the Hyperledger Fabric platform with carefully engineered extensions to offer a global platform for financial blockchain services. Provenance continues to invest in the platform to ensure long term reliability and capability for current and future financial applications.

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

