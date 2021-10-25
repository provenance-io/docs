# DIME \(Encryption Envelope Specification\)

### Dime \(**Data Instance with Metadata and Encryption**_**\)**_

The purpose of end-to-end encryption within the Provenance Blockchain protocol is to strongly control the intended audience of information.  Within the context of the protocol there are three areas to consider for information access.  These three areas are _Submission, Processing/Validation, and Retrieval._ Submitted data will exist in each of these three context in an unencrypted/viewable state assigned to specified audience.

#### Submission Context <a id="DIME(EncryptionEnvelopeSpecification)-SubmissionContext"></a>

The submission context is always assigned to the creator who submits information to the system.  While information may be transferred to another owner, it is impossible to submit data to the system and not be marked as the owner of it.  To clarify, if an asset is changing ownership this is performed by the existing owner adding permission to the new owner followed by _**the new owner retrieving the data and resubmitting it back to the system to finalize the change in ownership**_.

The Submission context participant is able to maintain access to the information indefinitely by either recording the asset ID and the clear text data encryption key, or maintaining their own private key and adding themselves to the audience of the item.  In the second case the owner would be able to either maintain the asset identifier with their private key or query the system for assets with a DEK assigned to their public key then restore the clear text DEK using their private key.

#### Processing Context <a id="DIME(EncryptionEnvelopeSpecification)-ProcessingContext"></a>

The processing context is similar to the final Retrieval context with the exception that the key and data handling practices are strongly defined by the smart contract and platform itself.  The overriding goal of the Processing Context is to keep the time the data spends in this area as short as possible and to destroy all keys to access the data as soon as reasonable.  In order to limit the scope of the processing context the first step is to only include the minimum number Smart Contract public keys required to successfully endorse a transaction.  This means that in many cases only 3 to 5 total smart contract instances out of potentially dozens spread across the Provenance Blockchain Network will be selected for access to the information.  During the smart contract execution output blocks are generated using a process that removes any DEKs assigned to the processing Context to eliminate any long term persistent smart contract access to the data.  The last two areas to focus on for reducing access are the DEKs passed into the smart contract in the parameters and the handling of the symmetric payload encryption key.  Frequent rotation of encryption keys by the smart contract reduces the time window when a DEK can be retrieved as old keys are permanently deleted.  The symmetric encryption key is the last and most sensitive piece of information because it provides permanent access to the resource \(as outlined in the submission and Retrieval contexts as an alternate access scheme\).  Due this sensitive nature of the key, it is decrypted right before any required use and immediately purged after use.

Given a prime design goal of the Processing Context is to limit its duration and audience extensive technical controls and monitoring are used to ensure the process is followed correctly and that no unauthorized entities are party to the transaction.

#### Retrieval Context <a id="DIME(EncryptionEnvelopeSpecification)-RetrievalContext"></a>

The final state of all blocks added to the chain is the Retrieval context.  Audience members \(and their DEKs\) are permanently committed to the chain ensuring a record of authorized access to each resource.  A member can query the API for entries by ID or for DEK entries that are tied to their public key.  The audience associated with the retrieval context is defined as the set of public keys that have a DEK recorded in the block chain.  Strictly speaking the Retrieval context is assumed to include the owning member regardless of the presence of a DEK in the audience list as the source of the encryption secrets was the submitting member/owner.  An entity from the Processing Context may be party to the retrieval context subject to policy and temporal constraints.

### Examples <a id="DIME(EncryptionEnvelopeSpecification)-Examples"></a>

#### Input Message <a id="DIME(EncryptionEnvelopeSpecification)-InputMessage"></a>

A message payload submitted to Provenance Blockchain APIs has the following structure.  Note that certain information is contained in cleartext form as part of the metadata including the sender, audience list, and basic identifiers.  The payload is a cipher text block that contains member information and associated results from the execution of the smart contract.

