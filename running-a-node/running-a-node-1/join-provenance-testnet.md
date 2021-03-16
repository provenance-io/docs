---
description: Join a locally installed Provenance node to the testnet.
---

# Join Provenance Testnet

Let's start by spinning up a full node to get a feel for how a node can be used to develop applications that integrate with Provenance.

### Quick Start

The quickest way to start a node is to install the `provenanced` daemon process, initiliaze a local installation, download the genesis file, and start the `provenanced` node in the foreground.  Be sure to change `choose-a-moniker` to a custom name for your node.

```text
$ export PIO_HOME=~/.provenanced
$ git checkout tags/v0.2.0 -b v0.2.0
$ make clean
$ make install
$ provenanced init choose-a-moniker --chain-id pio-testnet-1 --testnet
$ wget https://raw.githubusercontent.com/provenance-io/testnet/main/pio-testnet-1/genesis.json
$ mv genesis.json $PIO_HOME/config
$ provenanced start --p2p.seeds 2de841ce706e9b8cdff9af4f137e52a4de0a85b2@104.196.26.176:26656,add1d50d00c8ff79a6f7b9873cc0d9d20622614e@34.71.242.51:26656 --x-crisis-skip-assert-invariants
```

> Provenance nodes take about 45 minutes to start up.  During startup the `provenanced` daemon will output state sync information such as:
>
> ```text
> 2:20PM INF committed state app_hash=3AA9147C2DBAE3328BAF633B6F33B1FBB6557FE8D81ECBC769A5AFB8DDFE98E3 height=29475 module=state num_txs=0
> ```

### 

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



