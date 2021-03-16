# Installing Provenance

### Prerequisites 

* Linux or Mac OS
* Go 1.5+ \([https://golang.org/doc/install](https://golang.org/doc/install)\)
* LevelDB \([https://github.com/google/leveldb](https://github.com/google/leveldb)\)

All interaction with Provenance whether creating a node, querying, or invoking actions can be accomplished using the `provenanced` command. 

#### Install

```text
$ git clone -b v0.2.1 https://github.com/provenance-io/provenance
$ cd provenance && make install
```

#### Verify

```text
$ provenanced version
v0.2.1
```

###  Next

Now that Provenance is installed you can [access an already existing node](../using-provenance.md) or [create a new node](running-a-node-1/).







Here we discuss how to set up and run a node.

Would include Full Node, query Node, Delegating

Model after the Gaia testnet docs here:

### 

{% embed url="https://hub.cosmos.network/main/gaia-tutorials/what-is-gaia.html" %}

Node upgrade processes and Cosmovisor



