---
description: A variation of the Asset Onboarding Service that implements the p8e Scope SDK
---

# Loan Onboarding Service

In this section, you'll learn about a practical implementation of the [p8e-scope-sdk](https://github.com/provenance-io/p8e-scope-sdk) that can be used to register new p8e contract specifications, then execute those contracts to store data in the Encrypted Object Store and record those assets on the Provenance Blockchain.

## Deployment

Please view the full source of the Loan Onboarding Service [here](https://github.com/figuretechnologies/service-loan-onboarding). \[TODO: Link open source project] The Loan Onboarding Service is a Kotlin implementation of a Spring based web application. Please view the [API Specification](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification) to learn how to interact with the API.&#x20;

The Loan Onboarding Service takes advantage of the Provenance Key Access Library. As such, any originator specified in the API call to the Loan Onboarding Service must exist in the key management system so private keys can be pulled and consumed within the service. By default, the service is configured to use [Hashicorp's Vault](https://www.vaultproject.io). To view the full source, please visit the Key Access Library [here](https://github.com/provenance-io/originator-key-access-lib).

### Local Deployment

To run the service, we first need to start the relevant, dependent services managed as containers. While these can be started from the `dependencies.yaml` file, a helper script was written for easier startups.&#x20;

`./dc.sh up` will download and start all relevant containers.&#x20;

Once all containers have been started, the Spring application can be started via gradle. `./gradlew bootRun`

To interact with the freshly started, local provenance instance, use these configuration properties in your API calls.&#x20;

| Property Name      | Property Value                                   |
| ------------------ | ------------------------------------------------ |
| chainId            | chain-local                                      |
| nodeEndpoint       | [https://127.0.0.1:9090](https://127.0.0.1:9090) |
| objectStoreAddress | grpc://localhost:8081                            |

### Test Deployment

To interact with the public Provenance testnet, use these configuration properties in your API calls&#x20;

| Property Name      | Property Value                  |
| ------------------ | ------------------------------- |
| chainId            | pio-testnet-1                   |
| nodeEndpoint       | \<Node To Run Against>          |
| objectStoreAddress | \<Object Store To Run Against>  |

