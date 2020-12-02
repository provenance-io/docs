# Definitions

_**Asset:**_ Any data representing a digital asset that has its origin and history maintained by Provenance.

_**Affiliates:**_ Affiliates are the entities that are allowed to execute contracts and submit transactions to Provenance. An Affiliate is permissioned on the network and issued an enrollment certificate used for authentication, authorization, and signing.

_**Certificate Authority \(CA\):**_ A certificate authority \(CA\) is a trusted entity that issues electronic documents that verify a digital entity\'s identity on the Internet. The electronic documents, which are called digital certificates, are an essential part of secure communication and play an important part in the public key infrastructure \(PKI\).

_**Chaincode:**_ Programs that run on top of the blockchain to implement the business logic of how applications interact with the ledger.

_**Client Contract**_**:** Client contracts provide a secure and intelligent piece of code that facilitates multiple party consensus on a given transaction. Contracts are executed by each affiliate party to a transaction. The hashed execution results of the contracts are sent to the Nodes to verify that data being processed is consistent with the current world state of the Provenance Distributed Ledger, and that the parties involved in the transaction are authorized to perform business operations upon the target Asset.

_**Consensus:**_ By default, a majority of Nodes must agree and endorse transactions before new blocks are committed on the blockchain. Consensus is the process of achieving this agreement. Consensus is controlled by an Endorsement Policy. Endorsement Policies, administered by Provenance, can be set up to utilize alternate consensus models such as: majority consensus, _X_ out of _N_ Nodes, specific Member Node\(s\), or combinations of Nodes.

_**CouchDB:**_ The state store used by the Provenance blockchain. Each Node will host a CouchDB per Node container. Data in the CouchDB is protected by end-to-end encryption.

_**DIME:**_ \(Data Instance with Metadata and Encryption\) An object container that holds encrypted information exchanged on the Provenance Protocol.

_**Distributed:**_ Identical information is replicated and synchronized between multiple Nodes.

_**Distributed Ledger Technology \(DLT\):**_ A distributed ledger \(also called a shared ledger\) is a consensus of replicated, shared, and synchronized digital data geographically spread across multiple sites, countries, or institutions. There is no central administrator or centralized data storage.

_**Elliptic Curve Integrated Encryption Scheme \(ECIES\):**_ hybrid encryption system proposed by Victor Shoup in 2001. Provenance uses ECIES for end-to-end encryption in conjunction with the DIME when storing information in the Encrypted Object Store.

_**Encrypted Object Store \(EOS\):**_ An encrypted, local data repository where affiliates store and permission their data. The data store is included with the SDK.

_**Endorsement Policy:**_ Instructs a Node on how to decide whether a blockchain transaction is properly endorsed. Nodes use endorsement policies to determine if: all endorsements are valid \(i.e. they are valid signatures from valid certificates over the expected message\), there is an appropriate number of endorsements \(i.e. a majority of Nodes\), and endorsements come from the expected sources \(i.e. a group of Nodes or a specific Node has endorsed\).

_**Hash:**_ A digital immutable token which has a fiat \(monetary\) value associated with it. \(Note: the Hash token is not a mathematical checksum in the context of Provenance.\)

_**Immutable:**_ Information integrity is securely maintained by deriving a checksum of new blocks containing encrypted data and a timestamp, that are combined with the previous block's checksum. Thus, once written, blocks cannot be altered.

_**Intermediate Certificate Authority:**_ To communicate with other organizations in the Provenance network, each organization must manage its Member enrollments via an intermediate certificate created and signed by the root Provenance Certificate Authority. The Intermediate Certificate Authority \(ICA\) creates Member certificates appropriate for use on the Provenance Network. This chain of trust allows the root Provenance Certificate Authority to revoke organization access.

_**Key Management Server:**_ The Key Management Server \(KMS\) component of Provenance provides an authoritative centralized location for public key retrieval. Public keys for members and platform infrastructure \(e.g. Node containers\) are required for creating DIME payloads used to interact with the system. As an authoritative source of public keys the KMS can control which Nodes are selected for participation in Endorsement Policies.

_**Member**_**:** An organizational entity such as an administrator, user, node, affiliate, or process. Members are entities or individuals that are permissioned to interact with Provenance.

_**Node:**_ An organization on the network which endorses and commits all transactions proposed by various DLT applications. The Node only gets a copy of the data present in the ledger they are hosting. This data includes checksums from the results of the executed contracts, scope UUID, Affiliates involved in the transaction, etc. Asset data is only stored by the Affiliates in the Encrypted Object Store and is not stored by the Node. A Node deployment will consist of one or more containers, a state store CouchDB per container, and an Intermediate Certificate Authority.

_**Node Container:**_ A container process that manages the underlying blockchain ledger and executes chaincode. A Node may have one or more containers in its environment. Having more than one container provides data redundancy and consistency.

_**Owner Controlled Assets:**_ Updates to or transfers of assets on the chain should be controlled by the owner.

_**Principle of Least Privilege \(PoLP\):**_ every process or individual must be able to access only the information and resources that are necessary for its legitimate purpose.

_**Public Key Infrastructure \(PKI\):**_ the policies, procedures, and roles to manage Provenance Member digital certificates and related public-key encryption processes. PKI is used to create, distribute, manage, utilize, authorize, authenticate, and revoke Member digital certificates.

_**Provenance Orderer:**_ A Provenance-hosted service that provides an atomic delivery of all transactions with total-order delivery and reliability. The Provenance Orderer delivers the same transactions to all connected Nodes in the same logical order before they are committed to the Node's local blockchain state store. This atomic communication guarantee is also called total-order broadcast, atomic broadcast, or consensus in the context of distributed systems. The communicated transactions are candidates for inclusion in the blockchain state.

_**Provenance Protocol \(the Protocol\):**_ Provenance\'s defined methodology for providing transparency to all assets and their transactions.

_**Software Development Kit \(SDK\):**_ Provenance Software Development Kit available to affiliates for interaction with the Provenance system.

