---
description: Join a locally installed Provenance node to the testnet.
---

# Join Provenance Testnet

Start a Provenance[ full node ](https://docs.tendermint.com/master/nodes/#node-types)to understand how nodes are used by applications that integrate with Provenance.

## Quick Start

The quickest way to start a node is to install the `provenanced` daemon process, initiliaze a local installation, download the genesis file, and start the `provenanced` node in the foreground.  Be sure to change`choose-a-moniker` to a custom name for your node.

#### Start a Node in Foreground

```text
export PIO_HOME=~/.provenanced
git clone https://github.com/provenance-io/provenance.git
cd provenance
git checkout tags/v0.2.0 -b v0.2.0
make clean
make install
provenanced init choose-a-moniker --chain-id pio-testnet-1 --testnet
wget https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/genesis.json
mv genesis.json $PIO_HOME/config
provenanced start --p2p.seeds 2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51:26656 --x-crisis-skip-assert-invariants
```

> Provenance nodes take about 45 minutes to start up.  During startup the `provenanced` daemon will output state sync information such as:
>
> ```text
> 2:20PM INF committed state app_hash=3AA9147C2DBAE3328BAF633B6F33B1FBB6557FE8D81ECBC769A5AFB8DDFE98E3 height=29475 module=state num_txs=0
> ```

Once the node has synced it is joined to the Provenance testnet.  At this point, the local `provenanced` process is a testnet node suitable for learning the `provenanced` CLI, querying the blockchain, signing and submitting transactions, and developing applications that connect to mainnet. However, there are configurations options to your testnet node more suitable as a long-running process.

## Setting Up a New Node

Unlike the Quick Start instructions, this section will describe setting up a brand new full node from scratch with [Cosmovisor](https://docs.cosmos.network/master/run-node/cosmovisor.html) and better configuration options.  This section effectively configures and starts a Provenance full node.

Before starting this section, be sure the prerequisites have been installed as described in [Installing Provenance](../#prerequisites).

{% hint style="info" %}
See the [testnet repo](https://github.com/provenance-io/testnet) for latest genesis/config files and version information.  The node started in this section is chain id `pio-testnet-1` provenance release 0.2.0 \([https://github.com/provenance-io/provenance/releases/tag/v0.2.0](https://github.com/provenance-io/provenance/releases/tag/v0.2.0)\)
{% endhint %}

### Cleaning up Existing testnet Node

If the Quick Start was followed, it will have saved configuration and start to the `$PIO_HOME` directory.  When setting up a new node, start from a fresh state by cleaning up artifacts and setting the `$PIO_HOME` environment variable:

```bash
rm -rf ~/.provenanced
export PIO_HOME=~/.provenanced
```

### Download and Install Provenance

Use `git` to download the `0.2.0` version of Provenance:

```bash
git clone -b v0.2.0 https://github.com/provenance-io/provenance.git
cd provenance
```

Build the `provenanced` process:

```bash
make clean
make install
```

Confirm the `provenanced` version:

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

Next, initialize the node to create the base config files at `$PIO_HOME`.  Choose a moniker for your node - simply a name containing only ASCII characters.  You can edit this `moniker` later in the `$PIO_HOME/config/config.toml` file.

```bash
provenanced init <your_custom_moniker> --testnet
```

### Download and Install testnet Genesis File

Before starting the `provenanced` node a genesis file must be established.  This can be downloaded from the Provenance testnet site using `wget`:

```bash
wget https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/genesis.json
```

Then move the `genesis.json` file to `$PIO_HOME/config`:

```bash
mv genesis.json $PIO_HOME/config
```

### Manually Configure config.toml Settings

> Provenance provides a base `config.toml` file that can be used instead of following these steps in this section.  Jump to

The `$PIO_HOME/config/config.toml` contains important node settings like [seed node](https://docs.tendermint.com/master/spec/p2p/node.html#seeds) locations, moniker, and database backend.  This section describes the updates to the `$PIO_HOME/config/config.toml` file needed to start a new node.  The [Configure as a Validator](configure-as-a-validator.md) section will describe [validator node](../../blockchain/provenance-blockchain/validator/) specific settings.

#### Configure Seed Nodes

Your node needs to know how to find peers on testnet. This is done by setting seed nodes in the `$PIO_HOME/config/config.toml`file.  The seed nodes are available at the Provenance testnet repo, depending on version.  Since we're using the `pio-testnet-1` chain in this document, [the seed nodes are available here](https://github.com/provenance-io/testnet#pio-testnet-1).

Open the `$PIO_HOME/config/config.toml` file and edit the `seeds` configuration setting using the [seed nodes from the Provenance testnet repo](https://github.com/provenance-io/testnet#pio-testnet-1):

```bash
# Comma separated list of seed nodes to connect to 
seeds = "2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51"
```

#### Configure Database Backend

As listed in the [prerequisites section](../#prerequisites), `leveldb` is the recommended node backend database.  

Open the `$PIO_HOME/config/config.toml` file and edit the `db_backend` configuration setting to use `leveldb`:

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

Change the instrumentation `namespace` option to `provenance` in the `$PIO_HOME/config/config.toml` file:

```bash
# Instrumentation namespace
namespace = "provenance"
```

### Using Provenance testnet config.toml

Instead of manually configuring the `$PIO_HOME/config/config.toml` file as shown in the previous section, a reference file is available at [Provenance testnet.  ](https://github.com/provenance-io/testnet/tree/main/pio-testnet-1)

#### Download and Install testnet config.toml

```bash
wget https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/config.toml
mv config.toml $PIO_HOME/config
```

#### Update Provenance testnet config.toml Moniker

Edit the `$PIO_HOME/config/config.toml` and update the `moniker` to use the moniker set in the [Initialize Provenance Node](join-provenance-testnet.md#initialize-provenance-node) section:

```bash
# A custom human readable name for this node
moniker = "pio-testnet2"
```

### Configure Cosmovisor

`cosmovisor` is a small process manager around Cosmos SDK binaries that monitors the governance module for chain upgrade proposals. Approved proposals will then be run to download the new Provenance code, stop the Provenance node, run the migration script, replace the node binary, and start with the new genesis file.

{% hint style="danger" %}
The `go get` commands in this section **will not work** if you are still in the `provenance` directory created during the github clone.  Change to or create a different directory before running `go get`
{% endhint %}

```bash
go get github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor
```

`cosmovisor` reads its configuration from environment variables:

* `DAEMON_HOME` is the location where upgrade binaries should be kept.  Use `$PIO_HOME.` 
* `DAEMON_NAME` is the name of the Provenance binary, or, `provenanced`.
* `DAEMON_ALLOW_DOWNLOAD_BINARIES` \(_optional_\) if set to `true` will enable auto-downloading of new binaries.
* `DAEMON_RESTART_AFTER_UPGRADE` \(_optional_\) if set to `true` it will restart the sub-process with the same command line arguments and flags \(but new binary\) after a successful upgrade. 

```bash
export DAEMON_NAME="provenanced"
export DAEMON_HOME="${PIO_HOME}"
export DAEMON_ALLOW_DOWNLOAD_BINARIES="true" 
export DAEMON_RESTART_AFTER_UPGRADE="true"
```

#### Create cosmovisor Directories

Cosmovisor requires directories for genesis and wrapping the `provenanced` daemon process.  Create directories and symlinks:

```bash
mkdir -p $PIO_HOME/cosmovisor/genesis/bin
mkdir -p $PIO_HOME/cosmovisor/upgrades
ln -sf $PIO_HOME/cosmovisor/genesis/bin $PIO_HOME/cosmovisor/genesis/current
```

Create a symlink to the `provenanced` daemon in the `cosmovisor` genesis directory:

```bash
cp $(which provenanced) $PIO_HOME/cosmovisor/genesis/bin 
ln -sf $PIO_HOME/cosmovisor/genesis/bin/provenanced $(which provenanced)
```

### Start Node using Cosmovisor

Once `cosmovisor` has been installed and configured, it effectively wraps the `provenanced` daemon process.  Thus, to start the Provenanced node use the `cosmovisor` process:

```bash
cosmovisor start --testnet --home $PIO_HOME
```

A node process is now running in the foreground.  It is an exercise for the reader to integrate the `provenanced` \(again, wrapped by `cosmovisor`\) with a service manager like `systemd` or `launchd`.

### Recommended System Configuration

{% hint style="info" %}
CPU/Memory/Storage are determined based on how you intend to use Provenance and how the node is configured \(type\) and data retention periods. These are general use numbers and can be adjusted based on desired performance. 
{% endhint %}

| CPU | Memory | Storage |
| :--- | :--- | :--- |
| 4 vCPU | 8GB | 100GB |

### Configuring as a Full Node

// TODO Describe a full node and how it would be used.

{% hint style="info" %}
See the [testnet repo](https://github.com/provenance-io/testnet) for latest genesis/config files and version information.
{% endhint %}

\*\*\*\*

**Creating a Provenanced Full Node**  


**\(Linux\)**

**PreRequisites:**

**A VM running a version of Linux, Ubuntu is preferred**  


1. **In order to install a provenanced node and connect to the provenance blockchain you will need to do the following:**
2. **You will need to install the following packages:**
   1.       **- build-essential**
   2.       **- libsnappy-dev**
   3.       **- wget**
   4.       **- libleveldb-dev**
   5.       **- unzip**
   6.       **- jq**
   7.       **- gcc**
   8.       **- libhidapi-dev**
   9.       **- g++**
   10.       **- liblz4-tool**
   11.       **- git**
3. **Create a new directory for your provenanced directory files, for example \(“/provenanced”\)**
4. **You will also need to install at least v1.15 of golang or higher**
5. **Download Cosmovisor by running the following command:**
   1. **go get github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor**
6. **Download the Provenanced binary at the following location:**
   1. **wget** [**https://github.com/provenance-io/provenance/releases/download/v0.2.1/provenance-linux-amd64-v0.2.1.zip**](https://github.com/provenance-io/provenance/releases/download/v0.2.1/provenance-linux-amd64-v0.2.1.zip)
7. **Then unzip the provenanced zip file**
   1. **unzip provenance-linux-amd64-v0.2.1.zip** 
8. **This will create a new bin directory that contains the following files:**
   1. **libwasmvm.so**
   2. **provenanced binary**
9. **Setting Environment variables**
   1. **Export PIO\_HOME=”/provenanced” \(or wherever your configuration will be located**
   2. **Export LD\_LIBRARY\_PATH=”~/bin" \(This is the location where the libwasmvm.so and provenanced binary live\)**
10. **Initialize provenanced test network and create default data and config directories**
    1. **provenanced -t --home /provenanced --chain-id pio-testnet-1 provenanced-node**
11. **After initializing your node you will need to replace the following two files:**
    1. **genesis.json**
    2. **Config.toml**
12. **Download the genesis and config.toml files for the required chain at the following repo location:**
    1. [**https://github.com/provenance-io/testnet/tree/main/pio-testnet-1**](https://github.com/provenance-io/testnet/tree/main/pio-testnet-1)
    2. **Replace the current genesis.json and config.toml files with these files**
13. **Once you have replaced those files you should then be able to spin up the node by running the following command:**
    1. **provenanced -t start** 
14. **You should see your node connect up to the blockchain**
15. **You can also set this as a start up service by creating a systemd service such as the following:**
    1. 

| **\[Unit\] Description=provenanced Requires=network-online.target After=network-online.target  \[Service\] Restart=on-failure User=provenanced Group=provenanced PermissionsStartOnly=true ExecStart=/home/provenanced start -t --home /provenanced/ Environment="PATH=/home/provenanced/go/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/local/go/bin" Environment="LD\_LIBRARY\_PATH=/provenanced/cosmovisor/current/bin" ExecReload=/bin/kill -HUP $MAINPID KillSignal=SIGTERM  \[Install\] WantedBy=multi-user.target** |
| :--- |


1. **Add this file to “/etc/systemd/system/provenanced.service”**
2. **After added run \`systemctl daemon-reload\`**
3. **To start the service run \`systemctl start provenanced\`**
4. **Service should be up and running**

**Setting up a provenanced node with Docker**  


### Next

Configuring the node for your use case \(Validator, Archival, Query...\)



