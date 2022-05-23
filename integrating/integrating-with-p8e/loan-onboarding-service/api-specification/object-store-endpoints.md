# Object Store Endpoints

## Create Object in EOS

Used to encrypt and store objects in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

**URL**: `https://{host}/api/v1/eos`

**Method**: POST

**Request Body**:

```
{
  "type": <type of asset>
  "message": <Base 64 encoded byte array of file>
  "objectStoreAddress": "grpc://object-store-v2.p8e:80",
  "permissions": {
    "audiences": [],
    "permissionDart": true,
    "permissionPortfolioManager": true,
  }
}
```

| Field                                  | Description                                                                                   | Data Type                     |
| -------------------------------------- | --------------------------------------------------------------------------------------------- | ----------------------------- |
| objectStoreAddress                     | The URL to the encrypted object store to run against                                          | String                        |
| message                                | The asset that is stored against the EOS. This is a Json formatted protobuf object.           | Json Object                   |
| objectStoreAddress                     | The URL to the encrypted object store to run against                                          | String                        |
| permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS | List\<Base64EncodedPublicKey> |
| permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.               | Bool                          |
| permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.  | Bool                          |
| type                                   | The fully qualified protobuf object type.                                                     | String                        |

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

**Request Params:**

```
https://{host}/api/v1/eos?objectStoreAddress=grpc://object-store-v2.p8e:80&hash=<stored hash to retrieve>&type=<fully qualified protobuf type>,
```

| Field              | Description                                          | Data Type |
| ------------------ | ---------------------------------------------------- | --------- |
| type               | The fully qualified protobuf object type.            | String    |
| hash               | The hash of the saved object in EOS                  | String    |
| objectStoreAddress | The URL to the encrypted object store to run against | String    |

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

## Create File in EOS

Used to encrypt and store files in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

**URL**: `https://{host}/api/v1/eos/file`

**Method:** POST

**Request Body:** `form-data`

| Field              | Description                                                                 | Data Type |
| ------------------ | --------------------------------------------------------------------------- | --------- |
| ObjectStoreAddress | The object store address to save the object against.                        | Part      |
| Permissions        | The permissions object used for configuring additional audiences. Optional. | Part      |
| file               | The file to upload                                                          | FilePart  |

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

## Get File in EOS

Used to retrieve files in the object store. See Encrypted Object Store for additional information.

**URL**: `https://{host}/api/v1/eos/file`

**Method:** GET

**Request Params:**

```
https://{host}/api/v1/eos/file?objectStoreAddress=grpc://object-store-v2.p8e:80&hash=<stored hash to retrieve>,
```

| Field              | Description                                          | Data Type |
| ------------------ | ---------------------------------------------------- | --------- |
| hash               | The hash of the saved object in EOS                  | String    |
| objectStoreAddress | The URL to the encrypted object store to run against | String    |

**response:**

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
