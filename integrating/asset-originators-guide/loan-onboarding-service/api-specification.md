---
description: Loan Onboarding Service API Spec
---

# API Specification

## Create Object in Object-Store

Used to store objects in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/eos`

**Method**: POST

**Request Body**:

```
{
  "assetId": <uuid of asset>
  "asset": <Base 64 encoded byte array of file>
  "objectStoreAddress": "grpc://object-store-v2.p8e:80",
  "permissions": {
    "audiences": [],
    "permissionDart": true,
    "permissionPortfolioManager": true,
  },
  "account": {
    "originatorUuid": <uuid>,
    "keyRingIndex": "0",
    "keyIndex": "0",
    "isTestNet": true,
  },
}
```

| Field                                  | Description                                                                                    | Data Type                     |
| -------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------- |
| assetId                                | A unique UUID that identifies the asset                                                        | String                        |
| asset                                  | The asset that is stored against the EOS. This can be a file, loan package, xml, etc           | Base 64 Encoded Byte Array    |
| objectStoreAddress                     | The URL to the encrypted object store to run against                                           | String                        |
| permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS  | List\<Base64EncodedPublicKey> |
| permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.                | Bool                          |
| permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.   | Bool                          |
| account/istestNet                      | If true, testnet shall be used, otherwise mainnet                                              | Bool                          |
| account/originatorUuid                 | The originator uuid that is stored in the associated key management system                     | String                        |
| account/keyRingIndex                   | The key ring index. Used to identify the provenance account.                                   | Int                           |
| account/keyIndex                       | The key index. Used to identify the provenance account.                                        | Int                           |

**Response**:

```
{
    "hash": "iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "uri": "object://test.figure.com/iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "bucket": "/mnt/data",
    "name": "42b2f0a8-dc40-470f-8b29-6de71486d2ff"
}
```

| Field  | Description                                  | Data Type |
| ------ | -------------------------------------------- | --------- |
| hash   | The returned hash of the saved object in EOS | String    |
| uri    | The location of the saved object             | String    |
| bucket | The volume path of the saved data            | String    |
| name   | The name of the saved data                   | String    |

## Get Object in Object-Store

Used to retrieve objects in the object store. See Encrypted Object Store for additional information.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/eos`

**Method:** GET

&#x20;**Request Body:**

```
{
   "originatorUuid": <UUID>,
   "hash": <stored hash to retrieve>,
   "objectStoreAddress": "grpc://object-store-v2.p8e:80",
}
```

| Field              | Description                                                                | Data Type |
| ------------------ | -------------------------------------------------------------------------- | --------- |
| originatorUuid     | The originator uuid that is stored in the associated key management system | String    |
| hash               | The hash of the saved object in EOS                                        | String    |
| objectStoreAddress | The URL to the encrypted object store to run against                       | String    |

## Create Snapshot

Used to create a snapshot of a stored object in the object store. See Encrypted Object Store for additional information.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/eos/snapshot`

**Method:** POST

**Request Body:**

```
{
    "account": {
        "originatorUuid": <uuid>,
        "keyRingIndex": "0",
        "keyIndex": "0",
        "isTestNet": true,
    },
    "hash": <hash of stored object>,
    "objectStoreAddress": "grpc://object-store-v2.p8e:80",
    "permissions": {
      "audiences": []
      "permissionDart": true,
      "permissionPortfolioManager": true
    },
}
```

| Field                                  | Description                                                                                    | Data Type                     |
| -------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------- |
| account/originatorUuid                 | The originator uuid that is stored in the associated key management system                     | String                        |
| account/keyRingIndex                   | The key ring index. Used to identify the provenance account.                                   | Int                           |
| account/keyIndex                       | The key index. Used to identify the provenance account.                                        | Int                           |
| hash                                   | The hash of the object that is currently stored in EOS to create a snapshot against            | String                        |
| objectStoreAddress                     | The URL to the encrypted object store to run against                                           | String                        |
| permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS  | List\<Base64EncodedPublicKey> |
| permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.                | Bool                          |
| permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.   | Bool                          |
| account/isTestNet                      | If true, testnet shall be used, otherwise mainnet                                              | Bool                          |

**Response:**

```
{
    "hash": "iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "uri": "object://test.figure.com/iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "bucket": "/mnt/data",
    "name": "42b2f0a8-dc40-470f-8b29-6de71486d2ff"
}
```

| Field  | Description                                  | Data Type |
| ------ | -------------------------------------------- | --------- |
| hash   | The returned hash of the saved object in EOS | String    |
| uri    | The location of the saved object             | String    |
| bucket | The volume path of the saved data            | String    |
| name   | The name of the saved data                   | String    |

## Create Contract Specification

The Loan Onboarding Service exposes three REST API endpoints for interactions with Provenance.&#x20;

Used to write p8e contract specifications to the Object Store and Provenance. See [Specifications](https://docs.provenance.io/p8e/p8e-usage/specifications) for additional information.&#x20;

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/specifications`

