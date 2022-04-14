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

Deploying a local environment in Docker with two object stores and a Provenance node is available in \<URL to playground repo here>. This stands up two object-stores backed by a Postgres data store along with a provenance node.  The object-stores and provenance node provide an environment to exercise the various operations needed to utilize the Provenance blockchain.  This repository also provides a web-server you can use to exercise and manipulate your environment to meet your needs.&#x20;

![Local Environment Setup](<../../.gitbook/assets/Post Close - Local Env Setup (6).png>)

Depending on your testing needs, you may choose to just operate on one of the two Object Stores, but two have been provided to support simulating operations across two separate Object Stores.

#### Configuration

By default, the components are listening on the following ports:

| Component                  | Container Name | Port(s)                                      |
| -------------------------- | -------------- | -------------------------------------------- |
| Provenance Blockchain Node | provenance     | <p>1317:1317<br>9090:9090<br>26657:26657</p> |
| Postgres                   | postgres       | 5432:5432                                    |
| Vault                      | vault          | 8200:8200                                    |
| Object Store 1             | object-store-1 | 5001:8081                                    |
| Object Store 2             | object-store-2 | 5002:8082                                    |

#### Requirements

The general system requirements to spin up this test environment locally are as follows, but do check out [README](https://github.com/provenance-io/p8e-cee-api) for more details.

1. Git command line
2. Terminal that can run shell scripts (e.b. Mac Terminal, Bash)
3. Docker and Docker Compose
4. Gradle
5. [Vault](https://www.vaultproject.io/docs/install) installed on your local OS



#### Setup

Download the repo.

```
git clone https://github.com/provenance-io/p8e-cee-api
cd p8e-cee-api
./gradlew clean build
```

Setup Docker containers as detailed in the image above using a script.&#x20;

```
./dc.sh up
```

Start the web server.

```
./gradlew bootRun  
```

## Available Local Object Store Operations

Coming soon!

### Enable p8e Object Store Replication

Some situations arise where you want the data written to one p8e Object Store to be replicated to another p8e Object Store.  This is commonly required to allow desired assets to be shared across parties with parties sharing only public information and not having to expose their private secrets.

A success execution of the enable replication endpoint will result in all objects stored into the specified source object store into the target object store with the same object hash.  That allows both the source and target object store to retrieve that object using their own URI "object:\<Object Store URL>/\<hash>".  An example URI for a p8e Object Store 1 created asset is "object://localhost:8081/ztJjRfkG1Rn8ISOjWYIevVRWhxvcAA9Ou0FN+GLJiPc=".

\<Put specs here>

## Sandbox Environment

{% hint style="info" %}
Figure hosts and operates a sandbox p8e environment that is configured to work with the Provenance testnet for potential asset originators to play with. Contact Figure for more information.
{% endhint %}

## What About the rest of the Contract Execution Environment?

So far, we've only implemented the Encrypted Object Store. The other half of the p8e Contract Execution Environment is not an individual component to deploy and configure, but an SDK that gets implemented by an application.

Once you have the Encrypted Object Store deployed and configured, move to the next page to learn about how the p8e-scode-sdk is implemented in the sample Loan Onboarding Service.
