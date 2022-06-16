---
description: How to record a loan
---

# Loan Onboarding

{% hint style="info" %}
The intended audience for this page is a Loan Originator who is operating their own Loan Origination System (LOS) or their LOS provider, but a similar process could be used for any asset being onboarded to Provenance.
{% endhint %}

As a borrower moves throughout the application stage of the mortgage process, data and documents are collected by the loan originator in their LOS. Once the application reaches an appropriate milestone, such as closing, the loan originator can start onboarding the loan to Provenance.

### Onboarding Loan Documents

Recall that in the [Loan Package data model](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/data-mapping), loan document metadata is stored separately from the loan application and underwriting data - in a separate Record. Also, rather than store every byte of every loan document within the loan scope, the `documents` Record will contain a list of loan document metadata. Each entry contains the URI to the actual object and a checksum of the contents to provide evidence of tampering with that off-chain object.

Technically speaking, those documents could be stored anywhere, however, the Encrypted Object Store is a great way to store documents such that they can be shared with business partners and downstream applications. Both DART and Portfolio Manager are built to look for documents in the Encrypted Object Store. Storing documents as encrypted objects in the Objects Store allows for automated replication and less integration work between business partners.

Therefore, step one of the loan onboarding process is to start inserting documents into the EOS as they become available. The [Store File](../api-specification/object-store-endpoints/store-file.md) endpoint handles encrypting and storing individual files in the object store without memorializing them as scopes on Provenance. The curl command below provides an example.

```
curl --location \
--request POST '<host>/api/v1/eos/file' \
--header 'apikey: <api_key>' \
--form 'objectStoreAddress="<object_store_address"' \
--form 'id="foo"' \
--form 'file=@"<path_to_file>"' \
--form 'permissions="{\"permissionDart\":true,\"permissionPortfolioManager\":true}"'
```

{% hint style="info" %}
If you are working with Figure Tech to integrate, they can provide an appropriate Host, API Key, and Object Store Address.
{% endhint %}

It's important to consider whether or not to permission certain other participants at this stage. Figure Tech recommends permissioning the following:

* **Portfolio Manager -** Permission if you intend on accessing, pledging, or selling this loan on Portfolio Manager.&#x20;
* **DART** - Permission if you intend on registering this loan and the associated eNote with DART, which will automatically track life of loan updates.
* **3rd Party Validators** - Permission to 3rd party validators if you intend on requesting validation services for this loan.
* **3rd Party Servicers** - Permission to 3rd party servicers if you intend on allowing a Provenance Blockchain-enabled servicer to read loan data and post loan states for this loan.

{% hint style="info" %}
It is important for integrators to keep track of the metadata returned by the Object Store when it creates a document. They will need to generate a list of documents when it comes time to create the loan scope in the next step.
{% endhint %}

{% hint style="info" %}
Figure Tech is actively integrating with 3rd Party document preparation vendors to receive certain documents, such as the eNote, directly.
{% endhint %}

### Onboarding the Loan Package

