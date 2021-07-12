# Signing and Encryption

Signing and Encryption is a critical part of P8e's way of managing data signing and transaction between the client's data, Object Store and the the Blockchain.

## Signing

P8e provides a couple of implementations for signing base on the way the client decides to store their affiliate key pair. 

### Pen

The Pen class uses the standard java security Signature implementation to sign data given that the affiliate key pair is stored within P8e's affiliate database.


### SmartKey

SmartKey (https://www.equinix.com/services/edge-services/smartkey) is a 3rd party key custody service provider that stores an affiliate's private key within an HSM (Hardware Security Module). P8e leverages the signing API provided from SmartKey to sign data before being sent to Object Store and the data verification process uses the java security Signature amongst all key custody providers.


## Encryption

P8E uses an ECIES public key authentication schema to encrypt and decrypt data, based on the affiliate's type, it would either retrieve the encryption key pair from either the P8e local database or from the specified key custody provider. There is only a minor deviation in computing the KDF involving the respective key custody provider's API, but will continue to decrypt at a common manner. 
