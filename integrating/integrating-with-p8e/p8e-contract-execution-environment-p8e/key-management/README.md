---
description: Solutions to keeping your keys safe and organized
---

# Key Management

The Provenance Blockchain network relies on public key infrastructure on both the public and client-side layers. Like any blockchain network, the only thing stopping the value of assets stored on-chain from changing hands, or data from being decrypted in the Object Store is secure key management.

## Provenance Key Access Library

The p8e CEE API takes advantage of the Provenance Key Access Library, which provides a layer of abstraction between the API and Key Management System (KMS). Rather than sending keys in an API request, consumers of the API will send a token authorizing them to pull the key from the KMS, and an identifier for the particular key they wish to use.

There are many different KMS solutions on the market today, each providing their own methods for authentication and authorization. It is up to the entity hosting the API to decide which KMS to use and how best to authenticate users. By default, the library is configured to use [Hashicorp Vault](https://www.vaultproject.io) as a KMS. This guide will describe the default implementation, which is designed to be used locally during testing, as well as a solution for accessing keys in a test or production environment. To view the full source, please visit the Key Access Library [here](https://github.com/provenance-io/originator-key-access-lib).

## Storing Keys in Vault

Hashicorp Vault provides a solution for managing secrets and protecting sensitive data. It provides many authentication methods and many secrets engines for storing sensitive data. Access to a particular path in a secrets engine is governed by a policy that provides a declarative way to grant or forbid access to certain operations in Vault.

{% hint style="info" %}
Vault concepts such as [Authentication](https://www.vaultproject.io/docs/concepts/auth), [Identity](https://www.vaultproject.io/docs/concepts/identity), [Tokens](https://www.vaultproject.io/docs/concepts/tokens), [Policies](https://www.vaultproject.io/docs/concepts/policies), and each of the [Auth Methods](https://www.vaultproject.io/docs/auth) and [Secrets Engines](https://www.vaultproject.io/docs/secrets) are well documented.
{% endhint %}

### Local Development

The script that launches the multi-party p8e environment locally in Docker includes a Vault instance that automatically unseals itself, creates a [Key-Value](https://www.vaultproject.io/docs/secrets/kv/kv-v2) secrets engine called `kv2_originations`, and copies the local test keys provided by the default network configuration as secrets in that engine. The secrets are organized by organization in a directory called `originators`, meaning the "key"  for the secret is the organization's provenance member ID and the "value" of the secret is a JSON object containing key-value pairs for each key or mnemonic belonging that that organization. By default, there are three key pairs: encryption, signing, and auth.

Example path and naming convention:

```
// Org <UUID> keys would be found at the path
// http://127.0.0.1:8200/v1/kv2_originations/data/originators/<UUID>
{
    "public_encryption_key": "<value>",
    "private_encryption_key": "<value>",
    "public_signing_key": "<value>",
    "private_signing_key": "<value>",
    "public_auth_key": "<value>",
    "private_auth_key": "<value>",
    "private_mnemonic": "<value>"
}
```

#### Vault Token - No Authentication by Default

As Vault unseals itself, the script copies the root Vault token to a local file known to the API as the default location of the vault token to use with each request. For this reason, the API does not require the user to authenticate with Vault before each request, and instead simply requires the user to supply the ID of the organization that correlates to the key in the Key-Value pair.

{% hint style="info" %}
This is not a secure solution and should not be used for anything other than local development.
{% endhint %}

### Test and Production Environments

In a test or production environment, authentication against the chosen KMS is a requirement. Ultimately it is up to the tech services provider that operates the API to implement the authentication and authorization method of their choosing.

If they choose to implement Vault, a policy mapping an authenticated user to a specific path in a secrets engine would grant or forbid access to keys.

{% hint style="info" %}
Contact Figure Tech for information about the sandbox environment including the auth method of choice and how to get an API key.
{% endhint %}

## Wallet Connect

Coming soon!
