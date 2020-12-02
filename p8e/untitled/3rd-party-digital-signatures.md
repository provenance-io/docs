# 3rd Party Digital Signatures

### Purpose <a id="DigitalSignature-Purpose"></a>

Data providers for Provenance should be capable of providing digital signatures that allow for third-party validation and authentication. This allows entities other than the originators to confirm that the data used to underwrite the asset was generated from a trusted third party \(such as a Credit Bureau\) and remained unchanged to create the asset.

### Method <a id="DigitalSignature-Method"></a>

To meet the digital signature requirements, data providers must be capable of providing the following three pieces of information:

1. The entity's certificate. This allows any other third party to confirm the information was signed using a private key specific to the data provider.  The entity will register their certificate with the Provenance Certificate Registry.  The certificate must include any required chain of trust to the certificate authority.  The public key should be ASN1 OID: prime256v1 and NIST CURVE: P-256.
2. The **canonicalized** original data. This is the original data package sent, in a standard canonical form.
3. The signature across the data. This is the outcome of the canonical original data sent through a checksum \(for example, SHA256\) and cryptographically signed with a private key.

#### **Original Message** <a id="DigitalSignature-OriginalMessage"></a>

As an example, assume the original message sent matches the below:

```text
<dataContent>
    <content>
        <any>any</any>
    </content>
</dataContent>
```

#### **Generate the Signature** <a id="DigitalSignature-GeneratetheSignature"></a>

To generate a signature:

* Canonicalize the message.  For XML use [https://www.w3.org/TR/xml-c14n2/](https://www.w3.org/TR/xml-c14n2/).  For JSON use [https://cyberphone.github.io/ietf-json-canon/](https://cyberphone.github.io/ietf-json-canon/)
* Run the canonicalized message through a SHA256 checksum
* Cryptographically sign the checksum with the certificate private key.  The signature output must be in ASN.1 DER format.
* Base64 encode the signed checksum
* Insert the Base64 encoded signed checksum into the **x-provenance-signature** response header.

```text
Validation
```

To validate this signature, use the certificate's public key to view the original SHA256 checksum. Also, run a checksum on the data used and compare the result. Both checksums should be identical as long as the underlying data has not been changed. The act of using the certificate public key to view the checksum verifies the source of the data, and the act of comparing the checksums verifies the data sent from the source was used to generate the financial asset.

### Signature Flow <a id="DigitalSignature-SignatureFlow"></a>

The following flow demonstrates how Figure Lending will integrate digital signatures into an existing service flow.

![](https://figure.atlassian.net/wiki/download/attachments/578781188/DigSigFlow.jpg?version=2&modificationDate=1573583401126&cacheVersion=1&api=v2)

1. Data Provider registers their certificate with the Provenance Certificate Registry
2. Figure Service submits a request to the Data Provider as normal
3. Data Provider processes the request as normal
   1. In addition to the default response, the Data Provider canonicalizes the default response data
   2. Then, the Data Provider creates a SHA256 checksum of the canonical data and signs it with their private key
   3. The signature is then Base64 encoded and returned in the **x-provenance-signature** response header.
4. Data Provider returns the default response and the signature created in step 3.
5. Figure Service handles the default response as normal.
6. Figure Service creates a Signature Packet:
   1. Create a canonical version of the default response
   2. Combine canonical response and the signature into the Signature Packet
7. Figure Service sends the Signature Packet to FIDO and publishes a Signature Message to the Signature Topic keyed by the loan application/packet UUID.
8. Once a loan is ready for Provenance on-boarding, DORF will consume and aggregate Data Provider signatures related to the loan:
   1. The Signature Packet is copied to Provenance, similar to existing documents.
   2. The Signature Packet is added to the loan packet documents section with a reference to the document location and a vendor type.
9. Signature validation is handled by Provenance post on-boarding.
   1. Invalid signatures will not block the Figure Service flow in any manner.
   2. When invalid signatures are encountered, the Signature Validator process will be responsible for alerting and updating loan validation messages.

