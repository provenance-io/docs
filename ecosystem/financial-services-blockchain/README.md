---
description: >-
  Provenance provides the foundation to build marketplaces and exchanges for
  buyers and sellers of digital assets.
---

# Provenance: Financial Services Blockchain

### Disintermediation

Many intermediaries argue that their services are necessary for the market to function. Who else will safeguard investors if not a trusted third party?  But such an argument is a false dichotomy as it presumes trust is necessary.

Provenance reduces third-party intermediation and internal staffing costs, promotes greater transparency and liquidity, and allows for new kinds of financial engineering and business opportunities. Any firm using Provenance benefits from the blockchain and only pays [Hash gas fee payments](../../contributing/adr/300-core-concepts/301-hash.md) back to the ecosystem.

### Distributed Trust

Provenance has three key attributes: **Distributed Information, Immutable History,** and **Trustless Data**.  Distributed content provides defense against hacking, eliminates agency problems that can lead to malfeasance, and helps support enforceable digital contracts. Data written on the ledger cannot be changed. Immutable information is critical in establishing asset provenance and the truth of the data. Having to rep and warranty data—analogous to “trust me”—undermines the system and introduces risk to those who provide these attestations.

These attributes are necessary, but not sufficient, to eliminate rent-seeking and bring incremental value to financial markets. To transform financial services, Provenance performs three key functions: ledger, registry and exchange.

* **Ledger –** Movements of value are done on blockchain, delivering true economies of scale.  Immutable records reduce or eliminate the need for trustees and reconciliation and provide real time visibility to information.
* **Registry -** Ownership is determined solely on blockchain, allowing T+0 pledging and sales of assets. Chain of custody is immutable, and recording and conveyance are the same thing, eliminating latency/discrepancies.
* **Exchange -** Price discovery becomes transparent, eliminating the need for market making intermediaries. Assets can be tokenized, allowing fractionalized sales and creating liquidity in illiquid asset classes.

### Enabling Marketplaces

Numerous financial marketplaces leverage Provenance to connect buyers and sellers of digital assets.  Marketplaces are are analogous to a town full of stores. Each store has products for primary and secondary offerings. Some stores require accreditation to enter and transact. For example, Figure’s Passport solution leverages the Provenance [Attribute](../../modules/account.md) and [Name](../../modules/name-module.md) modules for accreditation. Marketplaces build on Provenance Blockchain's support for loans, loan participation and private funds. Soon these marketplaces will support a full range of securities, from asset-backed securities to private equity.

Marketplaces leveraging Provenance allow two counter-parties to trade bilaterally, settling real-time without counter-party or settlement risk using the Provenance [Account](../../modules/inherited-modules.md), [Marker](../../modules/marker-module.md), [Metadata](../../modules/metadata-module.md), and [Bank](../../modules/inherited-modules.md) Modules. Complex ownership, exchange, and settlement functions can be expressed using [Provenance Smart Contracts](../../modules/provwasm-smart-contracts.md).  Together, these Provenance features allow unprecedented access, ease, and transparency to asset and fund interest buyers and sellers.  

### Digital Fund Services

Provenance is revolutionizing how private funds are digitally issued, accessed and exchanged.

[Figure's Digital Fund Services](https://provenance.io/#digital-fund-services) on Provenance utilize tools for digital fundraising and ongoing fund management with a primary marketplace for raising capital and a secondary marketplace for trading fund interests.  Using [Smart Contracts](../../modules/provwasm-smart-contracts.md) in concert with the Provenance [Account](../../modules/inherited-modules.md), [Marker](../../modules/marker-module.md), and [Bank](../../modules/inherited-modules.md) modules improves liquidity profile of illiquid funds through bilateral secondary trading of fund interests with real time settlement while eliminating high search and transaction costs of current secondary markets.

## Value Proposition

The actions outlined herein create a perfectly immutable record of loan provenance on a distributed system that acts as a ledger, registry and ultimately, exchange. The warehouse, securitization and liquidity benefits alone represent up to $90 billion or more in reduced fees, technology and personnel costs and improved transparency and liquidity in the $3 trillion annual securitization space. These benefits primarily encompass: 

* **Lower origination and aggregation costs** - Trustless and immutable origination results in the reduction of QC, compliance and upfront audit costs. Ability to pledge real time with complete chain of custody reduces custody, payee agent and administrative costs for the warehouse provider while providing significant capital efficiency to the originator.  Embedded servicing eliminates standalone servicing platform and personnel costs.  Estimated benefit of 50-125 bps annually \($15 to $37.5 billion\).
* **Lower deal costs** - Smart contracts and immutable data provide certainty to assets, reducing audit scope from the loan level to the smart contract level. Blockchain acts as custodian and payee agent, and legal documents can come from a smart contract tied to the collateral and resulting loans, saving significant fees. Deals can be done instantaneously, rather than in weeks. Estimated benefit of 20-20 bps annually \($6 to $9 billion\).
* **Improved and more efficient ratings** -  Data certainty reduces deal risk and scope of ratings work.  Knowledge of bondholders eliminates quorum risk for deal votes \(changing servicers, modifying collateral, etc.\)  Blockchain derisks new originators, servicers and lifts rating caps.  Estimated benefit TBD.
* **Improved liquidity** - Real time transparency of underlying collateral eliminates reliance on monthly remittance reports, allowing for continuous pricing and eliminating information asymmetry.  Estimated benefit 100-150 bps annually \($30 to $45 billion\).

