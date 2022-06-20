---
description: Execute any p8e contract by name
---

# Execute Contract

### Description

Used to execute a p8e contract by providing the contract configuration and inputs to the contract.

For single-party contracts, the transaction is automatically submitted to the Provenance Blockchain node for memorialization.

For multi-party contracts, the envelope state is returned while contract execution passes to the next participant via the Object Store's mailbox feature.

{% hint style="info" %}
See [API Key](../../#api-key-for-test-or-production-environments) and [Permissioning Others](../../../p8e-contract-execution-environment-p8e/key-management/permissioning-others.md) sections for more detail on which keys are used for encryption.
{% endhint %}

### Usage

**URL**: `https://{host}/api/v1/cee/execute`

**Method**: POST

**Headers**:

{% tabs %}
{% tab title="Local" %}
{% hint style="info" %}
Supply one `x-uuid` header when running locally.
{% endhint %}

| Key          | Value                     |
| ------------ | ------------------------- |
| Content-Type | application/json          |
| x-uuid       | \<Provenance Member UUID> |
{% endtab %}

{% tab title="Test/Production" %}
{% hint style="info" %}
Supply one `apikey` header when running in test or production environments..
{% endhint %}

| Key          | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |
| apikey       | \<API Key>       |
{% endtab %}
{% endtabs %}

**Request Body**:

```
{
    "scope": {
        "scopeUuid": "",
        "sessionUuid": ""
    },
    "config": {
        "account": {
            "partyType": ""
        },
        "client": {
            "objectStoreUrl": ""
        },
        "contract": {
            "contractName": "",
            "scopeSpecificationName": "",
            "parserConfig": {
                "name": "",
                "descriptors": [
                    ""
                ]
            }
        },
        "provenanceConfig": {
            "chainId": "",
            "nodeEndpoint": "",
            "gasAdjustment": 1.5
        }
    },
    "participants": [
        {
            "uuid": "",
            "partyType": ""
        }
    ],
    "permissions": {
        "audiences": [],
        "permissionDart": true,
        "permissionPortfolioManager": true
    },
    "records": {
        ... Record key-value pairs correlated to contract inputs ...
    }
}
```

| Field                           | Description                                                                                                                                            | Data Type     |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| account/partyType               | Party type of the provenance member invoking the contract. This type should be associated with the UUID (or API Key) passed in the header.             | String        |
| client/objectStoreUrl           | URL to the encrypted object store to run against.                                                                                                      | String        |
| contract/contractName           | Fully qualified name of the p8e contract you wish to invoke.                                                                                           | String        |
| scope/scopeUuid                 | Unique identifier for the scope being created/updated by the p8e contract.                                                                             | String (UUID) |
| contract/scopeSpecificationName | Fully qualified name of the scope specification.                                                                                                       | String        |
| contract/parserConfig           | Optional configuration for parsing Any types used as inputs to contracts.                                                                              | Object        |
| contract/parseConfig/name       | Fully qualified name of the parser                                                                                                                     | String        |
| contract/parseConfig/desriptors | List of fully qualified Protocol Buffer message names used as Any types in the inputs to contracts.                                                    | List\<String> |
| provenanceConfig/chainId        | Unique identifier (name) of the Provenance Blockchain network.                                                                                         | String        |
| provenanceConfig/nodeEndpoint   | URL to the Provenance Blockchain node where the transaction will be submitted.                                                                         | String        |
| provenanceConfig/gasAdjustment  | Multiplier applied to estimated gas prior to submitting the transaction.                                                                               | Double        |
| participants/uuid               | Provenance Member ID for any additional contract participants.                                                                                         | String (UUID) |
| participants/partyType          | Party type of the additional provenance member participating in the contract execution.                                                                | String        |
| permissions                     | Object containing PermissionInfo. (See [Permissioning Others](../../../p8e-contract-execution-environment-p8e/key-management/permissioning-others.md)) | JSON          |
| records                         | Object containing the inputs to the p8e contract as key-value pairs                                                                                    | JSON          |
| scope/sessionUuid               | The session uuid. Optional.                                                                                                                            | String (UUID) |

**Response Status Codes**:

| Code | Meaning                         |
| ---- | ------------------------------- |
| 200  | Transaction executed and stored |
| 400  | Illegal arguments               |
| 500  | Server error                    |

**Response Body**:

{% tabs %}
{% tab title="Single-Party" %}
```json
{
    "multiparty": false,
    "metadata": {
        "hash": "string",
        "gasWanted": "string",
        "gasUsed": "string",
        "height": "string"
    }
}
```

| Field      | Description                                                                         | Data Type |
| ---------- | ----------------------------------------------------------------------------------- | --------- |
| multiparty | <p>Single-party vs. multi-party flag<br>Always false for single-party responses</p> | Bool      |
| hash       | The returned hash of the transaction                                                | String    |
| gasWanted  | Amount of hash sent by the invoker of the transaction (denominated in nHash)        | String    |
| gasUsed    | Amount of hash used as gas to record the transaction (denominated in nHash)         | String    |
| height     | Block height where the transaction executed                                         | String    |
{% endtab %}

{% tab title="Multi-Party" %}
```json
{
    "multiparty": true,
    "envelopeState": "string"
}
```



| Field         | Description                                                                       | Data Type          |
| ------------- | --------------------------------------------------------------------------------- | ------------------ |
| multiparty    | <p>Single-party vs. multi-party flag<br>Always true for multi-party responses</p> | Bool               |
| envelopeState | Current state of the contract execution                                           | String (ByteArray) |
{% endtab %}
{% endtabs %}

### Examples

<details>

<summary>Initialize Loan Scope</summary>

#### Sample Request

The following example executes the [InitializeLoanScope](https://github.com/provenance-io/loan-package-contracts/blob/main/contract/src/main/kotlin/io/provenance/scope/loan/contracts/InitializeLoanScope.kt) contract. This is a [single party contract](https://github.com/provenance-io/loan-package-contracts/blob/main/contract/src/main/kotlin/io/provenance/scope/loan/contracts/InitializeLoanScope.kt#L36), so no participants list is necessary.

Note: this contract is very basic and meant as an example. It allows invokers to initialize a loan with any combination of the six records that make up a loan scope, and overwrites any data that was already present. This contract will be phased out in favor of more specific loan life cycle contracts.

```shell
curl --location \
--request POST 'localhost:8080/p8e-cee-api/external/api/v1/cee/execute' \
--header 'x-uuid: deadbeef-face-479b-860c-facefaceface' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "client": {
            "objectStoreUrl": "grpc://localhost:5001"
        },
        "contract": {
            "contractName": "io.provenance.scope.loan.contracts.InitializeLoanScope",
            "scopeUuid": "91888240-669f-460c-917d-448302e93f2b",
            "scopeSpecificationName": "io.provenance.scope.loan.LoanScopeSpecification",
            "parserConfig": {
                "name": "io.provenance.api.frameworks.cee.parsers.JsonMessageParser",
                "descriptors": [
                    "tech.figure.loan.v1beta1.MISMOLoanMetadata"
                ]
            }
        },
        "provenanceConfig": {
            "chainId": "chain-local",
            "nodeEndpoint": "grpc://localhost:9090"
        }
    },
    "permissions": {
        "permissionDart": true,
        "permissionPortfolioManager": true
    },
    "records": {
        "asset": {
            "id": {
                "value": "91888240-669f-460c-917d-448302e93f2b"
            },
            "type": "LOAN",
            "description": "30 year fixed rate mortgage",
            "kv": {
                "mismoLoan": {
                    "@type": "type.googleapis.com/tech.figure.loan.v1beta1.MISMOLoanMetadata",
                    "uli": "TEST-ULI-123456789123456789",
                    "document": {
                        "id": {
                            "value": "1774edb8-c7a7-4fd7-9e8e-33d4e6bd5185"
                        },
                        "uri": "object://localhost:8080/pRLi9H6w9m/yovnODAS4RUKO90Xnjmi8d26K4KYROTg=",
                        "fileName": "MISMO v3.4 - 91888240-669f-460c-917d-448302e93f2b",
                        "contentType": "application/xml",
                        "documentType": "MISMO v3.4",
                        "checksum": {
                            "checksum": "pRLi9H6w9m/yovnODAS4RUKO90Xnjmi8d26K4KYROTg=",
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
                        "value": "38557668-762f-429e-a26d-b685618d7037"
                    },
                    "uri": "object://localhost:8080/71S848TExiNm6FoeZN0MqtBbChjjUWUjOtir9chXxo4=",
                    "fileName": "sample2.pdf",
                    "contentType": "application/pdf",
                    "documentType": "Sampel PDF",
                    "checksum": {
                        "checksum": "71S848TExiNm6FoeZN0MqtBbChjjUWUjOtir9chXxo4=",
                        "algorithm": "sha512"
                    }
                }
            ]
        },
        "eNote": {
            "controller": {
                "controllerUuid": {
                    "value": "7bc65bd0-3441-4f8e-9c5d-893a438c396b"
                },
                "controllerName": "Controller One"
            },
            "eNote": {
                "id": {
                    "value": "b923d6fb-ae73-4ace-8831-433512e83269"
                },
                "uri": "object://localhost:8080/gE8i+JHSwPss2y6sEs41m2I1o6M+NZaPeSTEudtqosw=",
                "fileName": "sample.pdf",
                "contentType": "application/xml",
                "documentType": "MISMO SMARTDoc v1.02 XML - eNote",
                "checksum": {
                    "checksum": "gE8i+JHSwPss2y6sEs41m2I1o6M+NZaPeSTEudtqosw=",
                    "algorithm": "SHA-256"
                }
            },
            "signedDate": {
                "value": "05/22/2022"
            },
            "vaultName": "Figure eVault"
        },
        "servicingData": {
            "loanId": {
                "value": "91888240-669f-460c-917d-448302e93f2b"
            },
            "assetType": {
                "supertype": "MORTGAGE",
                "subtype": "FIXED"
            },
            "currentBorrowerInfo": {
                "primary": {
                    "id": {
                        "value": "0096f2d9-8d85-4956-8a32-f0df7ab9bacd"
                    },
                    "partyType": "PRIMARY_BORROWER",
                    "name": {
                        "firstName": "Jon",
                        "lastName": "Snow"
                    },
                    "dob": {
                        "value": "01/01/2000"
                    },
                    "phoneNumbers": [
                        {
                            "number": "1-800-JON-SNOW",
                            "numberType": "HOME"
                        }
                    ],
                    "addresses": [
                        {
                            "street": "1 Main St",
                            "street2": "",
                            "street3": "",
                            "city": "Winterfell",
                            "state": "North",
                            "country": "Seven Kingdoms",
                            "zip": "00001",
                            "unitNumber": "",
                            "addressType": "",
                            "ownershipType": ""
                        }
                    ],
                    "ssn": "000-00-0000",
                    "email": "jon@snow.io",
                    "citizenship": "",
                    "maritalStatus": "SINGLE",
                    "isSelfEmployed": false
                }
            },
            "originalNoteAmount": {
                "amount": 250000.00,
                "currency": "USD"
            },
            "loanState": [
                {
                    "id": {
                        "value": "65baf01d-ee8b-4ad2-b646-cffe26640df3"
                    },
                    "effectiveTime": "05-22-2022T00:00:00.00Z",
                    "uri": "object://localhost:8080/o0iERVQp7v7WdSkq8dlREr+yP8kEuAHvBMmprazYr0k=",
                    "checksum": {
                        "checksum": "o0iERVQp7v7WdSkq8dlREr+yP8kEuAHvBMmprazYr0k=",
                        "algorithm": "SHA-256"
                    }
                }
            ]
        },
        "servicingRights": {
            "servicerId": {
                "value": "7bc65bd0-3441-4f8e-9c5d-893a438c396b"
            },
            "servicerName": "Servicer One"
        }
    }
}'
```

#### Sample Response

```json
{
    "multiparty": false,
    "metadata": {
        "hash": "1F562E27086927577D0A42985C400D97A824BC2064338564E003DCC77C941993",
        "gasWanted": "378982",
        "gasUsed": "266065",
        "height": "15640"
    }
}
```

</details>

