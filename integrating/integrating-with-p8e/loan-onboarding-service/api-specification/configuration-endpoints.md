# Configuration Endpoints

## Enable Replication Across Object Stores

Some situations arise where you want the data written to one p8e Object Store to be replicated to another p8e Object Store. This is commonly required to allow desired assets to be shared across parties with parties sharing only public information and not having to expose their private secrets.  This results in all objects stored into the specified source object store into the target object store with the same object hash. That allows both the source and target object store to retrieve that object using their own URI "object:/\<Object Store URL>/\<hash>".

**URL**: `https://{host}/api/v1/config/replication/enable`

**Method:** POST

**Request Body:**

```
{
    "sourceObjectStoreAddress": "grpc://object-store-v2.p8e:80",
    "targetObjectStoreAddress": "grpc://object-store-2-v2.p8e:80",
    "targetPublicKey":"<target system public key>"
}
```

| Field                     | Description                                                                                                   | Data Type |
| ------------------------- | ------------------------------------------------------------------------------------------------------------- | --------- |
| sourceObjectStoreAddress  | The URL to the encrypted object store to that object will be replicated from.                                 | String    |
| targetObjectStoreAddress  | The URL to the encrypted object store to that object will be replicated to.                                   | String    |
| targetSigningPublicKey    | The signing public key of the affiliate to register                                                           | String    |
| targetEncryptionPublicKey | The public key that the source encrypted object store will use to write to the target encrypted object store. |           |

**Response:**

204 represents a success replication request.
