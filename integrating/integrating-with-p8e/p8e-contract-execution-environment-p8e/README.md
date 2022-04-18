---
description: Practical implementations of p8e
---

# P8e Contract Execution Environment (p8e)

While the components and key concepts of p8e and p8e contracts are covered in the [p8e Overview](https://docs.provenance.io/p8e/overview), this guide will focus on how to deploy your own p8e environment and put it to use.

By the end of this guide, you will feel comfortable spinning up and configuring your own development environment, writing and publishing your own p8e contracts, and executing them. Whether it's to build a new use case or integrate with an existing one, you will gain an understanding of where your data lives and who has access to it.

## Participation Models

There is more than one way to participate in the Provenance Blockchain ecosystem. Some participants will want to simply send data to a technology service provider (such as Figure Tech) and allow them to orchestrate the process of pushing data to the Encrypted Object Store and Provenance Blockchain ledger. In this model, the technology service provider likely also hosts and operates the P8e Contract Execution Environment and/or [Provenance Blockchain Nodes](https://docs.provenance.io/blockchain/introduction/major-components#provenance-blockchain-node) used to store loan data and record transactions, respectively.

Other participants will want full control over their data and will choose to operate their own interfaces, middleware (including Object Stores), or Provenance Blockchain Nodes. Ultimately, each participant can decide which components they are comfortable operating. The more control over the running applications and data stores, the more flexibility the participant has to build out new use cases with business partners within the ecosystem, extending the loan data model as needed.

To support these choices, Figure Tech has both deployed a white-label asset onboarding service, where orchestration including key management is completely handled on behalf of loan originators, and published an open-source API that implements the open-source Provenance Software Development Kit (SDK) and can be extended by any asset originator.

## To Host or Not to Host

Here lies the first major decision any asset originator needs to make: to deploy and host your own p8e Contract Execution Environment or not. That's sounds binary, but there are actually more than two choices:

1. Integrators can operate each component on their own.
2. Integrators can operate their own API to orchestrate the execution of p8e contracts, while pointing to an object store hosted by a business partner or technology service provider.
3. Integrators can simply send data to a partner that operates the components.

This guide will walk you through everything you need to know to go with option #1. That said, Figure Tech is available as a partner to integrators that want to go with option #2 or #3.

{% hint style="info" %}
If you are solely looking for information related to the white-label approach, feel free to skip down to the [Sandbox Environment](./#sandbox-environment) section.
{% endhint %}

Remember to consider data isolation when making the decision of what to host or who to work with as a technology servicer provider. Ensure that you understand whether or not your data is stored in a shared object store or isolated in its own environment.
