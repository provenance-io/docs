---
description: How to onboard a loan
---

# API Usage Guide

At this point you've learned more about some key concepts, and looked under the covers at the loan package data model, p8e contract specifications, and onboarding API specifications. Whether you're operating your own p8e Contract Execution Environment or just playing with the sandbox, you're ready to onboard a loan to Provenance. This page will point you in the right direction.

{% hint style="info" %}
The intended audience for this page is a loan originator who is already operating their own Loan Origination System (LOS). It describes onboarding a mortgage, but could be loosely followed to onboard any type of loan or asset.
{% endhint %}

## Loan Onboarding

As a borrower moves throughout the application stage of the mortgage process, data and documents are collected by the loan originator in their LOS. Once the application reaches an appropriate milestone, such as closing, the loan originator can start onboarding the loan to Provenance.

### Onboard Loan Documents

Recall that in the [loan package data model](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/data-mapping), documents are stored separately from the loan application and underwriting data. For this reason, documents get inserted into the p8e Encrypted Object Store (EOS) separately from the loan package. Rather than store every byte of every loan document within the loan package scope, the scope will end up containing a list of loan document metadata, with each entry containing the URI to the actual object.

Therefore, step 1 of the loan onboarding process is to start inserting documents into the EOS as they come. The [Create Object](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-object-in-object-store) endpoint handles creating individual objects in the object store without memorializing them as scopes on Provenance. The curl command below provides an example.

```
curl --location --request POST 'http://test.figure.com/service-loan-onboarding/external/api/v1/eos' \
--header 'Content-Type: application/json' \
--data-raw '{
    "account": {
        "originatorUuid": "deadbeef-face-479b-860c-facefaceface"
    },
    "uli": "deadbeef-face-479b-860c-facefacefac9",
    "objectStoreAddress": "grpc://object-store-v2.p8e:80",
    "asset": "aGVsbG8gd29ybGQ="
}'
```

The "asset" in this case is the loan document represented as a base 64 encoded byte array. It's important to consider whether or not to permission certain other participants at this stage. Figure Tech recommends permissioning the following:

* **Portfolio Manager -** Permission if you intend on accessing, pledging, or selling this loan on Portfolio Manager.&#x20;
* **DART** - Permission if you intend on registering this loan and the associated eNote with DART, which will automatically track life of loan updates.
* **3rd Party Validators** - Permission to 3rd party validators if you intend on requesting validation services for this loan.

{% hint style="info" %}
Figure can also integrate with 3rd party document preparation vendors to receive documents directly. However, it is important for loan originators to keep track of the metadata returned by the object store when it creates a document. They will need that list when it comes time to create the loan scope.
{% endhint %}

{% hint style="info" %}
Figure Tech will provide the appropriate object store path for loan originators following the white-label approach.
{% endhint %}

### Onboard Loan Package

Once documents are stored in EOS, and the loan application process is in a stage where it is worth onboarding the loan data to Provenance, the loan originator should again hit the [Create Object](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-object-in-object-store) endpoint. This time, however, instead of a document being passed as the "asset", you will use the Loan Package protocol buffer.

{% hint style="info" %}
Ultimately the stage at which an originator onboards a loan to Provenance is a personal business decision. Remember that there is a fee to write each update to the loan scope in the form of [gas](../../../blockchain/basics/gas-and-fees.md). A suggestion would be to onboard the loan either at the point in which you would no longer want to update the base loan data, or at the point you would want to share one common set of loan application data with a 3rd party, such as a validator.
{% endhint %}

The curl command below will onboard a loan package to the EOS by executing the Record Loan p8e contract, and return a fully formed Provenance Blockchain transaction proposal containing the appropriate hashes of the inputs and outputs of that contract.

```
curl --location --request POST 'http://test.figure.com/service-loan-onboarding/external/api/v1/p8e/scope' \
--header 'x-uuid: 072a6707-b888-45d3-acc4-53b564a01bba' \
--header 'x-roles: {"service-loan-onboarding":["ROLE_ADMIN"]}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "chainId": "pio-testnet-1",
    "nodeEndpoint": "grpc://192.168.1.242:9090",
        "account": {
            "originatorUuid": "deadbeef-face-479b-860c-facefaceface"
        },
        "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
        "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
        "scopeId": "deadbeef-face-479b-860c-facefacefac9",
        "hash": "dPm9lQzaeVFIp7l1W6Km4N6KKvTMSbGiiZI+1+zJY78=",
        "uli": "deadbeef-face-479b-860c-facefacetime"
}'
```