```text
[
    # Unencrypted data fields used for information lookup and retrieval. (Non-Sensitive information)
    metadata: [
        ownerId: [ guid ],
        dataId: [ guid ],
        sent: [ UTC ],
        expires: [ UTC ],
        # Additional key:value pairs as appropriate
        key: [ value ]
    ],

    # The sender of the message includes their public key as well as a signature over the [metadata|audience|payload]
    # in order to prove message integrity.
    sender: [
        publicKey,
        digitalSignature
    ],

    audience: [
        # DEK tags are built by the submitting member's SDK.  Each DEK is tagged with the associated audience context group
        # Only 1 submission context DEK is allowed along with 'N' number processing and retrieval context DEKs
        publicKey1: [ payloadId=1, context=submission, initializationVector, ephemeralPublicKey, messageAuthenticationCode, cipherTextDEK ]
        ...
        publicKeyN: [ payloadId=1, context=processing, initializationVector, ephemeralPublicKey, messageAuthenticationCode, cipherTextDEK ]
    ]

    # Payload is encrypted with AES in CTR mode using AES-GCM based on generated shared secret (DEK ciphertext above)
    # Multiple payloads are supported for cases where their maybe different audiences for field level access.
    payload: [
        [
           payloadId=1,
           cypherText = mgmb662mIjTiKUwp0C9u/XZT04G0mqVpz4GWaIbGlcL5tdYZDJLY7MH0k9MHY9/LaPLoqIePPicBLtoM5pQ4KdVULJpJVV8nADEKS
              6QL9YChWb+xoxQE2VXvjkdyS8Ezda5qV12k3lvst6ddtT6Unz0EqjKlAEy2TuPeWcOZK+r5n0Oyu9NRtagNU5uJ/3X/XfAMDZ9iVAhvL5ZnOZUf
              qikAN13gKmfUOnM8malMcbrQBJP26BpAFPNzUYtKP7FSUkcqSEHTA8xW/dlj/FW9lBtm0M3vF2gFUaSIOU8J4Z7r40Ffn2Jkrn0ubGnJ2jLK4TK
               [ ... ]
              sCTdAdjg6OclpBbtn8MSv0qutxY6+CbJ6VUCNx7QX84ydAjRCc28a+H3iethnvK+IfMbKSrGlm6036GSCKlQfgLmMvXn7GA93ad1KXSs/Uasdgn
              OUfC1SVlrTu7ufRk5oOmt41qyZkHAX/pxC8TEFtoXOtr5oIIDG5nGVWLtlX7HlHuv5lrrL54BZkwWY7CVuKqeI+Ybqwo=
        ],
        [
           payloadId=n,
           cypherText = mgmb662mIjTiKUwp0C9u/XZT04G0mqVpz4GWaIbGlcL5tdYZDJLY7MH0k9MHY9/LaPLoqIePPicBLtoM5pQ4KdVULJpJVV8nADEK
             6QL9YChWb+xoxQE2VXvjkdyS8Ezda5qV12k3lvst6ddtT6Unz0EqjKlAEy2TuPeWcOZK+r5n0Oyu9NRtagNU5uJ/3X/XfAMDZ9iVAhvL5ZnOZUf
             qikAN13gKmfUOnM8malMcbrQBJP26BpAFPNzUYtKP7FSUkcqSEHTA8xW/dlj/FW9lBtm0M3vF2gFUaSIOU8J4Z7r40Ffn2Jkrn0ubGnJ2jLK4TK
              [ ... ] 
             sCTdAdjg6OclpBbtn8MSv0qutxY6+CbJ6VUCNx7QX84ydAjRCc28a+H3iethnvK+IfMbKSrGlm6036GSCKlQfgLmMvXn7GA93ad1KXSs/Uasdgn
             OUfC1SVlrTu7ufRk5oOmt41qyZkHAX/pxC8TEFtoXOtr5oIIDG5nGVWLtlX7HlHuv5lrrL54BZkwWY7CVuKqeI+Ybqwo=
        ]
    ]
]
```

#### Payload Output Block <a id="DIME(EncryptionEnvelopeSpecification)-PayloadOutputBlock"></a>

The payload output block is the structure of elements created by smart contract and submitted for inclusion in the block chain.  For the audience of this block and their associated DEKs see the DEK Output Block.

