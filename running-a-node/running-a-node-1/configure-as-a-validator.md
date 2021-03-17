# Configure a Validator

Validators perform the critical function of proposing and validating transactions on the Provenance network. A strong network of validators ensures Provenance security is maintained. Validators stake Hash™ to become part of the active validators on the network and are a foundational element for Hash holders that want to delegate their stake and share in rewards produced by the network's fee distribution framework. 

### Quick Start

This quick start assumes that the necessary steps to join a full node to the Provenance network have already been completed and the node is ready to be designated as a validator. 

{% hint style="warning" %}
To configure a validator, Hash needs to grant the newly created validator voting power on the network. 
{% endhint %}

Each Tendermint node has a public key that identifies it to other participants on the network. To configure the full node created in [Join Provenance Testnet](join-provenance-testnet.md), find the public key that it identifies. Tendermint provides a simple way to display the key for use.

#### Finding Node Public Key

Use the following to find a node's public key.

```text
~$ provenanced -t --home tmp/data tendermint show-validator
```

#### Staking Hash to Become a Validator

The following command has detail that should be closely reviewed. 

| Parameter | Description |
| :--- | :--- |
| chain-id | cha |
| home | home directory containing the blockchain data for the node |
| moniker | the name of a validator that should be shown to other participants on the network |
| pubkey | Public key determined using the tendermint show-validator command |
| amount | amount of Hash to delegate as voting power on the network |
| from | the account that holds the Hash to be delegated |
| fees |  |
| commission-rate |  |
| commission-max-rate |  |
| commission-max-change-rate |  |
| min-self-delegation |  |
| broadcast-mode |  |

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



