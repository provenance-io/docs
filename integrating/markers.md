# Markers

## Wallets:

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

Refer to \[\[BIP32\]{.ul}\]\[7\], \[\[BIP39\]{.ul}\]\[8\], and \[\[BIP44\]{.ul}\]\[9\] for more in depth explanations of wallets.

## Markers:

![](../.gitbook/assets/markers.png)

The Provenance Blockchain provides the direct benefit of facilitating transfer of value between two parties without an intermediary. Provenance uses a simple construct called a marker to manage full and fractional asset ownership on the blockchain. Markers are defined using Protocol Buffers \(Protobuf\) and consist of the following structure.

* **Address:** The address is analogous to a UUID. Addresses are contained within wallets. Marker ownership is determined based on the owner of the wallet containing the address.
* **Type:** Each marker has an associated type that defines the behavior the marker exhibits \(i.e. Account, Loan Pool, Stock, Syndication, Participation, etc...\)
* **Scopes:** Markers wrap assets that have been defined using the Provenance Contract Engine. Each scope UUID can only exist within a single marker on the blockchain.
* **Shares:** Ownership of a marker can be fractionalized into shares. A good example is a loan participation marker that holds many scopes. We could issue 25 equal shares and then transfer them to another owner's wallet thus securing the new owner's ownership in the loan participation.
* **Ownership:** Wallet markers can store ownership of other markers. The string in the ownership map is the address of the marker owned while the int64 is the number of shares held.
* **State:** Transaction states are stored by the blockchain for processing purposes \(i.e. Proposed, Finalized, Cancelled, Destroyed\).
* **Parent Address:** Can and should be null in most cases, but if present indicates the parent marker.