```text
[
    metadata: [
        
        dataId: [ guid ],
        # The owner id points to the member who created this record. As part of committing the block to the chain the time this occurs is part of the permanent record.
        owner: [ guid ],

        attributes: [
            chainCodeResultAttribute1: [ ... ]
            [ ... ]
            chainCodeResultAttributeN: [ ... ]
        ]
    ],

    # Payload is encrypted with AES in CTR mode using AES-GCM
    payload: [
        [
           payloadId=1,
           cypherText = mgmb662mIjTiKUwp0C9u/XZT04G0mqVpz4GWaIbGlcL5tdYZDJLY7MH0k9MHY9/LaPLoqIePPicBLtoM5pQ4KdVULJpJVV8nADEKS
              6QL9YChWb+xoxQE2VXvjkdyS8Ezda5qV12k3lvst6ddtT6Unz0EqjKlAEy2TuPeWcOZK+r5n0Oyu9NRtagNU5uJ/3X/XfAMDZ9iVAhvL5ZnOZUf
              qikAN13gKmfUOnM8malMcbrQBJP26BpAFPNzUYtKP7FSUkcqSEHTA8xW/dlj/FW9lBtm0M3vF2gFUaSIOU8J4Z7r40Ffn2Jkrn0ubGnJ2jLK4TK
               [ ... ]
              sCTdAdjg6OclpBbtn8MSv0qutxY6+CbJ6VUCNx7QX84ydAjRCc28a+H3iethnvK+IfMbKSrGlm6036GSCKlQfgLmMvXn7GA93ad1KXSs/Uasdgn
              OUfC1SVlrTu7ufRk5oOmt41qyZkHAX/pxC8TEFtoXOtr5oIIDG5nGVWLtlX7HlHuv5lrrL54BZkwWY7CVuKqeI+Ybqwo=
        ],
        [
           payloadId=n,
           cypherText = mgmb662mIjTiKUwp0C9u/XZT04G0mqVpz4GWaIbGlcL5tdYZDJLY7MH0k9MHY9/LaPLoqIePPicBLtoM5pQ4KdVULJpJVV8nADEKS
             6QL9YChWb+xoxQE2VXvjkdyS8Ezda5qV12k3lvst6ddtT6Unz0EqjKlAEy2TuPeWcOZK+r5n0Oyu9NRtagNU5uJ/3X/XfAMDZ9iVAhvL5ZnOZUf
             qikAN13gKmfUOnM8malMcbrQBJP26BpAFPNzUYtKP7FSUkcqSEHTA8xW/dlj/FW9lBtm0M3vF2gFUaSIOU8J4Z7r40Ffn2Jkrn0ubGnJ2jLK4TK
              [ ... ] 
             sCTdAdjg6OclpBbtn8MSv0qutxY6+CbJ6VUCNx7QX84ydAjRCc28a+H3iethnvK+IfMbKSrGlm6036GSCKlQfgLmMvXn7GA93ad1KXSs/Uasdgn
             OUfC1SVlrTu7ufRk5oOmt41qyZkHAX/pxC8TEFtoXOtr5oIIDG5nGVWLtlX7HlHuv5lrrL54BZkwWY7CVuKqeI+Ybqwo=
        ]
    ]

]
```

#### DEK Output Block <a id="DIME(EncryptionEnvelopeSpecification)-DEKOutputBlock"></a>

A separate message from the payload blocks containing Data Encryption Keys that adds members to the intended audience.  Each DEK Output Block contains a unique identity reference to the Payload Block associated with the key.  A DEK Output Block can be committed at any time after the source block is added to the chain if additional audience members need to be given access to the data.

```text
[
    metadata: [
        dataId: [ guid ],
        audience: [
            # DEK tags are built by the data owner using the SDK when the request is submitted. (Only one submission with 'N' retrieval contexts are valid)
            publicKey1: [ context=submission, initializationVector, ephemeralPublicKey, messageAuthenticationCode, cipherTextDEK ]
            ...
            publicKeyN: [ context=retrieval, initializationVector, ephemeralPublicKey, messageAuthenticationCode, cipherTextDEK ]
        ]
    ],
]
```

