# Contract Execution Environment

P8e was designed to directly integrate with the Provenance Metadata Module to simplify generating signed records of an asset’s provenance as either a single party or in conjunction with multiple interacting parties. At it’s core is a grpc driven api that is invoked using a kotlin/java sdk. The api processes data through a deterministic client side process that can be shared with other parties to transform data into a hashable format.

Assets can be directly defined against the Metadata module, but the execution environment assists in the complex hashing of data, maintenance of immutable objects, and signature orchestration between multiple parties.

