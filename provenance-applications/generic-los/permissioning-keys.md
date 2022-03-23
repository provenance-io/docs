# Permissioning Keys

Permissioning keys for various applications to be used against onboarded assets is an important usecase of the generic LOS. The primary mechanism for providing this support is done by adding additional audience members to stored assets. Please reference [DIME (Encryption Envelope Specification)](https://docs.provenance.io/p8e/overview/encrypted-object-store/dime-encryption-envelope-specification#dime-encryptionenvelopespecification-retrievalcontext) for additional information.&#x20;

For security purposes, it is recommended that keys are stored in secure, access-controlled environments such as [Vault](https://www.vaultproject.io).&#x20;

When storing objects against Object-Store, additional audiences can be specified via the REST API call config object. This field is a list of base64 encoded public keys.&#x20;

```
{
  "audiences": [<Base64EncodedPublicKey>],
  ...
}
```
