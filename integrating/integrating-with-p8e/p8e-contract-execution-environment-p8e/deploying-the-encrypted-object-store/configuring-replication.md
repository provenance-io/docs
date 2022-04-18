---
description: How to replicate data from one Object Store to another
---

# Configuring Replication

Provenance Blockchain Object Stores have the ability to replicate encrypted data between themselves. While the feature is not enabled by default, it is easy to turn on and configure.

Replication is managed by the owner of the Object Store. When deploying the Object Store, an owner will need to enable replication at the global level with an environment variable.

{% hint style="info" %}
Replication is enabled by default in the configuration provided by the local deployment script. See the [ENV](https://github.com/provenance-io/p8e-cee-api/blob/main/service/docker/common-object-store-1.env#L4) file for each object store for the variable name.
{% endhint %}

### Controlling Replication

Replication is then controlled by mapping public encryption keys to Object Stores. The idea is that when one party maps another party's public encryption key to that same third-party's object store, data encrypted with that public encryption key will automatically be replicated to the third-party's object store.

Each party that owns their own object store must enable replication in the other direction to properly set up bi-directional replication. This is designed to keep control in the hands of the object store owner.

To make configuration easier, the [p8e Contract Execution Environment API](https://github.com/provenance-io/p8e-cee-api) includes an endpoint to help object store owners map public encryption keys to third-party object stores. The following curl command shows this endpoint in action.

```
curl --location \
--request POST '${HOST}/api/v1/config/replication/enable' \
--header 'Content-Type: application/json' \
--data-raw '{
    "sourceObjectStoreAddress": "grpc://object-store-v2.p8e:80",
    "targetObjectStoreAddress": "grpc://object-store-2-v2.p8e:80",
    "targetPublicKey":"<target party/system public key>"
}'
```

As you can see, the user supplies three data points:

* Source Object Store Address: This is the address of the object store where this configuration will be applied
* Target Object Store Address: This is the address of the object store where encrypted data will be replicated each time the public encryption key provided in this request is used to encrypt data in the source object store
* Target Public Key: This is the key that will be mapped to the target object store. Any time this key is used for encryption in the source object store, data is automatically replicated to the target object store.

### Practical Use

After replication is enabled and public encryption keys are mapped to the appropriate target object store, users can control which object get replicated by choosing when to use that third-party's public encryption key.

When creating an object using the p8e Contract Execution Environment API, clients can specify one of more Additional Audiences. This is where they can choose to pass as many public encryption keys as necessary for their use case. For each encryption key, the object store will automatically check to see if replication is configured and automatically replicate that particular object.

{% hint style="info" %}
It does not automatically replicate every object in the Object Store.
{% endhint %}

### Security Considerations

Developers need to consider how traffic is managed between object stores and who can access the endpoint used to configure replication. Whitelisting IP addresses and/or using API keys for communication between objects stores in two separate environments is recommended. Additionally, the p8e Contract Execution Environment API should be extended so that only the owner of an object store can call the configuration endpoint in a test or production environment.
