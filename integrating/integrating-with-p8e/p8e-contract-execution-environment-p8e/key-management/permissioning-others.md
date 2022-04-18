---
description: When to use each key
---

# Permissioning Others

When storing data in the Encrypted Object Store, asset originators have to the choice to encrypt that data with one or more keys. First and foremost, you will want to encrypt your own data with your own public encryption key. After that, the choice is yours to permission other entities such as your business partners or various down stream applications like Portfolio Manager and DART.

The primary mechanism for providing this support is done by adding additional audience members to stored assets. Please reference [DIME (Encryption Envelope Specification)](https://docs.provenance.io/p8e/overview/encrypted-object-store/dime-encryption-envelope-specification#dime-encryptionenvelopespecification-retrievalcontext) for additional information.

When storing objects against Object-Store, additional audiences can be specified via the REST API call config object. This field is a list of base64 encoded public keys.&#x20;

```
{
  "audiences": [<Base64EncodedPublicKey>],
  ...
}
```

To make onboarding loan data in such a way that it is accessible to DART and/or Portfolio Manager, the Loan Onboarding Servicer has two additional parameters that turn those decisions into a binary decision.

```
{
  "audiences": [<Base64EncodedPublicKey>],
  "permissionDart": true,
  "permissionPortfolioManager": true,
  ...
}
```

Passing those two values as `true` will add the public keys for each service to the audiences list that gets submitted to p8e. Asset originators can decide for themselves which services they want to permission on an asset-by-asset basis.

### Replication and Decryption

If you haven't already, check out the section on Configuring Replication between two object stores. Replication is the primary method for sharing data with business partners. When configured correctly, encrypting data with your partner's public key will automatically trigger replication and allow that partner to decrypt your data. Conversely, failing to add a partner as an additional audience will prevent them from receiver and decrypting the data.
