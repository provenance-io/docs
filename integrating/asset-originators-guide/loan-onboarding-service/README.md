---
description: A variation of the Asset Onboarding Service that implements the p8e Scope SDK
---

# Loan Onboarding Service

In this section, you'll learn about a practical implementation of the [p8e-scope-sdk](https://github.com/provenance-io/p8e-scope-sdk), the [p8e-gradle-plugin](https://github.com/provenance-io/p8e-gradle-plugin), and the [metadata-asset-model](https://github.com/provenance-io/metadata-asset-model), which together can be used to register new p8e contract specifications ("smart contracts"), then execute those contracts to store data in the Encrypted Object Store and record assets on the Provenance Blockchain.

## Overview

Before we deploy and start using the Loan Onboarding Service, it's worth revisiting some key concepts from a practical perspective. In this section we will dive into:

* ****[**Data modeling**](data-mapping.md) - How to use the [metadata-asset-model](https://github.com/provenance-io/metadata-asset-model) to your advantage while providing the right data to register loans in downstream applications such as DART and Portfolio Manager
* ****[**P8e contracts**](p8e-contracts/) - Developing and publishing new contracts with a focus on a simple loan onboarding contract
* ****[**Key management and permissioning**](key-management.md) - Some practical guidance on which keys to use and an overview of how data is stored, encrypted, decrypted, and shared within p8e

After we brush up on those topics, we will cover the [API Specification](api-specification.md) and walk through the proper execution order:

1. Publishing a new p8e contract,
2. Storing data in p8e, and finally
3. Recording a new asset on the Provenance Blockchain ledger.
