---
description: How to record a loan
---

# Loan Onboarding

{% hint style="info" %}
The intended audience for this page is a Loan Originator who is operating their own Loan Origination System (LOS) or their LOS provider, but a similar process could be used for any asset being onboarded to Provenance.
{% endhint %}

As a borrower moves throughout the application stage of the mortgage process, data and documents are collected by the loan originator in their LOS. Once the application reaches an appropriate milestone, such as closing, the loan originator can start onboarding the loan to Provenance.

### Onboarding Loan Documents

Recall that in the [Loan Package data model](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/data-mapping), loan document metadata is stored separately from the loan application and underwriting data - in a separate Fact. Also, rather than store every byte of every loan document within the loan scope, the `documents` Fact will contain a list of loan document metadata. Each entry contains the URI to the actual object and a checksum of the contents to provide evidence of tampering with that off-chain object.

Technically speaking, those documents could be stored anywhere, however, the Encrypted Object Store is a great way to store documents such that they can be shared with business partners and downstream applications. Both DART and Portfolio Manager are built to look for documents in the Encrypted Object Store. Storing documents as encrypted objects in the Objects Store allows for automated replication and less integration work between business partners.

Therefore, step one of the loan onboarding process is to start inserting documents into the EOS as they become available. The [Create Object](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-object-in-object-store) endpoint handles creating individual objects in the object store without memorializing them as scopes on Provenance. The curl command below provides an example.

```
curl --location \
--request POST '${HOST}/api/v1/eos' \
--header 'Content-Type: application/json' \
--data-raw '{
    "account": {
        "originatorUuid": "<Originator's Provenance Member ID>"
    },
    "assetId": "74a764ba-3eeb-4271-8337-48a2d8434e1d", // Unique document UUID
    "objectStoreAddress": "grpc://object-store-v2.p8e:80",
    "asset": "aGVsbG8gd29ybGQ=", // Base64 encoded object/file
    "permissionDart": true,
    "permissionPortfolioManager": true,
    "audiences": [], // here is where you would list additional public keys belonging to business partners, allowing them to decrypt this object with their associated private keys
    "isTestNet": true
}'
```

{% hint style="info" %}
If you are working with Figure Tech to integrate, they can provide your Provenance Member ID and appropriate Object Store Address.
{% endhint %}

The "asset" in this case would be the loan document represented as a base 64 encoded byte array. It's important to consider whether or not to permission certain other participants at this stage. Figure Tech recommends permissioning the following:

* **Portfolio Manager -** Permission if you intend on accessing, pledging, or selling this loan on Portfolio Manager.&#x20;
* **DART** - Permission if you intend on registering this loan and the associated eNote with DART, which will automatically track life of loan updates.
* **3rd Party Validators** - Permission to 3rd party validators if you intend on requesting validation services for this loan.

{% hint style="info" %}
It is important for integrators to keep track of the metadata returned by the Object Store when it creates a document. They will need to generate a list of documents when it comes time to create the loan scope in the next step.
{% endhint %}

{% hint style="info" %}
Figure Tech is actively integrating with 3rd Party document preparation vendors to receive certain documents, such as the eNote, directly.
{% endhint %}

### Onboarding the Loan Package

Once documents are stored in EOS, and the loan application process is in a stage where it is worth onboarding the loan data to Provenance, the loan originator should hit the [Onboard Scope](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-scope-tx) endpoint. Instead of a document being passed as the "asset", you will use the Loan Package protocol buffer.

{% hint style="info" %}
Ultimately the stage at which an originator onboards a loan to Provenance is an internal business decision. Remember that there is a fee to write each update to the loan scope in the form of [gas](../../../../blockchain/basics/gas-and-fees.md). A suggestion would be to onboard the loan either at the point in which you would no longer want to update the base loan data, or at the point you would want to share one common set of loan application data with a 3rd party, such as a validator, servicer, warehouse lender, or investor.
{% endhint %}

The Onboard Scope endpoint requires the consumer to specify:

* which network to execute the contract on (mainnet vs. testnet vs. local),
* which account will sign off on the transaction,
* which scope (asset ID) to execute the p8e contract on,
* which p8e contract to execute, and&#x20;
* inputs to the contract, in this case a Loan Package protocol buffer.

Here is where the loan originator has some flexibility in regards to how they want to represent the loan on Provenance. As mentioned in [Data Mapping](../../lending-ecosystem/data-mapping.md), you can either map the loan data from your LOS to the predefined Loan proto provided in the metadata-asset-model library, or encode the loan data from your LOS into one of the MISMO Reference Model XML formats.

