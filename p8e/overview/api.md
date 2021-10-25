# SDK

### Overview

The P8e Contract Execution Software Development Kit \(SDK\) consists of a collection of JVM based libraries that help manage interactions with Provenance Blockchain, or more specifically, "scopes" as defined by the Metadata module.

The [SDK](https://github.com/provenance-io/p8e-scope-sdk) repository is the best source for more concrete descriptions and runnable examples. It also contains a docker-compose environment that allows for running a complete local stack.

### Major Elements

#### Contract Execution

At its core, the SDK is responsible for executing contracts amongst one or more participants. The remainder of the SDK documentation will expand on this component.

#### Contract Bootstrapping

A gradle plugin is provided that manages the process of memorializing the declarative specifications needed for contract execution.

#### Handling Execution Results

After successful contract execution, the result will be a collection of Provenance Blockchain protobuf messages. At this point all of the records have been encrypted and stored in EOS. At this stage messages can be memorialized to Provenance Blockchain with the Provenance Blockchain HTTP or gRPC interface. Optionally, the Provenance Blockchain event stream may be read to asynchronously detect changes made to scopes previously submitted.

#### Proto Indexer

Post execution it might be required to persist the records in a system for easy search-ability at a later point in time. The protobuf indexer provides a way to accomplish this by converting the protobuf into an, optionally filtered, key-value JSON structure. A custom [protobuf descriptor](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.descriptor) is provided that supports a hierarchical whitelisting/blacklisting of nested protobuf messages.

