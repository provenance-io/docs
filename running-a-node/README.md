# Installing Provenance

### Prerequisites 

* Linux or Mac OS
* Go 1.15+ \([https://golang.org/doc/install](https://golang.org/doc/install)\)
* LevelDB \([https://github.com/google/leveldb](https://github.com/google/leveldb)\)

All interaction with Provenance whether creating a node, querying, or invoking actions can be accomplished using the `provenanced` command. 

{% hint style="info" %}
See the [Provenance testnet repository](https://github.com/provenance-io/testnet) for latest version information
{% endhint %}

Installing `provenanced` is done directly from the source code located on the [Provenance Github](https://github.com/provenance-io/provenance).

```text
$ git clone -b <version> https://github.com/provenance-io/provenance
$ cd provenance && make install
```

To verify that the command has been successfully installed we can check the version that has been installed.

```text
$ provenanced version
<version>
```



### Next

Now that `provenanced` you should start by running a node on the network that will allow access to Provenance. 

{% page-ref page="running-a-node-1/" %}

If you already have a node running, let's start using Provenance.

{% page-ref page="../using-provenance/" %}







Here we discuss how to set up and run a node.

Would include Full Node, query Node, Delegating

Model after the Gaia testnet docs here:

### 

{% embed url="https://hub.cosmos.network/main/gaia-tutorials/what-is-gaia.html" %}

Node upgrade processes and Cosmovisor



