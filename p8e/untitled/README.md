# Integration

Squatch was designed to directly integrate with the Provenance Metadata Module to simplify generating signed records of an asset’s provenance as either a single party or in conjunction with multiple interacting parties. At its core, P8e is a GRPC API that is invoked using a Kotlin SDK. The API processes data through a deterministic client side process that can be shared with other parties to transform data into a format that is hashed and stored as a transaction on the blockchain.

Assets can be directly defined against the Metadata module, but the execution environment assists in the complex data formats, maintenance of immutable objects, and signature orchestration between multiple parties.



