---
description: Querying blockchain state.
---

# Query Lifecycle

A [**query**](https://docs.cosmos.network/master/building-modules/messages-and-queries.html#queries) is a request for information made by end-users of applications through an interface and processed by a full-node. Users can query information about the network, the blockchain itself, and blockchain state directly from the blockchain's stores or modules.  Queries differ from transactions in that they do not require consensus to be processed and therefore do not change state.  Queries can be fully handled by a single full-node.

{% hint style="info" %}
This section uses the `provenanced` application to connect to the Provenance testnet.  Follow the installation instructions in the [Installing Provenance section](../running-a-node/) before continuing with this section.
{% endhint %}





