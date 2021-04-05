---
description: Join a locally installed Provenance node to the testnet.
---

# Joining Testnet

Start a Provenance[ full node ](https://docs.tendermint.com/master/nodes/#node-types)to understand how nodes are used by applications that integrate with the Provenance ecosystem.

## Quick Start

The quickest way to start a node is to install the `provenanced` daemon process, initialize a local installation, download the genesis file, and start the `provenanced` node in the foreground.  Be sure to change`choose-a-moniker` to a custom name for the new node.

{% hint style="info" %}
While using the quick-start method provides a quick and easy way to start a testnet node,  it does place the burden of keeping Provenance software up-to-date on the reader.  The recommended approach is to use Cosmovisor to manage the Provenance node as described in the next section.
{% endhint %}

#### Start a Node in Foreground

Use the following to start up a Provenance node in the foreground.

{% hint style="info" %}
Refer to [https://github.com/provenance-io/testnet](https://github.com/provenance-io/testnet) for the latest testnet version.  As of this writing, it is `0.2.0` as reflected in the version tags below.
{% endhint %}

```text
export PIO_HOME=~/.provenanced
git clone https://github.com/provenance-io/provenance.git
cd provenance
git checkout tags/v0.2.0 -b v0.2.0
make clean
make install
provenanced -t init choose-a-moniker --chain-id pio-testnet-1
curl https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/genesis.json > genesis.json
mv genesis.json $PIO_HOME/config
provenanced start --testnet --p2p.seeds 2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51:26656 --x-crisis-skip-assert-invariants
```

> Note that initially, a Provenance node may take about 1-2 hours to start up as it has to sync up with all the old transactions on the blockchain.  During startup, the `provenanced` daemon will output state sync information such as:
>
> ```text
> 2:20PM INF committed state app_hash=3AA9147C2DBAE3328BAF633B6F33B1FBB6557FE8D81ECBC769A5AFB8DDFE98E3 height=29475 module=state num_txs=0
> ```

{% hint style="info" %}
The crisis module halts the blockchain under the circumstance that a blockchain [invariant](https://github.com/cosmos/cosmos-sdk/blob/master/docs/building-modules/invariants.md) is broken. Invariants can be registered with the application during the application initialization process.  During sync, it makes sense to disable this module so the `--x-crisis-skip-assert-invariants` is specified.
{% endhint %}

Once the node has synced it is joined to the Provenance testnet.  Note after the sync is completed, the information sent to the screen is associated with the live blockchain transactions. At this point, the local `provenanced` process is a testnet node suitable for learning the `provenanced` CLI, querying the blockchain, signing and submitting transactions, and developing applications that connect to mainnet. However, there are configuration options to the testnet node that are more suitable as a long-running process.

## Setting Up a New Node

Unlike the Quick Start instructions, this section describes setting up a new full node from scratch with [Cosmovisor](https://docs.cosmos.network/master/run-node/cosmovisor.html) and better configuration options.  This section effectively configures and starts a Provenance full node.

Before starting this section, be sure the prerequisites have been installed as described in [Installing Provenance](../#prerequisites).

{% hint style="info" %}
See the [testnet repo](https://github.com/provenance-io/testnet) for the latest genesis/config files and version information.  The node started in this section is chain id `pio-testnet-1` provenance release 0.2.0 \([https://github.com/provenance-io/provenance/releases/tag/v0.2.0](https://github.com/provenance-io/provenance/releases/tag/v0.2.0)\)
{% endhint %}

### Cleaning up Existing testnet Node

If the Quick Start was followed, it will have saved the configuration and state to the `$PIO_HOME` directory.  When setting up a new node, use the following to start from a fresh state by cleaning up artifacts and setting the `$PIO_HOME` environment variable.

```bash
rm -rf ~/.provenanced
export PIO_HOME=~/.provenanced
mkdir -p $PIO_HOME/config
```

### Download and Install Provenance

Use `git` to download the latest testnet version of Provenance \(`0.2.0` as of this writing\):

```bash
git clone -b v0.2.0 https://github.com/provenance-io/provenance.git
cd provenance
```

Build the `provenanced` process.

```bash
make clean
make install
```

Confirm the `provenanced` version.

```bash
provenanced version --long

name: Provenance
server_name: provenanced
version: 0.2.0
commit: 75fef3a701af3787a56d4c8c6b40f67b95b79eb6
build_tags: netgo,gcc,cleveldb,ledger
go: go version go1.15.5 darwin/amd64
```

### Initialize Provenance Node

Next, initialize the node to create the base config files at `$PIO_HOME`.  Choose a moniker, simply a name containing only ASCII characters, for the node. This `moniker` may be edited later in the `$PIO_HOME/config/config.toml` file.

```bash
provenanced init <your_custom_moniker> --testnet
```

### Download and Install testnet Genesis File

Before starting the `provenanced` node, a genesis file must be established.  This may be downloaded from the Provenance testnet site using `curl`.

```bash
curl https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/genesis.json > genesis.json
```

Move the `genesis.json` file to the Provenance home configuration directory:

```bash
mv genesis.json $PIO_HOME/config
```

### Manually Configure config.toml Settings

> Provenance provides a base `config.toml` file that can be used instead of following these steps in this section.  [Refer to the "Using Provenance testnet config.toml" section for more information.](join-provenance-testnet.md#using-provenance-testnet-config-toml)

The `$PIO_HOME/config/config.toml` contains important node settings including [seed node](https://docs.tendermint.com/master/spec/p2p/node.html#seeds) locations, moniker, and database backend.  This section describes the updates to the `$PIO_HOME/config/config.toml` file needed to start a new node.  The [Become a Validator](become-a-validator.md) section will describe [validator node](../../../ecosystem/community/validator.md)-specific settings.

#### Configure Seed Nodes

Nodes need to know how to find peers on testnet. This is done by setting seed nodes in the `$PIO_HOME/config/config.toml`file.  Depending on the version, the seed nodes are available in the Provenance testnet repo. Because the `pio-testnet-1` chain is used in this document, [the seed nodes are available here](https://github.com/provenance-io/testnet#pio-testnet-1).

Open the `$PIO_HOME/config/config.toml` file and edit the `seeds` configuration setting using the [seed nodes from the Provenance testnet repo](https://github.com/provenance-io/testnet#pio-testnet-1).

```bash
# Comma separated list of seed nodes to connect to 
seeds = "2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51"
```

#### Configure Database Backend

As listed in the [prerequisites section](../#prerequisites), `leveldb` is the recommended node backend database.  

Open the `$PIO_HOME/config/config.toml` file and edit the `db_backend` configuration setting to use `leveldb`.

```bash
# Database backend: goleveldb | cleveldb | boltdb | rocksdb | badgerdb
# * goleveldb (github.com/syndtr/goleveldb - most popular implementation)
#   - pure go
#   - stable
# * cleveldb (uses levigo wrapper)
#   - fast
#   - requires gcc
#   - use cleveldb build tag (go build -tags cleveldb)
# * boltdb (uses etcd's fork of bolt - github.com/etcd-io/bbolt)
#   - EXPERIMENTAL
#   - may be faster is some use-cases (random reads - indexer)
#   - use boltdb build tag (go build -tags boltdb)
# * rocksdb (uses github.com/tecbot/gorocksdb)
#   - EXPERIMENTAL
#   - requires gcc
#   - use rocksdb build tag (go build -tags rocksdb)
# * badgerdb (uses github.com/dgraph-io/badger)
#   - EXPERIMENTAL
#   - use badgerdb build tag (go build -tags badgerdb)
db_backend = "cleveldb"
```

#### Configure Logging

Optionally, and to cut down on informational logs, open the `$PIO_HOME/config/config.toml` file and edit the `log_level` configuration settings to use `error:`

```bash
# Output level for logging, including package level options
log_level = "error"
```

#### Configure Instrumentation

Change the instrumentation `namespace` option to `provenance` in the `$PIO_HOME/config/config.toml` file.

```bash
# Instrumentation namespace
namespace = "provenance"
```

### Using Provenance testnet config.toml

Instead of manually configuring the `$PIO_HOME/config/config.toml` file as shown in the previous section, a reference file is available at [Provenance testnet.  ](https://github.com/provenance-io/testnet/tree/main/pio-testnet-1)

#### Download and Install testnet config.toml

```bash
curl https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/config.toml > config.toml
mv config.toml $PIO_HOME/config
```

Edit the `$PIO_HOME/config/config.toml` and update the `moniker` to use the moniker set in the [Initialize Provenance Node](join-provenance-testnet.md#initialize-provenance-node) section.

```bash
# A custom human readable name for this node
moniker = "pio-testnet2"
```

### Configure Cosmovisor

`cosmovisor` is a small process manager around Cosmos SDK binaries that monitors the governance module for chain upgrade proposals. Approved proposals will then be run to download the new Provenance code, stop the Provenance node, run the migration script, replace the node binary, and start with the new genesis file.

#### Download and Install Cosmovisor using `go get`

{% hint style="danger" %}
The `go get` commands in this section **will not work** if you are still in the `provenance` directory created during the github clone.  Change to or create a different directory before running `go get.`

Some MacOS users have experienced issues with using `go get` to install `cosmovisor` when using `go` version `1.5.x`.  Refer to the Build Cosmovisor from Source section if the `go get` steps do not work.
{% endhint %}

```bash
go get github.com/provenance-io/cosmovisor/cmd/cosmovisor
```

#### Build Cosmovisor from Source \(Optional\)

{% hint style="info" %}
Building `cosmovisor` from source is only necessary if the `go get` installation steps did not work.  

Skip this section if `cosmovisor` has been installed using `go get` in the previous section.
{% endhint %}

Create a new directory to install cosmovisor:

```bash
mkdir -p $PIO_HOME/cosmovisor/install
```

Make the current working directory the new `$PIO_HOME/cosmovisor/install` directory:

```bash
cd $PIO_HOME/cosmovisor/install
```

The Cosmos SDK GitHub repo contains the `cosmovisor` build, clone the repo:

```bash
git clone https://github.com/provenance-io/cosmovisor.git
```

Change to the `cosmovisor` build directory:

```bash
cd cosmovisor
```

Make `cosmovisor`

```bash
make cosmovisor
```

Copy the `cosmovisor` binary to the `$GOPATH`

```bash
cp cosmovisor $GOPATH/bin/cosmovisor
```

#### Export Cosmovisor Environment Variables

`cosmovisor` reads its configuration from environment variables.

* `DAEMON_HOME` is the location where upgrade binaries should be kept.  Use `$PIO_HOME.` 
* `DAEMON_NAME` is the name of the Provenance binary, or, `provenanced`.
* `DAEMON_ALLOW_DOWNLOAD_BINARIES` \(_optional_\) if set to `true` will enable auto-downloading of new binaries.
* `DAEMON_RESTART_AFTER_UPGRADE` \(_optional_\) if set to `true` will restart the sub-process with the same command line arguments and flags \(but new binary\) after a successful upgrade. 

```bash
export DAEMON_NAME="provenanced"
export DAEMON_HOME="${PIO_HOME}"
export DAEMON_ALLOW_DOWNLOAD_BINARIES="true" 
export DAEMON_RESTART_AFTER_UPGRADE="true"
```

#### Create cosmovisor Directories

Cosmovisor requires directories for genesis and wrapping the `provenanced` daemon process.  To create directories and symlinks, use the following.

```bash
mkdir -p $PIO_HOME/cosmovisor/genesis/bin
mkdir -p $PIO_HOME/cosmovisor/upgrades
ln -sf $PIO_HOME/cosmovisor/genesis/bin $PIO_HOME/cosmovisor/genesis/current
```

To create a symlink to the `provenanced` daemon in the `cosmovisor` genesis directory, use the following.

```bash
cp $(which provenanced) $PIO_HOME/cosmovisor/genesis/bin 
ln -sf $PIO_HOME/cosmovisor/genesis/bin/provenanced $(which provenanced)
```

### Start Node using Cosmovisor

Once `cosmovisor` has been installed and configured, it effectively wraps up the `provenanced` daemon process.  To start the Provenanced node, use the following `cosmovisor` process.

```bash
cosmovisor start --testnet --home $PIO_HOME --p2p.seeds 2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51:26656 --x-crisis-skip-assert-invariants
```

A node process should now be running in the foreground.  It is an exercise for the reader to integrate the `provenanced` \(again, wrapped by `cosmovisor`\) with a service manager like `systemd` or `launchd`.

