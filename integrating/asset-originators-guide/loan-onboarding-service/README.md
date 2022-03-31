---
description: A variation of the Asset Onboarding Service that implements the p8e Scope SDK
---

# Loan Onboarding Service

In this section, you'll learn about a practical implementation of the [p8e-scope-sdk](https://github.com/provenance-io/p8e-scope-sdk), the [p8e-gradle-plugin](https://github.com/provenance-io/p8e-gradle-plugin), and the [metadata-asset-model](https://github.com/provenance-io/metadata-asset-model), which together can be used to register new p8e contract specifications ("smart contracts"), then execute those contracts to store data in the Encrypted Object Store and record assets on the Provenance Blockchain.

## Overview

Before we deploy and start using the Loan Onboarding Service, it's worth revisiting some key concepts from a practical perspective. In this section we will dive into:

Please view the full source of the Loan Onboarding Service [here](https://github.com/figuretechnologies/service-loan-onboarding). \[TODO: Link open source project] The Loan Onboarding Service is a Kotlin implementation of a Spring based web application. Please view the [API Specification](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification) to learn how to interact with the API.&#x20;

The Loan Onboarding Service takes advantage of the Provenance Key Access Library. As such, any originator specified in the API call to the Loan Onboarding Service must exist in the key management system so private keys can be pulled and consumed within the service. By default, the service is configured to use [Hashicorp's Vault](https://www.vaultproject.io). To view the full source, please visit the Key Access Library [here](https://github.com/provenance-io/originator-key-access-lib).

To run the service, we first need to start the relevant, dependent services managed as containers. While these can be started from the `dependencies.yaml` file, a helper script was written for easier startups.&#x20;

`./dc.sh up` will download and start all relevant containers.&#x20;

Once all containers have been started, the Spring application can be started via gradle. `./gradlew bootRun`

To interact with the freshly started, local provenance instance, use these configuration properties in your API calls.&#x20;

| Property Name      | Property Value                                   |
| ------------------ | ------------------------------------------------ |
| chainId            | chain-local                                      |
| nodeEndpoint       | [https://127.0.0.1:9090](https://127.0.0.1:9090) |
| objectStoreAddress | grpc://localhost:8081                            |

\[TODO: add deployment and configuration, networking considerations if any]

1. **Data modeling** - How to use the [metadata-asset-model](https://github.com/provenance-io/metadata-asset-model) to your advantage while providing the right data to register loans in downstream applications such as DART and Portfolio Manager
2. **P8e contracts** - Developing and publishing new contracts with a focus on a simple loan onboarding contract
3. **Key management and permissioning** - Some practical guidance on which keys to use and an overview of how data is stored, encrypted, decrypted, and shared within p8e
4. **API specification** - Three endpoints that enable publishing a new p8e contract, storing data in p8e, and recording a new asset on the Provenance Blockchain ledger

| Property Name      | Property Value                  |
| ------------------ | ------------------------------- |
| chainId            | pio-testnet-1                   |
| nodeEndpoint       | \<Node To Run Against>          |
| objectStoreAddress | \<Object Store To Run Against>  |

