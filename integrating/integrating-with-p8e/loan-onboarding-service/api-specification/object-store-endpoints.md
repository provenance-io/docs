# Object Store Endpoints

## Create Object in EOS

Used to encrypt and store objects in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

**URL**: `https://{host}/api/v1/eos`

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

| Field                      | Description                                                                                   | Data Type                     |
| -------------------------- | --------------------------------------------------------------------------------------------- | ----------------------------- |
| uli                        | The uli (universal loan identifier) that identifies a loan                                    | String                        |
| asset                      | The asset that is stored against the EOS. This can be a file, loan package, xml, etc          | Base 64 Encoded Byte Array    |
| objectStoreAddress         | The URL to the encrypted object store to run against                                          | String                        |
| audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS | List\<Base64EncodedPublicKey> |
| permissionDart             | If the dart product should be allowed permission against the saved data in EOS.               | Bool                          |
| permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.  | Bool                          |
| istestNet                  | If true, testnet shall be used, otherwise mainnet                                             | Bool                          |
| account/originatorUuid     | The originator uuid that is stored in the associated key management system                    | String                        |
| account/keyRingIndex       | The key ring index. Used to identify the provenance account.                                  | Int                           |
| account/keyIndex           | The key index. Used to identify the provenance account.                                       | Int                           |

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

**URL**: `https://{host}/api/v1/eos`

**Method:** GET

**Request Body:**

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

**Response:**

```
// Example Asset
{
    "id" : {
        "value" : <Object UUID>
    },
    "type" : "FILE",
    "description" : <description of the asset>,
    "kv" : {
        "bytes" : {
            "typeUrl" : "type.googleapis.com/google.protobuf.BytesValue",
            "value" : <file data>
        }
    }
}
```

## Create Snapshot in the Object Store

Used to create a snapshot of a stored object in the object store. See Encrypted Object Store for additional information.

**URL**: `https://{host}/api/v1/eos/snapshot`

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

| Field                                  | Description                                                                                   | Data Type                     |
| -------------------------------------- | --------------------------------------------------------------------------------------------- | ----------------------------- |
| account/originatorUuid                 | The originator uuid that is stored in the associated key management system                    | String                        |
| account/keyRingIndex                   | The key ring index. Used to identify the provenance account.                                  | Int                           |
| account/keyIndex                       | The key index. Used to identify the provenance account.                                       | Int                           |
| hash                                   | The hash of the object that is currently stored in EOS to create a snapshot against           | String                        |
| objectStoreAddress                     | The URL to the encrypted object store to run against                                          | String                        |
| permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS | List\<Base64EncodedPublicKey> |
| permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.               | Bool                          |
| permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.  | Bool                          |
| account/isTestNet                      | If true, testnet shall be used, otherwise mainnet                                             | Bool                          |

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

