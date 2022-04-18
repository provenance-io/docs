---
description: Practical guide to deploying the Object Store
---

# Deploying the Encrypted Object Store

## Local Deployment

A local environment can help get you acquainted with the object store. Typical use cases will require at least two object stores, to simulate data being shared between two separate participants.

A script for deploying a local environment in Docker with two object stores and a Provenance node is available in the p8e CEE API repository. It stands up two object stores backed by a PostgreSQL data store along with a single Provenance Blockchain node. The object stores and blockchain node provide an environment to exercise the various operations needed to understand how data is stored in p8e. This repository also provides a web-server you can use to exercise and manipulate your environment to meet your needs.

![Local Environment Setup](<../../../../.gitbook/assets/Post Close - Local Env Setup (6).png>)

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

## Sandbox Environment

{% hint style="info" %}
Figure hosts and operates a sandbox p8e environment that is configured to work with the Provenance testnet for potential asset originators to play with. Contact Figure for more information.
{% endhint %}
