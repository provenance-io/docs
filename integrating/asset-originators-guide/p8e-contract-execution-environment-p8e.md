---
description: Practical implementations of p8e
---

# P8e Contract Execution Environment (p8e)

While the components and key concepts of p8e and p8e contracts are covered in the [p8e Overview](https://docs.provenance.io/p8e/overview), this guide will focus on how to deploy your own p8e environment and put it to use.

## To Host or Not to Host

Here lies the first major decision any asset originator needs to make: to deploy and host your own p8e Contract Execution Environment or not. That's sounds binary, but there are actually three choices:

1. Host your own
2. Find a tech services provider, such as [Figure Tech](https://www.figure.tech), to run one for you
3. Find a partner you trust that runs their own and is willing to put your data alongside theirs

This guide will focus on option #1, while the Figure white-label approach mentioned in the Participation Models section of the overview could implement either option #2 or #3.

Both #1 and #2 provide data isolation, while all three enable permissioned data replication with business partners.

## Deploy and Configure the Encrypted Object Store

### Local Deployment

The encrypted object store is easily deployed locally in a Docker container. However, to enable testing of p8e contracts locally, you will also need to deploy a Provenance node. A simple, single node Provenance network is sufficient for local development and is well-suited for testing new contracts and integrations.

A Docker Compose environment that deploys the object store as a container alongside Postgres for data persistence and a single node instance of Provenance is available in the [p8e-scope-sdk GitHub repository](https://github.com/provenance-io/p8e-scope-sdk/tree/main/dev-tools/compose).

The startup call provides an option to specify a multi-party environment that spins up two separate Encrypted Object Stores for testing replication and multi-party p8e contracts.

No additional configuration is needed in this environment.

### Test Deployment

\[TODO: special configuration for testnet, networking considerations]

### Production Deployment

\[TODO: special configuration for mainnet, networking considerations]

## Sandbox Environment

{% hint style="info" %}
Figure hosts and operates a sandbox p8e environment that is configured to work with the Provenance testnet for potential asset originators to play with. Contact Figure for more information.
{% endhint %}

## What About the rest of the Contract Execution Environment?

So far, we've only implemented the Encrypted Object Store. The other half of the p8e Contract Execution Environment is not an individual component to deploy and configure, but an SDK that gets implemented by an application.

Once you have the Encrypted Object Store deployed and configured, move to the next page to learn about how the p8e-scode-sdk is implemented in the sample Loan Onboarding Service.
