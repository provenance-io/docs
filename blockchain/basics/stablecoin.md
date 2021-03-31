---
description: >-
  High-level creation of a coin marker on Provenance from an omnibus bank
  perspective.
---

# Stablecoin

## Overview

Stablecoins provide a bridge between fiat and digital currency used as the basis for transactions of value on Provenance. Each new stablecoin is represented on the blockchain as a [marker ](../../modules/marker-module.md)managed by the issuer. Issuers of stablecoin manage fiat currency in a traditional banking account structure that handles the necessary BSA/AML obligations. Issuing institutions have complete control over the management of their coin and provide a redemption method where a holder can convert the digital holding to fiat over banking rails. 

{% hint style="info" %}
See [Omnibus Banks](../../ecosystem/participants/omnibus-banks.md) for more participant information.
{% endhint %}

## Creating a Stablecoin

{% hint style="info" %}
To follow along with this section refer to [Installing Provenance](../running-a-node/) to install the `provenanced` binary.
{% endhint %}

### Creating the Key <a id="Creating-a-Key"></a>

{% hint style="danger" %}
Securing encryption keys is outside the scope of this example. 
{% endhint %}

All stores of value on Provenance are secured by one or more encryption keys. Generated keys should be stored securely to protect against unauthorized use. See [Creating a Key](../using-provenance/#creating-a-key-s) for details on creating encryption keys.

### Creating the Marker <a id="Creating-a-New-Coin"></a>

There are multiple ways to configure a coin to suit a business use case. Here is an example that demonstrates a stablecoin that will be minted and burned to keep the supply in circulation 1:1 with the backing fiat held by the issuer. 

{% hint style="info" %}
`<initial_supply>` will be set to `0` and a denom name of `lrc` will be used for this example.
{% endhint %}

```bash
provenanced --testnet --chain-id pio-testnet-1 tx marker new <initial_supply><denom_name> --type COIN --from stakeholder1 --gas auto --gas-adjustment 1.3 --fees 3000nhash
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

### Setting Marker Permissions <a id="Setting-Permissions"></a>

Marker permissions allow multiple different encryption keys to interact with the underlying functionality it provides. The address used in this example is the same as the manager of the marker, making a single key the only permissioned user to mint/burn and grant/revoke permissions.  

#### \`admin\` 

Allow the grantee to grant privileges to other addresses.

```text
provenanced tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc admin --from stakeholder1 --fees 5000nhash --testnet --chain-id pio-testnet-1
```

#### \`mint\`

Allow the grantee to mint additional tokens.

```text
provenanced tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc mint --from stakeholder1 --fees 5000nhash --testnet --chain-id pio-testnet-1
```

#### \`burn\` 

Allow the grantee to burn tokens.

```text
provenanced tx marker grant tp19fn5mlntyxafugetc8lyzzre6nnyqsq95449gt lrc burn --from stakeholder1 --fees 5000nhash --testnet --chain-id pio-testnet-1
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
provenanced tx marker finalize lrc --from stakeholder1 --fees 5000nhash --testnet --chain-id pio-testnet-1
```

Activating the marker will ensure that the supply is updated according to the settings for total supply. 

```text
provenanced tx marker activate lrc --from stakeholder1 --fees 5000nhash --testnet --chain-id pio-testnet-1
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
MARKER_STATUS_ACTIVE
```
{% endhint %}

## Review

* A marker has been created and now represents a stablecoin valued 1:1 with fiat inventoried by a trusted institution. 
* Permissions on the marker have been granted to a single encryption key that has permissions to grant/revoke access, mint/burn token. 
* A denom of `lrc` has been established on Provenance which is the name reference for this coin. 

Now that we have a fully functioning stablecoin marker, let's continue and look at how we mint and burn token allowing a 1:1 bridge from fiat to digital currency.

## Omnibus

A middle-tier component, that we'll simply call "omnibus," will be used to describe the software processes that would be used to integrate Provenance with a core banking system. Omnibus will bridge a core banking system API with Provenance to monitor for banking transactions that require minting of stablecoin and to monitor for blockchain transactions that require burning of stablecoin. 

### Fiat to Stablecoin \(Mint\)

The process to convert fiat to stablecoin can be handled in several different ways. Here we'll look at a few that tend to be useful.  

#### Receiving Wires

Wiring of funds tends to be used for large transactions and it one of the simplest ways to identify received funds, but does tend to come with a high-probability for human error since the wire reference is hand entered on the transmission side of the wire. 

![](https://i.imgur.com/JWTPqkr.png)

#### Virtual Accounts

Assigning a user an account/routing number can be a useful tool that makes converting deposits to stablecoin simple. In this use case the client simply sends funds via ACH to an account number at the institution. The account/routing number is 1:1 with a blockchain address and the omnibus facilitates the mapping, minting and transfer of stablecoin.

![](https://i.imgur.com/iPz9GdT.png)

#### ACH Pull

In some cases it is necessary to allow a small scale user to initiate a pull of funds from their current brick and mortar account to be delivered to their blockchain address as stablecoin. This use case is by far the most complex since ACH has many clearing issues that an institution has to consider.

![](https://i.imgur.com/7wbQjWU.png)

### Stablecoin to Fiat \(Burn\) <a id="MintingBurningTransferring"></a>

Once a blockchain address holds stablecoin, it is necessary to provide a process to convert it to fiat at some time in the future. To achieve this conversion the omnibus must understand where the outbound transfer of fiat will be sent and what reference to watch for on the blockchain when stablecoin is received for redemption. 

#### Memo Field

![](https://i.imgur.com/tZ7c5Np.png)



## Verified Coins

While coins in general can be created on Provenance by any entity not all coins are issued by trusted entities. Trusting the creator of the coin is important because the store of value is a bridge for t-0 settlement with fiat. While the coin denom\(name\) is secured on Provenance by an encryption key, it is necessary to know your counter party when using stablecoins to guarantee the validity of the entity backing the issued coin.

Determining a method to verify trust lines between entities on the blockchain is an exercise that each entity should determine on their own, but using a trusted list of partner addresses can simplify choosing known good actors on the network. 



