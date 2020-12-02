# Encryption Scheme

Provenance uses Elliptic Curve Integrated Encryption Scheme \(ECIES\) to encrypt Member information before it is processed and stored on the Provenance blockchain.  As its name properly indicates, ECIES is an integrated encryption scheme which uses the following functions:

* Key Agreement \(KA\): Function used for the generation of a shared secret by two parties.
* Key Derivation Function \(KDF\): Mechanism that produces a set of keys from keying material and some optional parameters.
* Encryption: Symmetric encryption algorithm.
* Message Authentication Code \(MAC\): Data used in order to authenticate messages.
* Hash \(SHA-256\): Digest function, used within the KDF and the MAC functions.

The Provenance ECIES scheme follows the ISO/IEC 18033-2 standard utilizing the following parameters:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Definition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Key Agreement</td>
      <td style="text-align:left">Diffie-Hellman</td>
    </tr>
    <tr>
      <td style="text-align:left">Key Encapsulation Method</td>
      <td style="text-align:left">
        <p>ECIES-KEM, as defined in ISO 18033-2:</p>
        <ul>
          <li>EC: prime256v1 (NIST P-256)</li>
          <li>CheckMode, OldCofactorMode, SingleHashMode, and CofactorMode are 0</li>
          <li>Point format is uncompressed</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Key Derivation Function</td>
      <td style="text-align:left">
        <p>HMAC-based with SHA-256 (HKDFwithSHA256)</p>
        <p>Binary representation of an Ephemeral Public Key used as optional parameter</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Symmetric Encryption Algorithm</td>
      <td style="text-align:left">AES-256 with Zero IV</td>
    </tr>
    <tr>
      <td style="text-align:left">Message Authentication Code Algorithm</td>
      <td style="text-align:left">
        <p>HMAC-SHA-256</p>
        <p>Invoking Member UUID used as optional parameter.</p>
      </td>
    </tr>
  </tbody>
</table>

### Encrypting Messages <a id="EncryptionScheme-EncryptingMessages"></a>