Once documents are stored in EOS, and the loan application process is in a stage where it is worth onboarding the loan data to Provenance, the loan originator should hit the [Execute Contract](../api-specification/contract-execution-endpoints/execute-contract.md) endpoint to execute the [Record Loan](https://github.com/provenance-io/loan-package-contracts/blob/main/contract/src/main/kotlin/io/provenance/scope/loan/contracts/RecordLoanContract.kt) p8e contract.

{% hint style="info" %}
Ultimately the stage at which an originator onboards a loan to Provenance is an internal business decision. A suggestion would be to onboard the loan either at the point in which you would no longer want to update the base loan data, or at the point you would want to share one common set of loan application data with a 3rd party, such as a validator, servicer, warehouse lender, or investor.
{% endhint %}

The Execute Contract endpoint requires the consumer to specify:

* the party type invoking the contract (`OWNER`),
* the object store where records will be stored,
* contract configuration details such as the contract name and new scope ID,
* provenance network configuration such as the network ID, a single node endpoint to submit the transaction, and a gas adjustment,
* a list of additional participants involved in signing the transaction (none in this case),
* a list of additional audiences that should be allowed to retrieve and decrypt records associated with the scope created or updated through the contract execution, and
* inputs to the contract.

Here is where the loan originator has some flexibility in regards to how they want to represent the loan on Provenance. As mentioned in [Data Mapping](../../lending-ecosystem/data-mapping.md), you can either map the loan data from your LOS to the predefined Loan proto provided in the metadata-asset-model library, or encode the loan data from your LOS into one of the MISMO Reference Model XML formats.

{% hint style="info" %}
Version 3.4 of the MISMO Reference Model is recommended.
{% endhint %}

Depending on what stage of the loan application process, you may be able to fill in one or more of the Loan Package Records. At a minimum, you are expected to provide:

* **The** `asset` **Record** - Including either the Loan proto or MISMOLoan proto
* **The** `servicingRights` **Record** - Naming the servicer and sub-servicer, if applicable
* **The** `documents` **Record** - List of known documents (created in step one)

As a best practice, include as many Records as are available. That translates to including:

* **The** `servicingData` **Record** - Include the initial loan state if you want the loan to appear in Portfolio Manager (Portfolio Manager uses this Record to determine the remaining value of any loan)
* **The** `validation` **Record** - If you already plan to request validation from a 3rd party service provider, include a `ValidationRequest` proto in the `validation` Record
* **The** `eNote` **Record** - if you have stored an eNote in either the DART eVault or an external eVault, then you can specify the `eNote` Record

{% hint style="info" %}
If you use a 3rd party document services provider that is directly integrated with Provenance to generate and send eNotes, then the `eNote` Record may already be populated for you. Simply omit that record in the input object - it cannot be overwritten by executing the Record Loan contract. This is designed to prevent mistakes.
{% endhint %}

An example of a set of records using a MISMO XML to represent the loan data is provided below. In this example, the loan originator is providing the asset, document list, and servicing rights. They are leaving out the eNote, servicing data, and validation records because they are not requesting validation at this time and are using a 3rd party document servicer provider that will send the eNote and initial servicing data to Provenance separately. The p8e contract will automatically combine this loan data with an eNote once both are onboarded.

Since this example uses a MISMO XML to store loan data, the originator should repeat step one to onboard that file as an object in the object store prior to invoking the Record Loan contract.

<details>

<summary>Sample Input Records for Record Loan Contract</summary>

```
{
    "asset": {
        "id": {
            "value": "<UUID of Loan - Preferably ULI converted to UUID>"
        },
        "type": "LOAN",
        "description": "<Description of Loan>",
        "kv": {
            "mismoLoan": {
                "@type": "type.googleapis.com/tech.figure.loan.v1beta1.MISMOLoanMetadata",
                "uli": "<ULI>",
                "document": {
                    "id": {
                        "value": "<UUID of Document>"
                    },
                    "uri": "<EOS URI>",
                    "fileName": "<Document Name>",
                    "contentType": "application/xml",
                    "documentType": "<Document Type>",
                    "checksum": {
                        "checksum": "<sha512 Hash of File>",
                        "algorithm": "sha512"
                    }
                }
            }
        }
    },
    "documents": {
        "document": [
            {
                "id": {
                    "value": "<UUID of Document>"
                },
                "uri": "<EOS URI>",
                "fileName": "<Document Name>",
                "contentType": "application/pdf",
                "documentType": "<Document Type>",
                "checksum": {
                    "checksum": "<sha512 Hash of File>",
                    "algorithm": "sha512"
                }
            }
        ]
    },
    "servicingRights": {
        "servicerId": {
            "value": "<UUID of Servicer>"
        },
        "servicerName": "<Servicer Name>"
    }
}
```

</details>

The curl command below will onboard the loan data to the Encrypted Object Store by executing the `Record Loan` p8e contract, and broadcast a fully formed Provenance Blockchain transaction proposal containing the appropriate hashes of the inputs, the process (the Record Loan p8e contract), and the outputs of that contract to the Provenance Blockchain memory pool. There, it will get picked up by validators and memorialized as a scope on the Provenance Blockchain ledger.

```
curl --location --request POST 'localhost:8080/p8e-cee-api/external/api/v1/cee/execute' \
--header 'apikey: <Loan Originator's API Key>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "account": {
            "partyType": "OWNER"
        },
        "client": {
            "objectStoreUrl": "grpc://object-store-v2.p8e:80"
        },
        "contract": {
            "contractName": "io.provenance.scope.loan.contracts.RecordLoanContract",
            "scopeUuid": "<UUID of Loan or ULI converted to UUID>",
            "scopeSpecificationName": "io.provenance.scope.loan.LoanScopeSpecification",
            "parserConfig": {
                "name": "io.provenance.api.frameworks.cee.parsers.JsonMessageParser",
                "descriptors": [
                    "tech.figure.loan.v1beta1.MISMOLoanMetadata"
                ]
            }
        },
        "provenanceConfig": {
            "chainId": "pio-testnet-1", // Swap with mainnet chain ID for prod
            "nodeEndpoint": "grpc://192.168.1.242:9090", // Swap with mainnet node for prod
            "gasAdjustment": 1.5
        }
    },
    "permissions": {
        "audiences": [],
        "permissionDart": true,
        "permissionPortfolioManager": true
    },
    "records": {
        "asset": {
            "id": {
                "value": "<UUID of Loan or ULI converted to UUID>"
            },
            "type": "LOAN",
            "description": "<Description of Loan>",
            "kv": {
                "mismoLoan": {
                    "@type": "type.googleapis.com/tech.figure.loan.v1beta1.MISMOLoanMetadata",
                    "uli": "<ULI>",
                    "document": {
                        "id": {
                            "value": "<UUID of Document>"
                        },
                        "uri": "<EOS URI>",
                        "fileName": "<Document Name>",
                        "contentType": "application/xml",
                        "documentType": "<Document Type>",
                        "checksum": {
                            "checksum": "<sha512 Hash of File>",
                            "algorithm": "sha512"
                        }
                    }
                }
            }
        },
        "documents": {
            "document": [
                {
                    "id": {
                        "value": "<UUID of Document>"
                    },
                    "uri": "<EOS URI>",
                    "fileName": "<Document Name>",
                    "contentType": "application/pdf",
                    "documentType": "<Document Type>",
                    "checksum": {
                        "checksum": "<sha512 Hash of File>",
                        "algorithm": "sha512"
                    }
                }
            ]
        },
        "servicingRights": {
            "servicerId": {
                "value": "<UUID of Servicer>"
            },
            "servicerName": "<Servicer Name>"
        }
    }
}'
```

{% hint style="info" %}
Again, it is recommended that loan originators carefully consider which downstream services, such as Portfolio Manager and DART, they'd like to permission at this stage. It can save time and gas fees later to permission business partners like Figure Tech, 3rd-Party Servicers and/or 3rd-Party Validators during the initial onboard.
{% endhint %}

During this process the API will automatically estimate gas fees associated with this transaction. For more information, see the [Gas and Fees](../../../../blockchain/basics/gas-and-fees.md) documentation.
