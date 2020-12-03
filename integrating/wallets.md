# Wallets

Provenance uses Hierarchical Deterministic wallets \(or \"HD Wallets\"\). Each identity in Provenance will have a wallet with the following structure.

![](../.gitbook/assets/wallets.png)

Each identity will have a Master Node or root. This is the master extended key. Each child extended key is derived from the parent extended key. The next level is Purpose which is a constant set to 44\' indicating that the subtree of this node is used according to this specification. The next level is Coin Type which is also a constant set to 505\' signifying Provenance's registered coin type. The next level is Account which is equivalent to a Custodial Wallet. Accounts are generated from the root account. Scope is used to distinguish between internal and external addresses. The last node is where accounts are identified by addresses within a wallet. Addresses consist of the following four parts:

1. human readable prefix
2. separator
3. address
4. checksum for integrity

An example test address is

tp10g8wz4krkyy9yq3muhws4ufxp0hwkrn69rhhee where:

* the prefix in test is "tp" \(note: the prefix in prod is "pb"\)
* the separator is "1"
* the address is "0g8wz4krkyy9yq3muhws4ufxp0hwkrn6"
* the checksum is "9rhhee"

Refer to [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki), [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), and [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) for more in depth explanations of wallets.

