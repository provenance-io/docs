---
description: Practical implementations of p8e
---

# P8e Contract Execution Environment (p8e)

While the components and key concepts of p8e and p8e contracts are covered in the [p8e Overview](https://docs.provenance.io/p8e/overview), this guide will focus on how to deploy your own p8e environment and put it to use.

## To Host or Not to Host

Here lies the first major decision any asset originator needs to make: to deploy and host your own p8e Contract Execution Environment or not. That's sounds binary, but there are actually three choices:

1. Host your own,
2. Find a tech services provider, such as [Figure Tech](https://www.figure.tech), to operate one for you as-a-service,
3. Find a partner you trust that runs their own and is willing to put your data alongside theirs.

This guide will focus on option #1, while the Figure white-label approach mentioned in the Participation Models section of the overview could implement either option #2 or #3.

{% hint style="info" %}
If you are solely looking for information related to the white-label approach, feel free to skip down to the [Sandbox Environment](p8e-contract-execution-environment-p8e.md#sandbox-environment) section, then on to the [next section](loan-onboarding-service/).
{% endhint %}

Both #1 and #2 provide data isolation, while all three enable permissioned data replication with business partners.

## Deploy and Configure the Encrypted Object Store

### Local Deployment

The encrypted object store is easily deployed locally in a Docker container. However, to enable testing of p8e contracts locally, you will also need to deploy a Provenance node. A simple, single node Provenance network is sufficient for local development and is well-suited for testing new business logic for contracts and testing integrations with p8e.

#### Single Party Tests

A Docker Compose environment that deploys the object store as a container alongside Postgres for data persistence and a single node instance of Provenance is available in the [p8e-scope-sdk GitHub repository](https://github.com/provenance-io/p8e-scope-sdk/tree/main/dev-tools/compose). Clone that repository and run the Docker Compose startup command from the `dev-tools/compose` directory.

```
git clone https://github.com/provenance-io/p8e-scope-sdk.git
cd p8e-scope-sdk
cd dev-tools/compose
docker-compose up -d
```

You can spin up that docker environment and run the loan onboarding service described in the next section separately, or extend the Docker Compose environment to include it. Running the loan onboarding service in debug mode alongside the p8e environment would end up looking like this:

![Single Object Store Local Testing Environment](<../../.gitbook/assets/Post Close - Local Env.png>)

#### Multi-Party Tests

A variation of the Docker Compose startup command provides an option to specify a multi-party environment that spins up two separate Encrypted Object Stores for testing replication and multi-party p8e contracts. In the setup provided, both Encrypted Object Stores create a separate database within the same instance of Postgres.

```
git clone https://github.com/provenance-io/p8e-scope-sdk.git
cd p8e-scope-sdk
cd dev-tools/compose
docker compose --profile multi-party up -d
```

Depending on your testing needs, you may want to use the same service to transact in both Object Stores to simulate multiple parties. Alternatively, you may want to build separate applications that perform different functions within the multi-party contract. As a practical example, a Loan Originator may want to build one service dedicated to validating and onboarding loan data from several upstream product-specific services, and another service dedicated to managing updates to that asset throughout the loan life cycle. Other reasons to test these services separately would be to simulate partnerships where each party owns their own Object Store and p8e-enabled applications.

{% tabs %}
{% tab title="Single Service" %}
![A single service transacting with unique object stores to execute either multi-party contracts or a series of contracts](<../../.gitbook/assets/Post Close - Local Env - MP1.png>)
{% endtab %}

{% tab title="Multiple Services" %}
![Multiple services transacting with unique object stores to execute either multi-party contracts or a series of contracts](<../../.gitbook/assets/Post Close - Local Env - MP2.png>)
{% endtab %}
{% endtabs %}

#### Configuration

By default, the components are listening on the following ports:

| Component                  | Container Name  | Port(s)                                      |
| -------------------------- | --------------- | -------------------------------------------- |
| Provenance Blockchain Node | provenance      | <p>1317:1317<br>9090:9090<br>26657:26657</p> |
| Postgres                   | postgres        | 5432:5432                                    |
| Object Store 1             | object-store    | 5000:8080                                    |
| Object Store 2             | object-store-mp | 5001:8080                                    |

### Test Deployment

Coming soon!

### Production Deployment

Coming soon!

## Sandbox Environment

{% hint style="info" %}
Figure hosts and operates a sandbox p8e environment that is configured to work with the Provenance testnet for potential asset originators to play with. Contact Figure for more information.
{% endhint %}

## What About the rest of the Contract Execution Environment?

So far, we've only implemented the Encrypted Object Store. The other half of the p8e Contract Execution Environment is not an individual component to deploy and configure, but an SDK that gets implemented by an application.

Once you have the Encrypted Object Store deployed and configured, move to the next page to learn about how the p8e-scode-sdk is implemented in the sample Loan Onboarding Service.
