---
description: High-level creation of a coin marker on Provenance.
---

# Coin

## Overview

A Coin on Provenance is implemented as a [Marker](../../modules/marker-module.md#coins). Each coin that is generated can be transferred freely between blockchain [accounts ](accounts.md)and represents a value exchange between parties. At it's core this is an extremely simple structure that is meant to be used as a building block for a more complex use case such as [stablecoin](stablecoin.md#stablecoins).

## Creating a Coin

{% hint style="info" %}
To follow along with this section refer to [Installing Provenance](../running-a-node/) to install the `provenanced` binary as well as have an [encryption key created](../using-provenance/#creating-a-key-s).
{% endhint %}

There are multiple ways to configure a coin to suit a business use case. Here is an example that demonstrates a stablecoin that will be minted and burned to keep the supply in circulation 1:1 with the backing fiat held by the issuer. 

| Parameter | Description |
| :--- | :--- |
| initial\_supply | initial supply of tokens as an integer |
| denom | name of the coin being created |
| key\_name | name of the key from the key store that was previously created |

{% hint style="info" %}
`<initial_supply>` will be set to `0` and a denom name of `lrc` will be used for this example.
{% endhint %}

```bash
provenanced --testnet --chain-id pio-testnet-1 tx marker new <initial_supply><denom_name> --type COIN --from <key_name> --fees 5000nhash
```

#### Verifying

```text
provenanced q marker get lrc
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
  denom: lrc
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

Marker permissions allow multiple different encryption keys to interact with the underlying functionality it provides. The address used in this example is the same as the manager of the marker, making a single key the only permissioned user to mint/burn and grant/revoke permissions.  

#### \`admin\` 

Allow the grantee to grant privileges to other addresses.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc admin --from <key_name>--fees 5000nhash 
```

#### \`mint\`

Allow the grantee to mint additional tokens.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc mint --from <key_name> --fees 5000nhash
```

#### \`burn\` 

Allow the grantee to burn tokens.

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc burn --from <key_name> --fees 5000nhash
```

#### \`withdraw\`

Allow the grantee to withdraw minted tokens that are stored in the marker's account. 

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc withdraw --from <key_name> --fees 5000nhash
```

### Review, Finalize and Activate

Let's review the marker to ensure that the permissions have been set correctly before it is finalized.

```text
provenanced q marker get lrc
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
  denom: lrc
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
provenanced --testnet --chain-id pio-testnet-1 tx marker finalize lrc --from <key_name> --fees 5000nhash
```

Activating the marker will ensure that the supply is updated according to the settings for total supply. 

```text
provenanced --testnet --chain-id pio-testnet-1 tx marker activate lrc --from <key_name> --fees 5000nhash
```

#### Verify Activation

```text
provenanced q marker get lrc
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
  denom: lrc
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

* A marker has been created and now represents a stablecoin valued 1:1 with fiat inventoried by an institution. 
* Permissions on the marker have been granted to a single encryption key that has permissions to grant/revoke access, mint/burn token. 
* A denom of `lrc` has been established on Provenance which is the name reference for this coin. 

Now that we have a fully functioning coin, let's continue and look at how we mint, burn and transfer it. 

## Basic Usage

### Minting

```text
provenanced --testnet --chain-id pio-testnet-1 --from stakeholder1 --fees 5000nhash tx marker mint 500lrc
```

### Burning

```text
provenanced --testnet --chain-id pio-testnet-1 --from stakeholder1 --fees 5000nhash tx marker burn 500lrc
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
  denom: lrc
pagination:
  next_key: null
  total: "0"
```
{% endhint %}

### Transferring



## Stablecoins

Stablecoins provide a bridge between fiat and digital currency used as the basis for transactions of value on Provenance. Each new stablecoin is represented on the blockchain as a [marker ](../../modules/marker-module.md)managed by the issuer. Issuers of stablecoin manage fiat currency in a traditional banking account structure that handles the necessary BSA/AML obligations. Issuing institutions have complete control over the management of their coin and provide a redemption method where a holder can convert the digital holding to fiat over banking rails. 

{% hint style="info" %}
See [Omnibus Banks](../../ecosystem/participants/omnibus-banks.md) for more participant information.
{% endhint %}