{% hint style="info" %}
Again, it is recommended that loan originators carefully consider which downstream services, such as Portfolio Manager and DART, they'd like to permission at this stage. It can save time and gas fees later to permission business partners like Figure Tech and/or 3rd Party Validators during the initial onboard.
{% endhint %}

### Submit Transaction Proposal to the Provenance Blockchain Network

Now that the p8e scope has been created for the loan package, the loan originator will call the API one last time to send the transaction proposal to the Provenance Blockchain memory pool, where it will get picked up by validators and memorialized as a scope on the Provenance Blockchain ledger.

Taking the fully formed transaction that was returned in the previous step, loan originators can hit the [Onboard](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#onboard-on-provenance) endpoint to sign and send the transaction proposal to the memory pool. The curl command below provides and example.

```
curl --location --request POST 'http://test.figure.com/service-loan-onboarding/external/api/v1/p8e/onboard' \
--header 'x-uuid: 072a6707-b888-45d3-acc4-53b564a01bba' \
--header 'x-roles: {"service-loan-onboarding":["ROLE_ADMIN"]}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "chainId": "pio-testnet-1",
    "nodeEndpoint": "grpc://192.168.1.242:9090",
    "tx": {
        "json": {
            "messages": [
                {
                    "@type": "/provenance.metadata.v1.MsgWriteScopeRequest",
                    "scope": {
                        "scopeId": "ADGzwLpEjzEsugB+d0xUlaA=",
                        "specificationId": "BFUbXsqSHUunre05ZrIk9Es=",
                        "owners": [
                            {
                                "address": "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9",
                                "role": "PARTY_TYPE_OWNER"
                            }
                        ],
                        "dataAccess": [
                            "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9"
                        ],
                        "valueOwnerAddress": "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9"
                    },
                    "signers": [
                        "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9"
                    ],
                    "scopeUuid": "31b3c0ba-448f-312c-ba00-7e774c5495a0",
                    "specUuid": "551b5eca-921d-4ba7-aded-3966b224f44b"
                },
                {
                    "@type": "/provenance.metadata.v1.MsgWriteSessionRequest",
                    "session": {
                        "sessionId": "ATGzwLpEjzEsugB+d0xUlaAmblbDFEtHT5q8DPb3Hs3i",
                        "specificationId": "A/l+zF3FgEeNvgJsGwwyI18=",
                        "parties": [
                            {
                                "address": "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9",
                                "role": "PARTY_TYPE_OWNER"
                            }
                        ],
                        "audit": {
                            "createdBy": "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9",
                            "updatedBy": "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9"
                        }
                    },
                    "signers": [
                        "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9"
                    ],
                    "sessionIdComponents": {
                        "scopeUuid": "31b3c0ba-448f-312c-ba00-7e774c5495a0",
                        "sessionUuid": "266e56c3-144b-474f-9abc-0cf6f71ecde2"
                    }
                },
                {
                    "@type": "/provenance.metadata.v1.MsgWriteRecordRequest",
                    "record": {
                        "name": "Asset",
                        "sessionId": "ATGzwLpEjzEsugB+d0xUlaAmblbDFEtHT5q8DPb3Hs3i",
                        "process": {
                            "hash": "32D60974A2B2E9A9D9E93D9956E3A7D2BD226E1511D64D1EA39F86CBED62CE78",
                            "name": "OnboardAssetProcess",
                            "method": "OnboardAsset"
                        },
                        "inputs": [
                            {
                                "name": "AssetHash",
                                "hash": "dPm9lQzaeVFIp7l1W6Km4N6KKvTMSbGiiZI+1+zJY78=",
                                "typeName": "String",
                                "status": "RECORD_INPUT_STATUS_PROPOSED"
                            }
                        ],
                        "outputs": [
                            {
                                "hash": "dPm9lQzaeVFIp7l1W6Km4N6KKvTMSbGiiZI+1+zJY78=",
                                "status": "RESULT_STATUS_PASS"
                            }
                        ],
                        "specificationId": "Bfl+zF3FgEeNvgJsGwwyI1/Vk4bgrkNeKS++DrzblUt1"
                    },
                    "signers": [
                        "tp156pvc8f338504us55nwxujt29f7948mcgz8xm9"
                    ],
                    "contractSpecUuid": "f97ecc5d-c580-478d-be02-6c1b0c32235f"
                }
            ]
        },
        "base64": [
            "CiwvcHJvdmVuYW5jZS5tZXRhZGF0YS52MS5Nc2dXcml0ZVNjb3BlUmVxdWVzdBKlAgqrAQoRADGzwLpEjzEsugB+d0xUlaASEQRVG17Kkh1Lp63tOWayJPRLGi0KKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05EAUiKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05Kil0cDE1NnB2YzhmMzM4NTA0dXM1NW53eHVqdDI5Zjc5NDhtY2d6OHhtORIpdHAxNTZwdmM4ZjMzODUwNHVzNTVud3h1anQyOWY3OTQ4bWNnejh4bTkaJDMxYjNjMGJhLTQ0OGYtMzEyYy1iYTAwLTdlNzc0YzU0OTVhMCIkNTUxYjVlY2EtOTIxZC00YmE3LWFkZWQtMzk2NmIyMjRmNDRi",
            "Ci4vcHJvdmVuYW5jZS5tZXRhZGF0YS52MS5Nc2dXcml0ZVNlc3Npb25SZXF1ZXN0EroCCr4BCiEBMbPAukSPMSy6AH53TFSVoCZuVsMUS0dPmrwM9vcezeISEQP5fsxdxYBHjb4CbBsMMiNfGi0KKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05EAWaBlYSKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05Iil0cDE1NnB2YzhmMzM4NTA0dXM1NW53eHVqdDI5Zjc5NDhtY2d6OHhtORIpdHAxNTZwdmM4ZjMzODUwNHVzNTVud3h1anQyOWY3OTQ4bWNnejh4bTkaTAokMzFiM2MwYmEtNDQ4Zi0zMTJjLWJhMDAtN2U3NzRjNTQ5NWEwGiQyNjZlNTZjMy0xNDRiLTQ3NGYtOWFiYy0wY2Y2ZjcxZWNkZTI=",
            "Ci0vcHJvdmVuYW5jZS5tZXRhZGF0YS52MS5Nc2dXcml0ZVJlY29yZFJlcXVlc3QS/wIKqwIKBUFzc2V0EiEBMbPAukSPMSy6AH53TFSVoCZuVsMUS0dPmrwM9vcezeIaZRJAMzJENjA5NzRBMkIyRTlBOUQ5RTkzRDk5NTZFM0E3RDJCRDIyNkUxNTExRDY0RDFFQTM5Rjg2Q0JFRDYyQ0U3OBoTT25ib2FyZEFzc2V0UHJvY2VzcyIMT25ib2FyZEFzc2V0IkMKCUFzc2V0SGFzaBosZFBtOWxRemFlVkZJcDdsMVc2S200TjZLS3ZUTVNiR2lpWkkrMSt6Slk3OD0iBlN0cmluZygBKjAKLGRQbTlsUXphZVZGSXA3bDFXNkttNE42S0t2VE1TYkdpaVpJKzErekpZNzg9EAEyIQX5fsxdxYBHjb4CbBsMMiNf1ZOG4K5DXikvvg6825VLdRIpdHAxNTZwdmM4ZjMzODUwNHVzNTVud3h1anQyOWY3OTQ4bWNnejh4bTkiJGY5N2VjYzVkLWM1ODAtNDc4ZC1iZTAyLTZjMWIwYzMyMjM1Zg=="
        ]
    },
    "account": {
        "originatorUuid": "deadbeef-face-479b-860c-facefaceface"
    }
}'
```

The API will automatically estimate gas fees associated with this transaction. For more information, see the [Gas and Fees](../../../blockchain/basics/gas-and-fees.md) documentation.

{% hint style="info" %}
Executing a transaction on any blockchain network is an asynchronous process, and Provenance Blockchain is not different. There will be time between sending a transaction proposal to the memory pool and that transaction being picked up and inserted into a block that gets written to the Provenance Blockchain chain. Be sure to read on to learn how to handle common errors.
{% endhint %}

### What could go wrong?



### Why not use one endpoint for all of the above?



## Querying for Existing Loan Scope



1. Loan onboarding order of operations and what gets created/returned with each step
   1. Include images with data flow
   2. How to calculate the gas/fees
   3. How to interpret errors/what could wrong at each stage
2. Querying for data you've onboarded
3. More code
   1. Client side code to hit the API
   2. curl calls
   3. Postman collection?
4. Playground/sandbox
5. Eventually put a video up
