# Installing Provenance

### Prerequisites 

* Linux or Mac OS
* Go 1.15+ \([https://golang.org/doc/install](https://golang.org/doc/install)\)
* LevelDB 1.23 \([https://github.com/google/leveldb](https://github.com/google/leveldb)\)

All interaction with Provenance whether creating a node, querying, or invoking actions can be accomplished using the `provenanced` command. 

{% hint style="info" %}
See the [Provenance testnet repository](https://github.com/provenance-io/testnet) for latest "Software Version" information of "pio-testnet-1" 
{% endhint %}

### Installing

Installing `provenanced` is done directly from the source code by cloning the version indicated for the testnet from the  [Provenance Github](https://github.com/provenance-io/provenance) repo, then make&install:

```text
$ git clone -b <version> https://github.com/provenance-io/provenance
$ cd provenance && make install
```

{% hint style="info" %}
`<version> is prefixed with 'v' when cloning version branches (eg. v0.2.0)`
{% endhint %}

### Verify

To verify that the command has been successfully installed we can check the version that has been installed.

```text
$ provenanced version
<version>
```

## Next

Now that `provenanced` is installed, let's start by introducing the types of nodes participating on the network that provide the foundation for transacting on Provenance.

{% page-ref page="running-a-node-1/" %}

If you already have a node running, let's start using Provenance.

{% page-ref page="../using-provenance/" %}

