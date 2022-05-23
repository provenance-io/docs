# p8e Endpoints

## Generate Scope Transaction

Used to generate a Provenance Blockchain scope transaction message without submitting it.

**URL**: `https://{host}/api/v1/p8e/tx/generate`

**Method**: POST

**Request Body**:

```
{
   "account": {
     "keyRingIndex": "0",
     "keyIndex": "0",
     "partyType": "OWNER",
   },
   "permissions": {
     "audiences": [],
     "permissionDart": false,
     "permissionPortfolioManager": false
   },
   "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
   "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
   "scopeId": <scope uuid>,
   "contractInput": <string of contract input>
}
```

| Field                                  | Description                                                                                                                                          | Data Type                     |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| account/keyRingIndex                   | The key ring index. Used to identify the provenance account.                                                                                         | String                        |
| account/keyIndex                       | The key index. Used to identify the provenance account.                                                                                              | String                        |
| permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS                                                        | List\<Base64EncodedPublicKey> |
| permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.                                                                      | Bool                          |
| permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.                                                         | Bool                          |
| contractSpecId                         | The contract specification id. denote the Contracts/Processes that will be used to manage the data within a scope.                                   | String                        |
| scopeSpecId                            | The scope specification id. indicates a set of allowed Contract Specifications that are allowed to be used against a given scope to perform updates. | String                        |
| scopeId                                | The scope id that defines the set of records                                                                                                         | String                        |
| contractInput                          | The string representation of the input to the contract to run                                                                                        | String                        |
| account/isTestNet                      | If true, testnet shall be used, otherwise mainnet                                                                                                    | Bool                          |

**Response**

<details>

<summary><strong>Sample Response</strong></summary>

```
{
    "json": {
        "messages": [
            {
                "@type": "/provenance.metadata.v1.MsgWriteScopeRequest",
                "scope": {
                    "scopeId": "AHEfrPPaWTcghQJo34ZE0gU=",
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
                "scopeUuid": "711facf3-da59-3720-8502-68df8644d205",
                "specUuid": "551b5eca-921d-4ba7-aded-3966b224f44b"
            },
            {
                "@type": "/provenance.metadata.v1.MsgWriteSessionRequest",
                "session": {
                    "sessionId": "AXEfrPPaWTcghQJo34ZE0gV8DiP9meZGe4NmWke85QBp",
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
                    "scopeUuid": "711facf3-da59-3720-8502-68df8644d205",
                    "sessionUuid": "7c0e23fd-99e6-467b-8366-5a47bce50069"
                }
            },
            {
                "@type": "/provenance.metadata.v1.MsgWriteRecordRequest",
                "record": {
                    "name": "Asset",
                    "sessionId": "AXEfrPPaWTcghQJo34ZE0gV8DiP9meZGe4NmWke85QBp",
                    "process": {
                        "hash": "32D60974A2B2E9A9D9E93D9956E3A7D2BD226E1511D64D1EA39F86CBED62CE78",
                        "name": "OnboardAssetProcess",
                        "method": "OnboardAsset"
                    },
                    "inputs": [
                        {
                            "name": "AssetHash",
                            "hash": "u8wa+g8aogt0Z1cIAYnoT4Y/ahEv+OBylpuzrYLO1Hc=",
                            "typeName": "String",
                            "status": "RECORD_INPUT_STATUS_PROPOSED"
                        }
                    ],
                    "outputs": [
                        {
                            "hash": "u8wa+g8aogt0Z1cIAYnoT4Y/ahEv+OBylpuzrYLO1Hc=",
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
        "CiwvcHJvdmVuYW5jZS5tZXRhZGF0YS52MS5Nc2dXcml0ZVNjb3BlUmVxdWVzdBKlAgqrAQoRAHEfrPPaWTcghQJo34ZE0gUSEQRVG17Kkh1Lp63tOWayJPRLGi0KKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05EAUiKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05Kil0cDE1NnB2YzhmMzM4NTA0dXM1NW53eHVqdDI5Zjc5NDhtY2d6OHhtORIpdHAxNTZwdmM4ZjMzODUwNHVzNTVud3h1anQyOWY3OTQ4bWNnejh4bTkaJDcxMWZhY2YzLWRhNTktMzcyMC04NTAyLTY4ZGY4NjQ0ZDIwNSIkNTUxYjVlY2EtOTIxZC00YmE3LWFkZWQtMzk2NmIyMjRmNDRi",
        "Ci4vcHJvdmVuYW5jZS5tZXRhZGF0YS52MS5Nc2dXcml0ZVNlc3Npb25SZXF1ZXN0EroCCr4BCiEBcR+s89pZNyCFAmjfhkTSBXwOI/2Z5kZ7g2ZaR7zlAGkSEQP5fsxdxYBHjb4CbBsMMiNfGi0KKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05EAWaBlYSKXRwMTU2cHZjOGYzMzg1MDR1czU1bnd4dWp0MjlmNzk0OG1jZ3o4eG05Iil0cDE1NnB2YzhmMzM4NTA0dXM1NW53eHVqdDI5Zjc5NDhtY2d6OHhtORIpdHAxNTZwdmM4ZjMzODUwNHVzNTVud3h1anQyOWY3OTQ4bWNnejh4bTkaTAokNzExZmFjZjMtZGE1OS0zNzIwLTg1MDItNjhkZjg2NDRkMjA1GiQ3YzBlMjNmZC05OWU2LTQ2N2ItODM2Ni01YTQ3YmNlNTAwNjk=",
        "Ci0vcHJvdmVuYW5jZS5tZXRhZGF0YS52MS5Nc2dXcml0ZVJlY29yZFJlcXVlc3QS/wIKqwIKBUFzc2V0EiEBcR+s89pZNyCFAmjfhkTSBXwOI/2Z5kZ7g2ZaR7zlAGkaZRJAMzJENjA5NzRBMkIyRTlBOUQ5RTkzRDk5NTZFM0E3RDJCRDIyNkUxNTExRDY0RDFFQTM5Rjg2Q0JFRDYyQ0U3OBoTT25ib2FyZEFzc2V0UHJvY2VzcyIMT25ib2FyZEFzc2V0IkMKCUFzc2V0SGFzaBosdTh3YStnOGFvZ3QwWjFjSUFZbm9UNFkvYWhFditPQnlscHV6cllMTzFIYz0iBlN0cmluZygBKjAKLHU4d2ErZzhhb2d0MFoxY0lBWW5vVDRZL2FoRXYrT0J5bHB1enJZTE8xSGM9EAEyIQX5fsxdxYBHjb4CbBsMMiNf1ZOG4K5DXikvvg6825VLdRIpdHAxNTZwdmM4ZjMzODUwNHVzNTVud3h1anQyOWY3OTQ4bWNnejh4bTkiJGY5N2VjYzVkLWM1ODAtNDc4ZC1iZTAyLTZjMWIwYzMyMjM1Zg=="
    ]
}
```

