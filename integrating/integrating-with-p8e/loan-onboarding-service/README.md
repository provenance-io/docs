---
description: A variation of the Asset Onboarding Service that implements the p8e Scope SDK
---

# p8e CEE API

In this section, you'll learn about a practical implementation of the [p8e-scope-sdk](https://github.com/provenance-io/p8e-scope-sdk), the [p8e-gradle-plugin](https://github.com/provenance-io/p8e-gradle-plugin), and the [metadata-asset-model](https://github.com/provenance-io/metadata-asset-model), which together can be used to register new p8e contract specifications ("smart contracts"), then execute those contracts to store data in the Encrypted Object Store and record assets on the Provenance Blockchain.

We will cover the [API Specification](api-specification/) and walk through the proper execution order:

1. Publishing a new p8e contract,
2. Storing data in p8e, and finally
3. Recording a new asset on the Provenance Blockchain ledger.

### Deploying Locally

To run this service locally, be sure to have [Docker](https://www.docker.com/) and [Vault by Hashicorp](https://www.vaultproject.io/) installed:

```
brew install docker
```

```
brew tap hashicorp/tap
brew install hashicorp/tap/vault
```

once installed, all you need to do is run the included docker setup script

```
./dc.sh up
```

and run the service - either via an Intellij run configuration or via the command line with the following command:

```
./gradlew bootRun
```

### Provenance Blockchain Member ID

With each request to the Contract Execution Environment endpoints, it's important to identify which Provenance Blockchain member is making the request to sign the transaction. To do this, the p8e-cee-api requires an `x-uuid` header on every request.

{% hint style="info" %}
When testing locally, be sure to include the `x-uuid` header with a value that correlates to one of the UUID's added as secrets to Vault in the `dc.sh up` script.
{% endhint %}

{% hint style="info" %}
The `x-uuid` header is automatically removed from requests made to the test or production environments and replaced with the UUID that matches the API Key used in the request. See next section for more detail.
{% endhint %}

### API Key for Test or Production Environments

Figure Tech uses [Kong API Gateway](https://konghq.com/kong) to handle ingress into its Kubernetes clusters. The Figure Tech hosted instances of the `p8e-cee-api` are secured by specific API Keys for each participant in the Provenance Blockchain network.

When used, Kong will authenticate an API Key against an ACL, strip any provided `x-uuid` header and add a new `x-uuid` header with the appropriate UUID associated with the API Key.

{% hint style="info" %}
API Keys are available upon request.
{% endhint %}
