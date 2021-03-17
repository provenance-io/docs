# Installing Provenance

### Prerequisites 

* Linux or Mac OS
* Go 1.15+ \([https://golang.org/doc/install](https://golang.org/doc/install)\)
* LevelDB 1.23 \([https://github.com/google/leveldb](https://github.com/google/leveldb)\)

{% hint style="info" %}
New go executables are installed at "$GOPATH/bin" where the environment variable GOPATH defaults to "~/go" when not set. Remember to add either "$GOPATH/bin" or "~/go/bin" to your PATH when GOPATH is either set or not.
{% endhint %}

{% hint style="success" %}
On MacOS, LevelDB can be installed with `brew install leveldb`.
{% endhint %}

### 'provenanced' Install

All interaction \(whether creating a node, querying, or invoking actions\) with Provenance can be accomplished using the `provenanced` command. 

{% hint style="info" %}
See the [Provenance testnet repository](https://github.com/provenance-io/testnet) for the latest "Software Version" information of `pio-testnet-1`
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

Check the version number to verify that the command has been successfully installed. 

```text
$ provenanced version
<version>
```

## Next

After `provenanced` is installed, introduce the types of nodes participating on the network that will provide the foundation for transacting on Provenance.

{% page-ref page="running-a-node-1/" %}

If there is a node running, you may start using Provenance.

{% page-ref page="../using-provenance/" %}

