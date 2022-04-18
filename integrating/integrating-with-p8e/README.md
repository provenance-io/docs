---
description: A practical guide for integrating with p8e
---

# Integrating with p8e

This guide walks through some key concepts and practical steps to begin your journey in originating and managing assets on Provenance Blockchain. You will learn how to use the [P8e Contract Execution Environment (p8e)](https://docs.provenance.io/p8e/overview) to manage private, sensitive data while maintaining a public record of changes, comprised of hashes of the data stored in p8e.

By following along, you will learn more about:

* data modeling in a decentralized world,
* operating an [Encrypted Object Store](https://github.com/provenance-io/object-store) with the option to enable replication,
* writing and publishing p8e smart contracts,
* key management and permissioning others, and
* using the [p8e Contract Execution Environment API](https://github.com/provenance-io/p8e-cee-api) to transact with the Provenance Blockchain network.

For sake of brevity, this guide will not describe how to deploy and configure a Provenance Blockchain Node. It will, however, guide users in how to spin up a local network for testing, and how to configure which node is used when submitting the transaction proposal message to the network.

This guide will touch on the differences between connecting to a local network for development, the Provenance Blockchain public [testnet](https://github.com/provenance-io/testnet), and the public production Provenance Blockchain [mainnet](https://github.com/provenance-io/mainnet).

{% hint style="info" %}
Figure Tech can provide Provenance Blockchain Nodes as a service for organizations that want to deploy, manage, and monitor their own nodes. Check out the [Figure Tech](https://www.figure.tech) website for more information.
{% endhint %}

The core concepts listed above apply no matter what type of asset you want to record and manage. That said, as we step through the guide we will follow a very real example of how Figure Technologies is leveraging the Provenance Blockchain network to build products for the lending ecosystem.

Loan originators, third-party validators, loan servicers, and investors can follow along to learn more about how the system works, or more specifically, how to integrate their own systems with the network.

Not interested in lending? The guide will provide a wealth of practical knowledge for building out any asset class on Provenance Blockchain. As you will see, it's easy to apply the lessons learned while building out the lending ecosystem to other use cases.

But first, let's dive into lending as a perfect example of how Provenance Blockchain can bring massive value to wide variety of players in the space.

{% hint style="info" %}
Loan originators looking to take the white-label approach to onboard loans can skip ahead to the [API Usage Guide](loan-onboarding-service/api-usage-guide/), which when combined with the [API Specification](loan-onboarding-service/api-specification/) and [Data Mapping](lending-ecosystem/data-mapping.md) pages will tell you all you need to know to get started.
{% endhint %}