![](https://figure.atlassian.net/wiki/download/thumbnails/402096129/image2018-8-3_11-15-25.png?version=1&modificationDate=1533316527853&cacheVersion=1&api=v2&width=768&height=817)

1. The Member uses the Provenance SDK to submit a message to the Provenance blockchain.
2. A random, symmetric Data Encryption Key \(**DEK**\) is generated for each Member message.  The DEK is used to encrypt the message using AES.
3. The Audience is a set of Provenance member public keys \(**ApubK**\) that are allowed to decrypt the encrypted the message.  The SDK will automatically add all whitelisted Node public keys to the Audience.  There may be 1 or many ApubKs.
   1. The SDK will provide a means for the Member to select audience members \(i.e. other Provenance members\).
4. A shared secret must be generated to allow the Audience to decrypt the DEK to subsequently decrypt the message.
   1. An ephemeral key pair is generated where the public key \(**EpubK**\) is derived from the private key \(**EprivK**\).
   2. The Key Agreement \(**KA**\) function uses the EprivK and ApubK to generate a secret \(**Secret**\).
   3. The Key Derivation Function \(**KDF**\) uses the Secret and the EpubK encoded as a byte array parameter to generate the Message Authentication Code key \(**MAC Key**\) and the Encryption Key \(**ENC Key**\).
5. The DEK is encrypted with the ENC Key using AES GCM resulting in an **Encrypted DEK**.
6. With the Encrypted DEK, the MAC Key, and the Member UUID as parameters a MAC function \(using AES GCM\) is used to produce a tag \(**Tag**\).
   1. The Tag is an AES-GCM authentication tag that is to be used during decryption validation.
7. A Provenance Data Integrity & Message Encryption \(**DIME**\) packet is created that contains:
   1. A cryptogram \(Tag, EpubK, Encrypted DEK\) for each ApubK including a cryptogram for MpubK.
   2. A payload block with the Encrypted Message.
   3. Metadata about the DIME like asset ID, date, key value sets.
8. Because the DEK is randomly generated, it shall be kept by the Member.  The SDK wraps the DEK in an ECIES cryptogram using the Members public key \(MpubK\) that is returned via the SDK to the Member. The clear DEK is never transmitted.

### Processing Encrypted Messages in Smart Contracts <a id="EncryptionScheme-ProcessingEncryptedMessagesinSmartContracts"></a>

This section describes how Provenance smart contracts process encrypted information created in the previous section.  A Provenance smart contract runs on the minimum number of eligible Nodes required by its endorsement policy.  Thus, the context of the execution outlined in the following diagram is a Node.

![](https://figure.atlassian.net/wiki/download/thumbnails/402096129/image2018-8-3_12-51-0.png?version=1&modificationDate=1533322261853&cacheVersion=1&api=v2&width=768&height=812)

When a Provenance smart contract is installed and then instantiated on a Node \(A\), it creates a public and private key pair and publishes the public key \(**NpubK**\) to the Provenance Key Management Server.  The smart contract keeps its private key \(**NprivK**\) in memory.  On a scheduled basis \(B\) the smart contract rotates and publishes its key pair.

1. In the previous section, the Member submitted a message to the Provenance protocol via the SDK.  The SDK encrypted the message and created a set of audience cryptograms \(**ApubK**\[n\] Cryptogram\).
   1. The audience cryptogram set is a list of Nodes that will process the message _plus_ a list of additional Provenance members that are granted permission to view the unencrypted message.
   2. The audience cryptogram is submitted to the smart contract in a transient data space.  Thus, this information is not persisted in the blockchain read set and therefore cannot be viewed by any Nodes after the smart contract executes.
   3. The Encrypted Message and Message Metadata is passed to the smart contract as a parameter and will be included in the blockchain read set.
2. The smart contract run uses its **NpubK** to determine if it is in the ApubK\[n\] Cryptogram set.
   1. If the smart contract is not a valid ApubK\[n\] Cryptogram audience member, the smart contract throws an exception and the transaction is rejected.
3. The smart contract extracts the **Tag**, **EpubK**, and **Encrypted DEK** from its ApubK\[n\] Cryptogram into memory.
4. The ECIES shared secret must be generated to decrypt the Encrypted DEK
   1. The same Key Agreement function \(**KA**\) used to encrypt the DEK is followed.  During decryption, the KA function uses the EpubK from the cryptogram and the smart contract's NprivK to generate a **Secret**.
   2. The Key Derivation Function \(**KDF**\) uses the Secret and the EpubK encoded as a byte array parameter to generate the Message Authentication Code key \(**MAC Key**\) and the Encryption Key \(**ENC Key**\).
5. With the MAC Key, the Encrypted DEK, and the invoking Member's UUID a new Tag is computed and compared with the Tag from the ApubK\[n\] Cryptogram.
   1. If the tags do not match, the smart contract throws and exception and the transaction is rejected.
6. With the ENC Key, the Encrypted DEK is decrypted into a clear DEK.
7. The Encrypted Message from the DIME is decrypted using the DEK resulting in a **Clear Message**.
8. The smart contract processes the Clear Message including:
   1. Message validation
   2. Altering the message
   3. Reading the DIME Message Metadata \(e.g. asset ID, information about the message like key value sets\) and including or altering the message based on the metadata.  May alter the metadata.
9. When the smart contract is done processing the Clear Message, it is encrypted with the DEK resulting in a new Encrypted Message.
   1. The DEK is destroyed from memory.
10. The audience cryptograms are processed removing all cryptograms where the cryptogram type is a smart contract \(Node\).  The result is a set of audience cryptograms containing only Provenance members that are privileged to decrypt the message.
11.  The smart contract wraps the filtered cryptogram set, along with Message Metadata in a **Message Audience DIME** - this DIME is written to the blockchain and provides the member permissions to the message data.
12. Finally, the smart contract wraps the Encrypted Message and Message Metadata in a DIME and writes it to the blockchain.