**Method**: POST

**Request Body**:

```
{
    "chainId": "pio-testnet-1",
    "nodeEndpoint": "tcp://rpc-0.test.provenance.io:26657",
    "account": {
        "originatorUuid": <uuid>,
        "keyRingIndex": "0",
        "keyIndex": "0",
        "isTestNet": true,
    },
    "scopeId": "4e554cb8-56dd-48df-b3fe-71f4c5b7d2cf",
    "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
    "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f"
}
```

| Field                  | Description                                                                                                                                          | Data Type |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| chainId                | The blockchain identifier                                                                                                                            | String    |
| nodeEndpoint           | The url to the provenance node to run against                                                                                                        | String    |
| account/originatorUuid | The originator uuid that is stored in the associated key management system                                                                           | String    |
| account/keyRingIndex   | The key ring index. Used to identify the provenance account.                                                                                         | String    |
| account/keyIndex       | The key index. Used to identify the provenance account.                                                                                              | String    |
| scopeId                | The scope id that defines the set of records                                                                                                         | String    |
| scopeSpecId            | The scope specification id. indicates a set of allowed Contract Specifications that are allowed to be used against a given scope to perform updates. | String    |
| contractSpecId         | The contract specification id. denote the Contracts/Processes that will be used to manage the data within a scope.                                   | String    |
| account/isTestNet      | If true, testnet shall be used, otherwise mainnet                                                                                                    | Bool      |

## Create Tx and Onboard to Provenance

Used to create scope tx and onboard assets to Provenance.&#x20;

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/onboard`

**Method**: POST

**Request Body**:

```
{
    "chainId": "pio-testnet-1",
    "nodeEndpoint": "grpc://192.168.1.242:9090",
    "txRequest": {
        "account": {
            "originatorUuid": <uuid>,
            "keyRingIndex": "0",
            "keyIndex": "0",
            "isTestNet": true,
        },
        "permissions": {
            "audiences": [],
            "permissionDart": false,
            "permissionPortfolioManager": false
        },
        "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
        "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
        "scopeId": <scope uuid>,
        "contractInput": <sting of contract input>
    },
}
```

| Field                                            | Description                                                                                                                                          | Data Type                     |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| chainId                                          | The blockchain identifier                                                                                                                            | String                        |
| nodeEndpoint                                     | The url to the provenance node to run against                                                                                                        | String                        |
| txRequest/account/originatorUuid                 | The originator uuid that is stored in the associated key management system                                                                           | String                        |
| txRequest/account/keyRingIndex                   | The key ring index. Used to identify the provenance account.                                                                                         | String                        |
| txRequest/account/keyIndex                       | The key index. Used to identify the provenance account.                                                                                              | String                        |
| txRequest/permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS                                                        | List\<Base64EncodedPublicKey> |
| txRequest/permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.                                                                      | Bool                          |
| txRequest/permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.                                                         | Bool                          |
| txRequest/contractSpecId                         | The contract specification id. denote the Contracts/Processes that will be used to manage the data within a scope.                                   | String                        |
| txRequest/scopeSpecId                            | The scope specification id. indicates a set of allowed Contract Specifications that are allowed to be used against a given scope to perform updates. | String                        |
| txRequest/scopeId                                | The scope id that defines the set of records                                                                                                         | String                        |
| txRequest/contractInput                          | The input to the contract                                                                                                                            | String                        |
| account/isTestNet                                | If true, testnet shall be used, otherwise mainnet                                                                                                    | Bool                          |

## Create Scope Tx

Used to create scope tx.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/generatetx`

**Method**: POST

**Request Body**:

```
{
   "account": {
     "originatorUuid": <uuid>,
     "keyRingIndex": "0",
     "keyIndex": "0",
     "isTestNet": true,
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
| account/originatorUuid                 | The originator uuid that is stored in the associated key management system                                                                           | String                        |
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

## Onboard on Provenance

Used to onboard to provenance.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/executetx`

**Method**: POST

**Request Body:**

```
{
   "chainId": "pio-testnet-1",
   "nodeEndpoint": "grpc://192.168.1.242:9090",
   "tx": <tx body>,
   "account": {
      "originatorUuid": <uuid>,
      "keyRingIndex": "0",
      "keyIndex": "0",
      "isTestNet": true,
    },
}
```

| Field                  | Description                                                                | Data Type |
| ---------------------- | -------------------------------------------------------------------------- | --------- |
| chainId                | The blockchain identifier                                                  | String    |
| nodeEndpoint           | The url to the provenance node to run against                              | String    |
| tx                     | The tx body that should be broadcast to provenance                         | String    |
| account/originatorUuid | The originator uuid that is stored in the associated key management system | String    |
| account/keyRingIndex   | The key ring index. Used to identify the provenance account.               | String    |
| account/keyIndex       | The key index. Used to identify the provenance account.                    | String    |
| account/isTestNet      | If true, testnet shall be used, otherwise mainnet                          | Bool      |
