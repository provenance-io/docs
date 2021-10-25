---
description: >-
  The following provides recommendations to best secure a validator on the
  provenance network
---

# Securing a Validator

Validators are the most important nodes in the network. They require a level of security that ensures that they are highly available and protected at every level. The following will review the recommended network architecture to ensure the provenance network validators are protected. This review is patterned after the official [Tendermint Documentation](https://docs.tendermint.com/master/nodes/validators.html) where additional information may be found.

## Recommended Network Architecture

![](../../.gitbook/assets/securing-provenanced-validator-2-%20%281%29.png)

The above network diagram is the recommended architecture to follow when building out your provenance nodes and validator. This can be done in a cloud environment such as AWS, GCP, Azure, within an on premise data center. or a combination of both. The idea of this infrastructure will be further detailed below but conceptually it is required to protect the validator by leveraging multiple layers of network security. 

### Network Security \(Firewall\)

The provenance network leverages two different ports by default the P2P port and the RPC port and should be considered when opening necessary firewall rules.

**P2P**

The persistent peer port \(P2P\) uses tcp port 26656 and is required for nodes to connect to the network and to each other. As such firewall rules should be created that limit access to this port . 

**RPC**

The RPC port is the application port which uses tcp port 26657. This should only be made available to sources that you trust. If you would like to allow more public access such as querying the status of the network of specific api endpoints we recommend using NGINX's reverse-proxy to limit that access.

It is recommended that each of these nodes be placed in specific zones or private networks and leverage network firewalls to limit access. For example, the Public Sentry nodes should leverage a public ip and access to these nodes will be open to the public. Private Sentry nodes should leverage private ip's or whitelist access with public ip's from the public sentry nodes. Validators should always leverage private ip addresses to ensure they are protected.

## Sentry Nodes \(Public and Private\)

In order to best protect the validator node it should be on its own network front ended by nodes called sentry's. This is done to prevent network attacks being launched directly against a validator network such as DDoS, or attempted brute force, etc. A sentry node is simply a full node that connects to the Provenance Blockchain and relays the chain to the validator. This ensures that the validator network information remains hidden and is not accessible over the public internet.

For the provenance network we recommend two sets of sentry nodes. Both are essentially a simple full node with some additional configuration. It is recommended that at least two of each type of sentry node is created to ensure high availability. Additionally creating these nodes in separate regions/data centers is also recommended.

**Public Sentry Nodes**

These nodes are accessible over the public internet and allow others to leverage your node to connect to the Provenance Blockchain. The access should be limited to the p2p port of 26656 but allow any IP to connect. We recommend these nodes in order to have additional endpoints to connect to and strengthen the Provenance Blockchain. The public sentry's will act as sort of protection for the private sentry nodes. As such it will require you to add each private sentry node id to the **private\_peer**_**\_**_**ids** in the config.toml file to avoid them being gossiped and found on the network. Additional recommended configuration is listed below.

**Private Sentry Nodes**

These nodes are the last line of network protection for the validator nodes. They will connect to the public sentry's in order to continue to relay the provenance network but additionally can be used to connect to specific partners. As an example, if you would like to partner with Company X, you could allow them to connect directly to this layer and whitelist their access via firewall rules. These nodes additionally would require you to add the validator node ids to the  **private\_peer**_**\_**_**ids** in the config.toml file. This will ensure they will not be gossiped on the network. Additional recommended configuration is listed below.

**Sentry Node Configuration**

The following configuration parameters are found in the config.toml file

| Configuration | Setting |
| :--- | :--- |
| pex | true |
| persistent-peers | validator node, other sentry nodes |
| private-peer-ids | private sentry node, validator node ids |
| unconditional-peer-ids | sentry node ids |
| addr-book-strict | false |

### Recommended System Configuration

{% hint style="info" %}
CPU/Memory/Storage are determined based on how you intend to use Provenance Blockchain and how the node is configured \(type\) and data retention periods. These are general use numbers and can be adjusted based on desired performance. 
{% endhint %}

| Node Type | CPU | Memory | Storage |
| :--- | :--- | :--- | :--- |
| Public Sentry/Private Sentry | 4 vCPU | 8GB | 500GB |

## Validator Configuration & Security

The validator node requires the highest level of security as it contains the key that will be authorized to sign blocks on the provenance network. If a bad actor were to get a hold of this key they could connect to the chain impersonating that same validator and cause a double signing incident which would result in a validator being jailed and then slashed. For this reason access to this node should be limited to only those that absolutely require it. All access should be monitored and recorded via a monitoring solution. 

Validator Configuration

The following configuration parameters are found in the config.toml file

| Configuration | Setting |
| :--- | :--- |
| pex | false |
| persistent-peers | private sentry nodes |
| private-peer-ids | none |
| unconditional-peer-ids | sentry node ids |
| addr-book-strict | false |

### Validator Key Management - HSM

The validator consensus key used to sign blocks on the provenance network must be protected. By default this key is in plain text on the node and anyone with access would be able to obtain it. It is recommended to leverage a remote signer KMS combined with an HSM to ensure the absolute security of the validator key. Due to the strong recommendation of leveraging an HSM this solution should be built on premise in a Secured Data Center. This would include leveraging network firewalls, secured servers, network switches, and limited access to these devices by necessary personnel.

There are multiple solutions available that could be used to provide this security. One solution readily available is [Tendermint KMS](https://github.com/iqlusioninc/tmkms) which is actively being maintained and supports multiple Hardware Security Modules. 

### Recommended System Configuration

{% hint style="info" %}
CPU/Memory/Storage are determined based on how you intend to use Provenance Blockchain and how the node is configured \(type\) and data retention periods. These are general use numbers and can be adjusted based on desired performance. 
{% endhint %}

| Node Type | CPU | Memory | Storage |
| :--- | :--- | :--- | :--- |
| Validator | 2vCPU | 8GB | 500GB |



