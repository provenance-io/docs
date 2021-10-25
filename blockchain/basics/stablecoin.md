---
description: High-level creation of a coin marker on Provenance Blockchain.
---

# Coin

## Overview

A Coin on Provenance is implemented as a [Marker](../../modules/marker-module.md). Each coin that is generated can be transferred freely between blockchain [accounts ](accounts.md)and represents a value exchange between parties. A coin marker is a simple structure that is meant to be used as a building block for a more complex use case, such as a [stablecoin](stablecoin.md#stablecoins).

## Creating a Coin

{% hint style="info" %}
To follow along with this section, refer to [Installing Provenance](../running-a-node/) to install the `provenanced` binary and have an [encryption key created](../using-provenance/#creating-a-key-s).
{% endhint %}

There are multiple ways to configure a coin to suit a business use case. Here is an example that demonstrates how a coin is created, minted, burned, and transacted. 

| Parameter | Description |
| :--- | :--- |
| `initial_supply` | Initial supply of tokens as an integer |
| `denom` | Name of the coin being created |
| `key_name` | Name of the key from the key store that was previously created |

{% hint style="info" %}
`<initial_supply>` will be set to 0, indicating that this coin will not have a fixed supply.
{% endhint %}

```bash
provenanced --testnet --chain-id pio-testnet-1 tx marker new <initial_supply><denom> --type COIN --from <key_name> --fees 5000nhash
```

#### Verifying

```text
provenanced --testnet q marker get <denom>
```

```text
marker:
  '@type': /provenance.marker.v1.MarkerAccount
  access_control: []
  allow_governance_control: false
  base_account:
    account_number: "25"
    address: tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8
    pub_key: null
    sequence: "0"
  denom: <denom>
  manager: tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt
  marker_type: MARKER_TYPE_COIN
  status: MARKER_STATUS_PROPOSED
  supply: "0"
  supply_fixed: false
```

The marker is now in `PROPOSED` status and is ready for configuration.

{% hint style="success" %}
Notice that the address for the marker is a newly created Provenance address that the utilized encryption key manages.

**Marker Address** `tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8`

**Manager** **Address** `tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt`
{% endhint %}

### Setting Marker Permissions <a id="Setting-Permissions"></a>

Marker permissions allow multiple encryption keys to interact with the underlying functionality it provides. The address used in this example is the same as the manager of the marker, making a single key the only permissioned user to mint/burn and grant/revoke permissions.  

#### `admin` 

Allow the grantee to grant privileges to other addresses.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt <denom> admin --from <key_name>--fees 5000nhash 
```

#### `mint`

Allow the grantee to mint additional tokens.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt <denom> mint --from <key_name> --fees 5000nhash
```

#### `burn`

Allow the grantee to burn tokens.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt <denom> burn --from <key_name> --fees 5000nhash
```

#### `withdraw`

Allow the grantee to withdraw minted tokens that are stored in the marker's account. 

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt <denom> withdraw --from <key_name> --fees 5000nhash
```

### Review, Finalize and Activate

Let's review the marker to ensure that the permissions have been set correctly before it is finalized.

```text
provenanced q marker get <denom>
```

```text
marker:
  '@type': /provenance.marker.v1.MarkerAccount
  access_control:
  - address: tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt
    permissions:
    - ACCESS_ADMIN
    - ACCESS_BURN
    - ACCESS_MINT
  allow_governance_control: false
  base_account:
    account_number: "25"
    address: tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8
    pub_key: null
    sequence: "0"
  denom: <denom>
  manager: tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt
  marker_type: MARKER_TYPE_COIN
  status: MARKER_STATUS_PROPOSED
  supply: "0"
  supply_fixed: false
```

{% hint style="info" %}
Note the permissions for the address allow `ADMIN, BURN, MINT` 
{% endhint %}

Finalizing the marker will cause the marker to begin enforcing the permissions granted. The manager will no longer be able to modify the marker without permissions after finalization.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker finalize <denom> --from <key_name> --fees 5000nhash
```

Activating the marker will ensure that the supply is updated according to the settings for total supply. 

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker activate <denom> --from <key_name> --fees 5000nhash
```

#### Verify Activation

```text
provenanced q marker get <denom>
```

```text
marker:
  '@type': /provenance.marker.v1.MarkerAccount
  access_control:
  - address: tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt
    permissions:
    - ACCESS_ADMIN
    - ACCESS_BURN
    - ACCESS_MINT
    - ACCESS_WITHDRAW
  allow_governance_control: false
  base_account:
    account_number: "25"
    address: tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8
    pub_key: null
    sequence: "0"
  denom: <denom>
  manager: ""
  marker_type: MARKER_TYPE_COIN
  status: MARKER_STATUS_ACTIVE
  supply: "0"
  supply_fixed: false
```

{% hint style="info" %}
Note that the marker is now `ACTIVE` and has permissions set for use.

```text
status: MARKER_STATUS_ACTIVE

access_control:
  - address: tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt
    permissions:
    - ACCESS_ADMIN
    - ACCESS_BURN
    - ACCESS_MINT
    - ACCESS_WITHDRAW
```
{% endhint %}

## Review

* A marker has been created and now represents a new coin type.
* Permissions on the marker have been granted to a single encryption key that has permissions to grant/revoke access, mint/burn token. 
* A `denom` has been established on Provenance as the name reference for this coin. 

Now that we have a fully functioning coin, let's continue and look at how we mint, burn, and transfer it. 

## Basic Usage

### Minting

```text
provenanced --testnet --chain-id pio-testnet-1 \
  --from stakeholder1 --fees 5000nhash tx marker mint 500<denom>
```

### Burning

```text
provenanced --testnet --chain-id pio-testnet-1 \
  --from stakeholder1 --fees 5000nhash tx marker burn 500<denom>
```

{% hint style="success" %}
Verification of minting and burning needs to be accomplished by querying the **address of the marker** not the address that has permissions to mint and burn. 

### Query

```text
provenanced --testnet --chain-id pio-testnet-1 q bank balances tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8
```

```text
balances:
- amount: "500"
  denom: <denom>
```
{% endhint %}

### Withdrawing \(Transfer\)

Withdrawing from a marker involves three addresses: 

1. Marker Address
2. Address with Permissions to Withdraw _\(This is the address that signs for the transaction\)_
3. Recipient Address

Let's look at the current coin values held by the three addresses involved in the `withdraw` transaction. 

#### Coin Marker `tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8`

```text
balances:
- amount: "500"
  denom: <denom>
```

#### Address with Permissions to Withdraw `tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt`

```text
balances:
- amount: "0"
  denom: <denom>
```

#### Recipient Address `tp1jdwgsdhdu692wsfreymglvz6aam59jh3uef4ve`

```text
balances:
- amount: "0"
  denom: <denom>
```

{% hint style="info" %}
The balances that are `0` above are shown for example purposes only and will not be displayed in output from the provenanced command.
{% endhint %}

#### Executing the Withdraw

As the address with permissions to withdraw from the marker, we can move coin that has been minted to any address on the Provenance blockchain. 

```text
 provenanced --testnet --chain-id pio-testnet-1 --fees 5000nhash --from <key_name> tx marker withdraw <denom> 500<denom> <recipient_address>
```

Once the withdraw has been completed the balances of two accounts have been updated. 

#### Coin Marker `tp12tpv7m43vu7dkfnq648q2l65v3tk9x6mn0x2a8`

```text
balances:
- amount: "0"
  denom: <denom>
```

#### Recipient Address `tp1jdwgsdhdu692wsfreymglvz6aam59jh3uef4ve`

```text
balances:
- amount: "500"
  denom: <denom>
```

{% hint style="info" %}
The recipient has received 500`denom` that was minted, withdrawn and transferred to their account. Note that the withdraw process includes the recipient as a part of the process. 
{% endhint %}

### Holders of Coin

A coin holder often wants to understand what addresses their coin is being held by. Provenance provides a simple way of perform this lookup.

```text
provenanced --testnet --chain-id pio-testnet-1 q marker holding <denom>
```

We can now see that the current holders of the coin we created, minted, and transferred are just the single recipient. 

```text
balances:
- address: tp1jdwgsdhdu692wsfreymglvz6aam59jh3uef4ve
  coins:
  - amount: "500"
    denom: <denom>
```

### Review

It is important to discern that addresses are identifiers that point to accounts on Provenance Blockchain. Each [account](accounts.md) on Provenance can hold coins of various denominations, and [markers](../../modules/marker-module.md) are a special type of account that has its own denomination, can hold coins, and can hold NFTs \(Scopes\) described later. 

In this exercise a coin was created, tokens of that coin were minted, and then subsequently transferred to a recipients address. The tokens that were transferred are now held by the recipient and are no longer within the control of the marker, manager, addresses that have permissions on the marker. 

## Stablecoins

The coin created above can be used as a stablecoin to provide a bridge between fiat and digital currency as the basis for transactions of value on Provenance Blockchain. Each new stablecoin is represented on the blockchain as a [marker ](../../modules/marker-module.md)managed by the issuer. Issuers of stablecoin manage fiat currency in a traditional banking account structure that handles the necessary BSA/AML obligations. Issuing institutions have complete control over the management of their coin and provide a redemption method where a holder can convert the digital holding to fiat over banking rails. 

{% hint style="info" %}
See [Omnibus Banks]() for more participant information.
{% endhint %}

