---
description: Wait, that's an NFT?
---

# Data Modeling

NFT stands for “Non-Fungible Token” and is used in blockchain ecosystems to represent a unique (non-fungible) digital asset (token) whose ownership is registered and tracked on a blockchain.

Provenance NFTs are typically financial assets, such as loans or funds. However, the type of asset on Provenance is not restricted, allowing for innovation in the financial services industry. The [metadata-asset-model](https://github.com/provenance-io/metadata-asset-model) provides a highly extensible [Google Protocol Buffer](https://developers.google.com/protocol-buffers) representation of an [Asset](https://github.com/provenance-io/metadata-asset-model/blob/main/docs/asset.md) to contain whatever data model the asset originator wishes to use.

## Facts and Scopes

{% hint style="info" %}
This section builds on the [Metadata Module](https://docs.provenance.io/modules/metadata-module) documentation. Revisit that page for definitions of the four core state objects in p8e: Contracts, Records, Sessions, and Scopes.
{% endhint %}

Asset originators must understand the structure of the scope they are going to onboard to Provenance Blockchain, also known as the Record Specification. We will stick to the mortgage as an example of an asset that we want to represent as an asset in Provenance. The loan Record Specification is one scope that includes the following facts:

| Fact             | Description                                                                                                           | Nullable                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Asset            | Core information about the loan that should not change throughout the life cycle of the loan after the loan is funded | No                                                       |
| Servicing Rights | Identifies the servicer                                                                                               | No                                                       |
| Documents        | List of loan document metadata with pointers to their location                                                        | Yes                                                      |
| Loan States      | List of loan states from servicing                                                                                    | Yes                                                      |
| Validation       | List of validation requests and validation results                                                                    | Yes                                                      |
| eNote            | Metadata belonging to the authoritative copy of the eNote, as well as identifiers for the controller and custodian    | Yes (only used when eNote is to be registered with DART) |

{% hint style="info" %}
Open source Scope Specification can be found [here](https://github.com/provenance-io/asset-specifications).
{% endhint %}

## Data Format

Each [Fact](../../../p8e/overview/#facts) in a [Scope](../../../p8e/overview/#scopes) is a key-value pair, where the key is a String and the value is a Protocol Buffer object. [Google Protocol Buffers](https://developers.google.com/protocol-buffers) support code generation in many languages. The table below lists the fact names and links to the Protocol Buffer definitions and documentation for each proto used in the loan scope.&#x20;

| Fact Name        | Proto Definition                                                                                                                                                                     | Proto Documentation                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| Asset            | [asset.proto](https://github.com/provenance-io/metadata-asset-model/blob/main/src/main/proto/tech/figure/asset/v1beta1/asset.proto#L19)                                              | [asset.md](https://github.com/provenance-io/metadata-asset-model/blob/main/docs/asset.md)         |
| Servicing Rights | [servicing\_rights.proto](https://github.com/provenance-io/metadata-asset-model/blob/dkneisly/loan-wrapper/src/main/proto/tech/figure/servicing/v1beta1/servicing\_rights.proto#L12) | [servicing.md](https://github.com/provenance-io/metadata-asset-model/blob/main/docs/servicing.md) |
| Documents        | [document.proto](https://github.com/provenance-io/metadata-asset-model/blob/main/src/main/proto/tech/figure/util/v1beta1/document.proto#L27)                                         | [util.md](https://github.com/provenance-io/metadata-asset-model/blob/main/docs/util.md)           |
| Loan States      | [loan\_state.proto](https://github.com/provenance-io/metadata-asset-model/blob/main/src/main/proto/tech/figure/servicing/v1beta1/loan\_state.proto#L23)                              | [servicing.md](https://github.com/provenance-io/metadata-asset-model/blob/main/docs/servicing.md) |
| Validation       | [validation.proto](https://github.com/provenance-io/metadata-asset-model/blob/dkneisly/loan-wrapper/src/main/proto/tech/figure/validation/v1beta1/validation.proto#L13)              | validation.md \[TODO]                                                                             |
| eNote            | [registry.proto](https://github.com/provenance-io/metadata-asset-model/blob/dkneisly/loan-wrapper/src/main/proto/io/dartinc/registry/v1beta1/registry.proto#L14)                     | registry.md \[TODO]                                                                               |

## Example

{% hint style="info" %}
Note: This example is subject to rapid change.
{% endhint %}

An example loan with eNote populated:

```kotlin
{
    "asset" : {
        "id": "c6978d46-3c3e-4175-a0d2-8f8ce47e8bb6",
        "type": "LOAN",
        "description": "MORTGAGE LOAN-1234",
        "kv": {
            "loan": {
                "typeUrl": "/tech.figure.asset.loan.Loan",
                "id": "c6978d46-3c3e-4175-a0d2-8f8ce47e8bb6",
                "originatorName": "Figure Lending",
                "originatorLoanId": "LOAN-1234",
                "borrowers": {
                    "primary": {
                        "partyType": "PRIMARY_BORROWER",
                        "name": {
                            "firstName": "FirstName",
                            "lastName": "LastName",
                            "middleName": "MiddleName",
                            "suffix": "NameSuffix"
                        }
                    }
                },
                "loanType": "MORTGAGE",
                "terms": {
                    "principalAmount": {
                        "amount": 1000000,
                        "currency": "USD"
                    },
                    "termInMonths": "360",
                    "rateType": "FIXED",
                    "interestRate": {
                        "value": "0.045"
                    },
                    "interestRateCap": {
                        "value": "0.045"
                    },
                    "payment": {
                        "firstPaymentAmount": {
                            "amount": 3000,
                            "currency": "USD"
                        },
                        "monthlyPaymentAmount": {
                            "amount": 3000,
                            "currency": "USD"
                        }
                    },
                    "dates": {
                        "initialOfferDate": {
                            "value": "2022-02-01"
                        },
                        "originationDate": {
                            "value": "2022-02-07"
                        },
                        "signedDate": {
                            "value": "2022-03-01"
                        },
                        "fundingDate": {
                            "value": "2022-03-06"
                        }
                    }
                },
                "funding": {
                    "status": "FUNDED",
                    "started": "2022-03-05T11:30:15.01Z",
                    "completed": "2022-03-06T04:30:15.01Z",
                    "disbursements": {
                        "id": {
                            "value": "<UUID>"
                        },
                        "amount": {
                            "amount": "1000000",
                            "currency": "USD"
                        },
                        "account": {
                            ...
                        },
                        "status": "COMPLETED",
                        "started": "2022-03-05T11:30:15.01Z",
                        "completed": "2022-03-06T04:30:15.01Z"
                    }
                },
                "mortgage": {
                    "lienProperty": {
                        "address": {
                            "street": "123 Main St",
                            "city": "City",
                            "state": "FL",
                            "zip": "33000"
                        }
                    },
                    "lienPosition": 1,
                    ...
                }
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
            "uri": "EOS URI",
            "fileName": "Electronic Promissory Note (eNote)",
            "ContentType": "application/xml",
            "documentType": "MISMO_ENOTE_SMART_DOC_XML",
            "checksum": "<File sha512 Hash>"
        },
        ...
    ],
    "loanStates": null,
    "validation": null,
    "eNote": {
        "controllerId": {
            "controllerUuid": {
                "value": "<Controller ID>"
            },
            "controllerName": "<Controller Name>"
        },
        "eNote": {
            "id": "<UUID>",
            "uri": "EOS URI",
            "fileName": "Electronic Promissory Note (eNote)",
            "ContentType": "application/xml",
            "documentType": "MISMO_ENOTE_SMART_DOC_XML",
            "checksum": "<File sha512 Hash>"
        },
        "signedDate": {
            "value": "2022-03-01"
        },
        "valueName": "DART eVault"
    }
}

```

