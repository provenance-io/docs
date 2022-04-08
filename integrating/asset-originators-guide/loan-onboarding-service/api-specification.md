---
description: Loan Onboarding Service API Spec
---

# API Specification

## Create Object in Object-Store

Used to store objects in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/eos`

**Method**: POST

**Body**:

```
{
  "assetId": <Scope UUID of Loan>
  "asset": <Base 64 encoded byte array of file>
  "objectStoreAddress": "grpc://object-store-v2.p8e:80",
  "audiences": [],
  "permissionDart": true,
  "permissionPortfolioManager": true,
  "isTestNet": true,
  "account": {
    "originatorUuid": <uuid>,
    "keyRingIndex": "0",
    "keyIndex": "0",
  },
}
```

| Field                      | Definition                             | Data Type |
| -------------------------- | -------------------------------------- | --------- |
| assetId                    | Unique identifier for the asset        |           |
| asset                      | Base64 encoded byte array of the asset |           |
| objectStoreAddress         |                                        |           |
| audiences                  |                                        |           |
| permissionDart             |                                        |           |
| permissionPortfolioManager |                                        |           |
| isTestNet                  |                                        |           |
|                            |                                        |           |

**Response**:

```
{
    "hash": "iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "uri": "object://test.figure.com/iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "bucket": "/mnt/data",
    "name": "42b2f0a8-dc40-470f-8b29-6de71486d2ff"
}
```

## Get Object in Object-Store

Used to retrieve objects in the object store. See Encrypted Object Store for additional information.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/eos`

**Method:** GET

&#x20;**Body:**

```
{
   "originatorUuid": <UUID>,
   "hash": <stored hash to retrieve>,
   "objectStoreAddress": "grpc://object-store-v2.p8e:80",
}
```

## Create Snapshot

Used to create a snapshot of a stored object in the object store. See Encrypted Object Store for additional information.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/eos/snapshot`

**Method:** POST

&#x20;**Body:**

```
{
    "account": {
        "originatorUuid": <uuid>,
        "keyRingIndex": "0",
        "keyIndex": "0",
    },
    "hash": <hash of stored object>,
    "objectStoreAddress": "grpc://object-store-v2.p8e:80",
    "audiences": [],
    "permissionDart": false,
    "permissionPortfolioManager": false,
}
```

**Response:**

```
{
    "hash": "iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "uri": "object://test.figure.com/iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "bucket": "/mnt/data",
    "name": "42b2f0a8-dc40-470f-8b29-6de71486d2ff"
}
```

## Create Contract Specification

The Loan Onboarding Service exposes three REST API endpoints for interactions with Provenance.&#x20;

Used to write p8e contract specifications to the Object Store and Provenance. See [Specifications](https://docs.provenance.io/p8e/p8e-usage/specifications) for additional information.&#x20;

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/specifications`

**Method**: POST

**Body**:

```
{
    "chainId": "pio-testnet-1",
    "nodeEndpoint": "https://rpc.test.provenance.io:443",
    "keyMnemonic": "walnut bubble shoe neck broccoli elevator assume puzzle business baby gentle suffer equal duty nephew domain adjust spin cigar response what sniff clip garment",
    "isTestNet": true,
    "keyRingIndex": "0",
    "keyIndex": "0",
    "scopeId": "4e554cb8-56dd-48df-b3fe-71f4c5b7d2cf",
    "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
    "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f"
}
```

## Create Tx and onboard on Provenance

Used to create scope tx and onboard assets to Provenance.&#x20;

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e`

**Method**: POST

**Body**:

```
{
   "chainId": "pio-testnet-1",
   "nodeEndpoint": "grpc://192.168.1.242:9090",
   "txRequest": {
    "account": {
         "originatorUuid": <uuid>,
         "keyRingIndex": "0",
         "keyIndex": "0",
       },
     "audiences": [],
     "permissionDart": false,
     "permissionPortfolioManager": false,
     "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
     "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
     "scopeId": <scope uuid>,
     "hash": <object hash>
   },
}
```

## Create Scope Transmission

Used to create scope tx.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/scope`

**Method**: POST

**Body**:

```
{
   "account": {
     "originatorUuid": <uuid>,
     "keyRingIndex": "0",
     "keyIndex": "0",
   },
   "audiences": [],
   "permissionDart": false,
   "permissionPortfolioManager": false,
   "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
   "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
   "scopeId": <scope uuid>,
   "hash": <object hash>
}
```

## Onboard on Provenance

Used to onboard to provenance.

**URL**: `https://figure.com/service-loan-onboarding/external/api/v1/p8e/onboard`

**Method**: POST

**Body:**

```
{
   "chainId": "pio-testnet-1",
   "nodeEndpoint": "grpc://192.168.1.242:9090",
   "tx": <tx body>,
   "account": {
      "originatorUuid": <uuid>,
      "keyRingIndex": "0",
      "keyIndex": "0",
    },
}
```
