# Components

### Object Store

Data submitted to Provenance is encrypted using ECIES and the DIME format before being stored in the affiliate’s object storage. The Encrypted Object Store was built using Google Cloud Storage. Other data storage solutions can be used providing the required integrations are built. The affiliate submitting the data grants permission to the data to other affiliates. All permissioned affiliates will also store a copy of the encrypted data \(in the DIME format\) in their object store.

### Indexing Engine

When transactions are successfully memorialized on the blockchain, the Event Pump emits block events. The Index Engine is responsible for listening for and consuming these events. Using information contained in these events, the engine queries the object store to retrieve asset data. The transaction structure is defined using a Protobuf, which includes previously identified data fields that are required to be indexed for future use. The indexable fields are extracted and saved to the index data store. Once saved, the index engine submits a message to the Provenance mailbox. Affiliates participating in the transaction can pick up these messages for insight to transaction statuses. The index data store facilitates querying information from the affiliate’s Encrypted Object Store and also the blockchain.

### P8e API

### P8e Web UI

