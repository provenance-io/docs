---
description: >-
  Concepts such as HD wallets & general signature validation on chain (this is a
  draft, mainly @JD's notes for now.
---

# HD Wallets & General chain cryptography

## Problem Statement: Provenance Blockchain Hardened vs. Non-hardened Addresses <a id="Problem-Statement-Provenance-Blockchain-Hardened-vs-Non-hardened-Addresses"></a>

Provenance uses Hierarchical Deterministic Wallets \(or “HD Wallets”\) as defined in the BIP32 specification: [https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)

There are two posible types of BIP32 derivation, hardened or non-hardened keys.

### Hardened vs. Non-hardened <a id="Hardened-vs-Non-hardened"></a>

With non-hardened keys, you can prove a child public key is linked to a parent public key using just the public keys. You can also derive public child keys from a public parent key, which enables watch-only wallets. With hardened child keys, you cannot prove that a child public key is linked to a parent public key.

For security reasons, using hardened keys is safer, but there are use cases for using non-hardened keys. A parent extended public key together with a non-hardened child private key can expose the parent private key. This means that extended public keys must be treated more carefully than regular public keys. It is also the reason for the existence of hardened keys and why they are used for the account level in the tree. This way, a leak of account-specific \(or below\) private keys never risks compromising the master or other accounts.

Knowing an extended public keys allows reconstruction of all descendant non-hardened public keys. Knowledge of a parent extended public key plus any non-hardened private key descending from it is equivalent to knowing the parent extended private key \(and thus every private and public key descending from it\). This means that extended public keys must be treated more carefully than regular public keys. It is also the reason for the existence of hardened keys, and why they are used for the account level in the tree. This way, a leak of account-specific \(or below\) private key never risks compromising the master or other accounts.

Hardened keys are useful whenever you don’t need the ability to give someone a master public key, and have them know all derivative addresses. Privacy is preserved with these types of keys because you cannot prove that a child public key is linked to a parent public key. Provenance user-facing wallets should use hardened keys on all paths.

Unhardened keys are useful for application service accounts like an escrowing service. With this type of service you want the ability to generate addresses that people pay and that the service can spend from. With an unhardened key there is no need to store private key information at the service yet there is still the ability to generate addresses. Thus there is no sensitive information held at the service. There is a loss of privacy in this model as one could prove a child public key is linked to a parent public key thereby identifying the service account.

### Path Levels <a id="Path-Levels"></a>

In the [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki), [BIP43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki), and [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) specifications a standard for path notation is outlined. Hardened derivation at a particular level is indicated by an apostrophe. For example, the following example of hardened derivation is used for the first three levels, while for the last two levels non-hardened derivation is used \(typically\):

`m / 44' / 505' / 0' / 0 / 0`

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

The wallet chain identifier. Provenance uses the constant `0` to represent and external chain and constant `1` for internal chain \(also known as change addresses\). `Change` addresses are used in bitcoin for the UTXO output from a transaction that is retained by the sender. Most blockchains outside of bitcoin \(and even many bitcoin wallets\) only use `0` for this value.

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

In Provenance, the Address Index is used as the key for things of value like coins, markers, names and tags.

### Ideal <a id="Ideal"></a>

Provenance service account keys follow the BIP44 path guidelines where the Change and Address Index path elements are unhardened. This approach relies on the expectation that the addresses below an account should be publicly associated with the account.

Provenance user keys, custodied in Keystone, are hardened on Change constant `0'` and derivative address indexes. Or, as represented by this path: `m/44'/505'/0'/0'/0'`.

### Reality <a id="Reality"></a>

Provenance has been vague about hardening vs unhardening the Change and Address Index path elements.

### Consequences <a id="Consequences"></a>

Provenance Keystone has hardened user-facing \(custodied\) keys and may not have needed to. However, these keys are in use by the Provenance Marketplace for Markerization and cannot be re-purposed \(easily\).

### Proposal <a id="Proposal"></a>

Use unhardened keys \(Change and Address Index path elements\) for service account and escrowing function \(i.e. Omnibus escrow used for stablecoin redemption accounts\).

Continue to use hardened keys \(Change and Address Index paths\) for Provenance custodied keys \(e.g. those created by Provenance Identity sign up and kept in Keystone\).

{% file src="../.gitbook/assets/e2ee-ecies-os.png" %}

