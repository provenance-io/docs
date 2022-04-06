---
description: Deploy, configure, profit
---

# Deployment

The full source code of the Loan Onboarding Service is available [here](https://github.com/figuretechnologies/service-loan-onboarding). \[TODO: Link open source project] The Loan Onboarding Service is a Kotlin implementation of a Spring based web application. Please view the [API Specification](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification) to learn how to interact with the API.&#x20;

### Local Environment

To run the service, we first need to start the relevant, dependent services managed as containers. While these can be started from the `dependencies.yaml` file in the Loan Onboarding Service repository, a helper script was written for easier startups.

`./dc.sh up` will download and start all relevant containers.

Once all containers have been started, the Spring application can be started via Gradle using the bootRun command: `./gradlew bootRun`

To interact with the freshly started, local provenance instance, use these configuration properties in your API calls.&#x20;

| Property Name      | Property Value                                   |
| ------------------ | ------------------------------------------------ |
| chainId            | chain-local                                      |
| nodeEndpoint       | [https://127.0.0.1:9090](https://127.0.0.1:9090) |
| objectStoreAddress | grpc://localhost:8081                            |



### Sandbox Environment

\[TODO: update the table below with actual sandbox environment values]

| Property Name      | Property Value                  |
| ------------------ | ------------------------------- |
| chainId            | pio-testnet-1                   |
| nodeEndpoint       | \<Node To Run Against>          |
| objectStoreAddress | \<Object Store To Run Against>  |

