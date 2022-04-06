---
description: Solutions to keeping your keys safe and organized
---

# Key Management

The Provenance Blockchain network relies on public key infrastructure on both the public and client-side layers. The only thing stopping the value encapsulated in an asset that is stored on-chain from changing hands, or data from being decrypted in the Object Store is secure key management.

## Keep your keys from falling into the wrong hands

The Loan Onboarding Service takes advantage of the Provenance Key Access Library. As such, any originator specified in the API call to the Loan Onboarding Service must exist in the key management system so private keys can be pulled and consumed within the service. By default, the service is configured to use [Hashicorp's Vault](https://www.vaultproject.io). The choice of Key Management solution is ultimately up to the asset originator. To view the full source, please visit the Key Access Library [here](https://github.com/provenance-io/originator-key-access-lib).

\[TODO: practical guide to storing keys/mnemonic in Vault]

\[TODO: implementing wallet connect as an option]