</details>

| Field  | Description                                                                                             | Data Type   |
| ------ | ------------------------------------------------------------------------------------------------------- | ----------- |
| json   | JSON object containing the fully formed set of messages to be sent to Provenance for contract execution | JSON Object |
| base64 | Base 64 encoded versions of those same messages provided as JSON above                                  | String      |

## Execute Transaction on Provenance

Used to send a transaction proposal message to be executed by the Provenance Blockchain network.

**URL**: `https://{host}/api/v1/p8e/tx/execute`

**Method**: POST

**Request Body:**

```
{
   "chainId": "pio-testnet-1",
   "nodeEndpoint": "grpc://192.168.1.242:9090",
   "tx": <tx body>,
   "account": {
      "keyRingIndex": "0",
      "keyIndex": "0",
      "partyType": "OWNER",
    },
}
```

| Field                | Description                                                  | Data Type |
| -------------------- | ------------------------------------------------------------ | --------- |
| chainId              | The blockchain identifier                                    | String    |
| nodeEndpoint         | The url to the provenance node to run against                | String    |
| tx                   | The tx body that should be broadcast to provenance           | String    |
| account/keyRingIndex | The key ring index. Used to identify the provenance account. | String    |
| account/keyIndex     | The key index. Used to identify the provenance account.      | String    |
| account/isTestNet    | If true, testnet shall be used, otherwise mainnet            | Bool      |

**Response**

```
{
    "hash": <result of the contract execution provided as a hash>,
    "gasWanted": <nhash value>,
    "gasUsed": <nhash value>,
    "height": <block height>
}
```

| Field     | Description                                                       | Data Type |
| --------- | ----------------------------------------------------------------- | --------- |
| hash      | The returned hash of the record stored on the Provenance ledger   | String    |
| gasWanted | The gas estimated and supplied for contract execution             | String    |
| gasUsed   | The gas used during contract execution                            | String    |
| height    | The block height when the transaction was committed to the ledger | String    |
