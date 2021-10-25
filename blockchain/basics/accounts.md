---
description: Understanding the Provenance accounts system.
---

# Accounts

## Overview

On Provenance Blockchain, an [account](https://docs.cosmos.network/v0.41/basics/accounts.html) designates a pair of _public key_ `PubKey` and _private key_ `PrivKey`. The `PubKey` is used to generate an `address` that is used to identify users \(among other parties\) on the blockchain. `Addresses` are also associated with `messages` to identify the sender of the `message`. The `PrivKey` is used to generate [digital signatures](https://docs.cosmos.network/master/basics/query-lifecycle.html#signatures) to prove that an `address`associated with the `PrivKey` approved of a given `message`.

### Addresses <a id="addresses"></a>

`Addresses` and `PubKey`s are both public information that identifies actors on the blockchain. `Account` is used to store authentication information. 

Each account is identified using an `address` which is a sequence of bytes derived from a public key. In Provenance Blockchain, 3 types of addresses specify a context where an account is used:

* `AccAddress` identifies users \(the sender of a `message`\).
* `ValAddress` identifies validator operators.
* `ConsAddress` identifies validator nodes that are participating in consensus. Validator nodes are derived using the EC `ed25519` curve wherease users use the `secp256k1` curve.

Addresses and public keys are formatted using [Bech32](https://en.bitcoin.it/wiki/Bech32) and implemented as a string value. The Bech32 method is the only supported format to use when interacting with the blockchain. The Bech32 human-readable part \(Bech32 prefix\) is used to denote an address type.

{% hint style="info" %}
Provenance testnet Bech32 addresses begin wit **`tp`** whereas mainnet addresses begin with **`pb`**.  
{% endhint %}

{% hint style="info" %}
_**A key pair and it's corresponding Bec32 address that exist outside of Provenance \(say**_, _**in a wallet\) is not a Provenance account until Hash has been transferred to the Bech32 address.**_
{% endhint %}

{% hint style="info" %}
To follow along with this section refer to [Installing Provenance](../running-a-node/) to install the `provenanced` binary.
{% endhint %}

For example, using `provenanced` we can generate a local key pair and store it in the default [Keyring](https://docs.cosmos.network/master/basics/accounts.html#keyring):

```bash
provenanced --testnet keys add my_test_key  

- name: my_test_key
  type: local
  address: tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv
  pubkey: tppub1addwnpepqg44k06r0m37z30fnaxw64em2ay8yc435extdf2ngqmxephcgy4nz40wqaf
  mnemonic: ""
  threshold: 0
  pubkeys: []

```

At this point, the `address` `tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv` is **not** a Provenance account:

```bash
provenanced --testnet query auth account tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv \
  --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
Error: rpc error: code = NotFound desc = account tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv not found: key not found
```

However, once we use the testnet Provenance Faucet to transfer Hash to our `address`, it becomes an account.  Post our `address` to the faucet:

```bash
curl 'https://test.provenance.io/blockchain/faucet/external'  \
  -X 'POST'  -H 'Content-Type: application/json'  \
--data-binary '{"address":"tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv"}'
```

Then, try the `provenanced` account query again:

```bash
provenanced --testnet query auth account tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv \                <<<
  --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
'@type': /cosmos.auth.v1beta1.BaseAccount
account_number: "66"
address: tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv
pub_key: null
sequence: "0"
```

### Authentication

The principal way of authenticating a user is done using [digital signatures](https://en.wikipedia.org/wiki/Digital_signature). Users sign transactions using their own private key. Signature verification is done with the associated public key. For on-chain signature verification purposes, the public key is stored in an `Account` object \(alongside other data required for a proper transaction validation\) as shown in the previous section.

## HD Wallet

As demonstrated in the previous section, a single key pair can be used to generate a Bech32 address that can then be used to create a Provenance account.  Using a single key pair isn't ideal because it requires the user to generate and save the physical key pair.  Instead, most blockchains utilize an HD Wallet solution that automatically generates a hierarchical tree-like structure of private/public addresses \(or keys\), thereby addressing the problem of the user having to generate them on their own.

HD Wallets, or Hierarchical Deterministic wallets, are used to generate addresses from a single master seed \(hence the name hierarchical\).  A seed is usually created from a 12 or 24 word mnemonic. A single seed can derive any number of `PrivKey`s using a one-way cryptographic function. Then, a `PubKey` can be derived from the `PrivKey`. Naturally, the mnemonic is the most sensitive information, as private keys can always be re-generated if the mnemonic is known. 

HD Wallets utilize a single backup that enables the user to fully restore the data at any time in the future. This is because of the wallet’s ability to drive all the private keys of the tree using [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki). This is a transfer protocol that enables parent keys to create child keys in a hierarchy.

So, when you have to restore an HD Wallet, the system automatically derives all the private keys of the tree using the same initial algorithm and the same private keys emerge.

The secret key is easier to remember as well, so users don’t risk losing access to their funds. It’s also a more convenient solution as you don’t have to store the secret key for each address that you’ve generated, only for the seed key.  [Several HD Wallets exists for Cosmos-based networks](https://cosmos.network/ecosystem/wallets) \(as Provenance is\).

HD Wallet addresses follow a "hierarchical" format:

![](../../.gitbook/assets/wallets.png)

The wallet will have a **Master Node** or root. This is the master extended key. Each child extended key is derived from the parent extended key. The next level is **Purpose** which is a constant set to `44'` indicating that the subtree of this node is used according to this specification. The next level is **Coin Type** which is also a constant set to `505'` signifying [Provenance Blockchain's registered coin type](https://github.com/satoshilabs/slips/blob/master/slip-0044.md). The next level is **Account** which is equivalent to a Custodial Wallet. Accounts are generated from the root account. **Scope** is used to distinguish between internal and external addresses. The last node is where **accounts** are identified by addresses within a wallet. Addresses consist of the following four parts:

1. human readable prefix
2. separator
3. address
4. checksum for integrity

Given our address in the previous example:

```bash
tp1tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2tj6eesv
```

* the prefix on testnet is `tp` \(note: the prefix on mainnet is `pb`\)
* the separator is `1`
* the address is `tkn2dwfkx7pmjr2rtgqhtrudsv7h8w2t`
* the checksum is `j6eesv`

Refer to [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki), [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), and [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) for more in depth explanations of wallets.

### Key Derivation Details

As discussed in the previous section the [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki), [BIP43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki), and [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) specifications define a standard for path notation for building the HD Wallet address hierarchy. Hardened derivation at a particular level is indicated by an apostrophe. For example, the following example of hardened derivation is used for the first three levels, while for the last two levels non-hardened derivation is used \(typically\):

`m / 44' / 505' / 0' / 0 / 0`

There are two possible types of BIP32 derivation, hardened and non-hardened keys.

### Hardened vs. Non-hardened <a id="Hardened-vs-Non-hardened"></a>

With non-hardened keys, you can prove a child public key is linked to a parent public key using just the public keys. You can also derive public child keys from a public parent key, which enables watch-only wallets. With hardened child keys, you cannot prove that a child public key is linked to a parent public key.

For security reasons, using hardened keys is safer, but there are use cases for using non-hardened keys. A parent extended public key together with a non-hardened child private key can expose the parent private key. This means that extended public keys must be treated more carefully than regular public keys. It is also the reason for the existence of hardened keys and why they are used for the account level in the tree. This way, a leak of account-specific \(or below\) private keys never risks compromising the master or other accounts.

Knowing an extended public keys allows reconstruction of all descendant non-hardened public keys. Knowledge of a parent extended public key plus any non-hardened private key descending from it is equivalent to knowing the parent extended private key \(and thus every private and public key descending from it\). 

Hardened keys are useful whenever you don’t need the ability to give someone a master public key, and have them know all derivative addresses. Privacy is preserved with these types of keys because you cannot prove that a child public key is linked to a parent public key. Provenance user-facing wallets should use hardened keys on all paths.

Unhardened keys are useful for application service accounts like an escrowing service. With this type of service you want the ability to generate addresses that people pay and that the service can spend from. With an unhardened key there is no need to store private key information at the service yet there is still the ability to generate addresses. Thus there is no sensitive information held at the service. There is a loss of privacy in this model as one could prove a child public key is linked to a parent public key thereby identifying the service account.

### Path Derivation <a id="Path-Levels"></a>

Provenance uses a Parameter path where `m/44'/505'/0'/0/0` == `m / purpose' / coinType' / account' / change / addressIndex`. It is within this path that hardened vs. non-hardened is realized.

#### Root <a id="Root"></a>

```text
m / 44' / 505' /  0' /  0  /  0
**
```

The Root level indicates the the extended key pair and seed value that is used to deterministically generate all other keys in the tree. This is the key pair that is restored from a mnemonic phrase.

#### Purpose <a id="Purpose"></a>

```text
m / 44' / 505' /  0' /  0  /  0
   ****
```

Purpose is a constant set to `44'` following the BIP43 recommendation. It indicates that the subtree of this node is used according to this specification.

Hardened derivation is used at this level.

#### CoinType <a id="CoinType"></a>

```text
m / 44' / 505' /  0' /  0  /  0
         *****
```

CointType is a constant set to `505'` for mainnet coins and `1'` for testnet coins. The Provenance mainnet coin type \(`505'`\) is registered here: [https://github.com/satoshilabs/slips/blob/master/slip-0044.md](https://github.com/satoshilabs/slips/blob/master/slip-0044.md).

Hardened derivation is used at this level.

#### Account <a id="Account"></a>

```text
m / 44' / 505' /  0' /  0  /  0
                ****
```

The Account level is a sequentially numbered index beginning at `0`. This level splits the key space into independent user identities, so the wallet never mixes the coins across different accounts.

Users can use these accounts to organize the funds in the same fashion as bank accounts; for donation purposes \(where all addresses are considered public\), for saving purposes, for common expenses etc.

Hardened derivation is used at this level.

#### Change <a id="Change"></a>

```text
m / 44' / 505' /  0' /  0  /  0
                      ****
```

The wallet chain identifier. Provenance uses the constant `0` to represent external chain and constant `1` for internal chain \(also known as change addresses\). `Change` addresses are used in bitcoin for the UTXO output from a transaction that is retained by the sender. Most blockchains outside of bitcoin \(and even many bitcoin wallets\) only use `0` for this value.

External chain is used for addresses that are meant to be visible outside of the wallet \(e.g. for receiving payments\).

Internal chain is used for addresses which are not meant to be visible outside of the wallet and is used for return transaction change.

Non-hardened derivation should be used at this level. However, keys that Provenance custodies on behalf of Provenance Identities \(i.e. Users\) are hardened at this level.

#### Address Index <a id="Address-Index"></a>

```text
m / 44' / 505' /  0' /  0  /  0
                            ****
```

Addresses are numbered from index 0 in sequentially increasing manner. This number is used as child index in BIP32 derivation.

Unhardened derivation is used at this level. However, keys that Provenance custodies on behalf of Provenance Identities \(i.e. Users\) are hardened at this level.

In Provenance Blockchain, the Address Index is used as the key for items of value like coins, markers, names and tags.

