# Become a Validator

Validators perform the critical function of proposing and validating transactions on the Provenance network. A strong network of validators ensures Provenance security is maintained. Validators stake Hash to become part of the active validators on the network and are a foundational element for Hash holders that want to delegate their stake and share in rewards produced by the network's fee distribution framework. 

## Quick Start

This quick start assumes that you have already completed the necessary steps to join a full node to the Provenance network and are ready to designate the node as a validator. 

{% hint style="warning" %}
To configure a validator you need to acquire Hash to grant the newly created validator voting power on the network. 
{% endhint %}

### Finding Node Public Key

Each node has a public key that identifies it to other participants on the network. In order to configure the full node created in [Join Provenance Testnet](join-provenance-testnet.md), we need to find the public key that identifies. Tendermint, the underlying consensus algorithm, provides a simple way to display the key for use.

```text
~$ provenanced -t --home tmp/data tendermint show-validator
```

### Staking Hash to Become a Validator

The following command has a lot of detail that should be reviewed closely. 

| Parameter | Description |
| :--- | :--- |
| chain-id | cha |
| home | home directory containing the blockchain data for the node |
| moniker | the name of your validator that should be shown to other participants on the network |
| pubkey | Public key determined using the tendermint show-validator command |
| amount | amount of Hash to delegate as voting power on the network |
| from | the account that holds the Hash to be delegated |
| fees |  |
| commission-rate |  |
| commission-max-rate |  |
| commission-max-change-rate |  |
| min-self-delegation |  |
| broadcast-mode | wait until the block containing the transaction is committed |

```text
provenanced -t \
   --keyring-dir <location_of_keyring> \
   --chain-id <chain_id> \
   --home <location_of_blockchain> \
tx staking create-validator \
   --moniker arbiter34.com \
   --pubkey <public_key> \
   --amount 9000000000nhash \
   --from stakeholder0 
   --fees 5000nhash \
   --commission-rate=1.0 \
   --commission-max-rate=1.0 \
   --commission-max-change-rate=1.0 \
   --min-self-delegation 1 \
   --broadcast-mode block
```

## Security

Validators are the most important nodes in the network. As such it requires a level of security that will ensure they are not only highly available but protected at every level. The following will go through the recommend network architecture to ensure the provenance network validators are protected. This has been patterned after the official [Tendermint Documentation](https://docs.tendermint.com/master/nodes/validators.html).

### Network Architecture

A multi-tier network architecture is recommended to secure validators. Each tier of the network increases restrictions on network access.

#### Tiers

| Tier | Description |
| :--- | :--- |
| Public Sentry | Public Sentry nodes are available to the world and provide a proxy for the network. |
| Private Sentry | Private Sentry nodes provide an interconnect layer that can be used to provide more direct integration with large foundational Provenance systems. |
| Valdator | The core of the Provenance blockchain that should be isolated from all public facing networks. |
| KMS | A remote signing platform that secures access to encryption keys. |

![Recommended Network Architecture](../../.gitbook/assets/securing-provenanced-validator-2-%20%281%29.png)

The above network diagram is the recommended architecture to follow when building out your provenance nodes and validator. This can be done in a cloud environment such as AWS, GCP, Azure, within an on premise data center. or a combination of both. The idea of this infrastructure will be further detailed below but conceptually it is required to protect the validator by leveraging multiple layers of network security. 

### Network Security \(Firewall\)

The provenance network leverages two different ports by default the P2P port and the RPC port and should be considered when opening necessary firewall rules.

**P2P**

The persistent peer port \(P2P\) uses tcp port 26656 and is required for nodes to connect to the network and to each other. As such firewall rules should be created that limit access to this port . 

**RPC**

The RPC port is the application port which uses tcp port 26657. This should only be made available to sources that you trust. If you would like to allow more public access such as querying the status of the network of specific api endpoints we recommend using NGINX's reverse-proxy to limit that access.

It is recommended that each of these nodes be placed in specific zones or private networks and leverage network firewalls to limit access. For example, the [Public Sentry](configure-a-sentry.md#public-sentry-nodes) nodes should leverage a public ip and access to these nodes will be open to the public. [Private Sentry](configure-a-sentry.md#private-sentry-nodes) nodes should leverage private ip's or whitelist access with public ip's from the public sentry nodes. Validators should always leverage private ip addresses to ensure they are protected.

## Configuration

The validator node requires the highest level of security as it contains the key that will be authorized to sign blocks on the provenance network. If a bad actor were to get a hold of this key they could connect to the chain impersonating that same validator and cause a double signing incident which would result in a validator being jailed and then slashed. For this reason access to this node should be limited to only those that absolutely require it. All access should be monitored and recorded via a monitoring solution. 

The following configuration parameters are found in the config.toml file

| Configuration | Setting |
| :--- | :--- |
| pex | false |
| persistent-peers | private sentry nodes |
| private-peer-ids | none |
| unconditional-peer-ids | sentry node ids |
| addr-book-strict | false |

## Key Management - HSM

There are multiple solutions available that could be used to provide this security. One solution readily available is [Tendermint KMS](https://github.com/iqlusioninc/tmkms) which is actively being maintained and supports multiple Hardware Security Modules. 

The validator consensus key used to sign blocks on the provenance network must be protected. By default this key is in plain text on the node and anyone with access would be able to obtain it. It is recommended to leverage a remote signer KMS combined with an HSM to ensure the absolute security of the validator key. Due to the strong recommendation of leveraging an HSM, this solution should be built on premise in a Secured Data Center. This would include leveraging network firewalls, secured servers, network switches, and limited access to these devices by necessary personnel.

## Recommended Hardware Configuration

{% hint style="info" %}
CPU/Memory/Storage are determined based on how you intend to use Provenance and how the node is configured \(type\) and data retention periods. These are general use numbers and can be adjusted based on desired performance. 
{% endhint %}

| Node Type | CPU | Memory | Storage |
| :--- | :--- | :--- | :--- |
| Validator | 2vCPU | 8GB | 500GB |