{% hint style="info" %}
Version 3.4 of the MISMO Reference Model is recommended.
{% endhint %}

Depending on what stage of the loan application process, you may be able to fill in one or more of the Loan Package Facts. At a minimum, you are expected to provide:

* **The** `asset` **Fact** - Including either the Loan proto or MISMOLoan proto
* **The** `servicingRights` **Fact** - Naming the servicer and sub-servicer, if applicable
* **The** `documents` **Fact** - List of known documents (created in step one)

As a best practice, include as many Facts as are available. That translates to including:

* **The** `loanStates` **Fact** - Include the initial loan state if you want the loan to appear in Portfolio Manager (Portfolio Manager uses this Fact to determine the remaining value of any loan)
* **The** `validation` **Fact** - If you already plan to request validation from a 3rd party service provider, include a `ValidationRequest` proto in the `validation` Fact
* **The** `eNote` **Fact** - if you have stored an eNote in either the DART eVault or other, eternal eVault, then you can specify the `eNote` Fact

{% hint style="info" %}
If you use a 3rd party document services provider that is directly integrated with Provenance to generate and send eNotes, then the `eNote` Fact may already be populated for you. Simply omit that fact in the input object - it cannot be overwritten by executing the onboard contract. This is designed to prevent mistakes.
{% endhint %}

An example of a fully formed Loan Package proto using the MISMO XML is provided below. In this example, the loan originator is providing the asset, servicing rights, document list, and initial loan state. They are leaving out the validation and eNote facts because they are not requesting validation at this time and are using a 3rd party document servicer provider that will send the eNote to Provenance separately. The p8e contract will automatically combine this loan data with an eNote once both are onboarded.

<details>

<summary>Fully Formed Loan Package Proto</summary>

```
{
    "asset" : {
        "id": "<uli>",
        "type": "MORTGAGE",
        "description": "MORTGAGE LOAN-1234",
        "kv": {
            "loan": {
                "typeUrl": "<mismo loan type>",
                "uri": "<uri>",
                "data": "<base64 encoded byte array>"
            }
        }
    },
    "servicingRights": {
        "servicerUuid": "<Servicer ID>",
        "servicerName": "Loan Servicing, Inc."
    },
    "documents": [
        {
            "id": "<UUID>",
            "uri": "<EOS URI>",
            "fileName": "<Document Name>",
            "ContentType": "application/pdf",
            "documentType": "<Document Type>",
            "checksum": "<File sha512 Hash>"
        },
        ...entire list of loan documents
    ],
    "loanStates": {
        "loanId": "<uli>",
        "assetType": {
            "supertype": "MORTGAGE",
            "subtype": "JUMBO"
        },
        "currentBorrowerInfo": {
            "primary": {
                ... See Person proto
            }
        },
        loanStateList: [
            {
                ... See LoanState proto
            }
        ]
    },
    "validation": null,
    "eNote": null
}
```

</details>

The curl command below will onboard a loan package to the Encrypted Object Store by executing the Record Loan p8e contract, and broadcast a fully formed Provenance Blockchain transaction proposal containing the appropriate hashes of the inputs and outputs of that contract to the Provenance Blockchain memory pool. There, it will get picked up by validators and memorialized as a scope on the Provenance Blockchain ledger.

```
curl --location \
--request POST '${HOST}/api/v1/p8e/onboard' \
--header 'Content-Type: application/json' \
--data-raw '{
    "chainId": "pio-testnet-1", // Swap with mainnet chain ID for prod
    "nodeEndpoint": "grpc://192.168.1.242:9090", // Swap with mainnet node for prod
    "txRequest": {
        "account": {
            "originatorUuid": <Originator's Provenance Member ID>,
            "keyRingIndex": "0",
            "keyIndex": "0",
            "isTestNet": true,
        },
        "permissions": {
            "audiences": [],
            "permissionDart": true,
            "permissionPortfolioManager": true
        },
        "scopeSpecId": "<Mortgage Scope Spec ID>",
        "scopeId": "<ULI converted to UUID>"
        "contractSpecId": "<Record Loan Contract Spec ID>",
        "contractInput": "<stringified object>", // Input Loan Package here
    }
}'
```

{% hint style="info" %}
Again, it is recommended that loan originators carefully consider which downstream services, such as Portfolio Manager and DART, they'd like to permission at this stage. It can save time and gas fees later to permission business partners like Figure Tech and/or 3rd Party Validators during the initial onboard.
{% endhint %}

During this process the API will automatically estimate gas fees associated with this transaction. For more information, see the [Gas and Fees](../../../../blockchain/basics/gas-and-fees.md) documentation.
