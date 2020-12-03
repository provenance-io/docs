# Markers

The Provenance Blockchain provides the direct benefit of facilitating transfer of value between two parties without an intermediary. Provenance uses a simple construct called a marker to manage full and fractional asset ownership on the blockchain. Markers are defined using Protocol Buffers \(Protobuf\) and consist of the following structure.

![](../.gitbook/assets/markers.png)

* **Address:** The address is analogous to a UUID. Addresses are contained within wallets. Marker ownership is determined based on the owner of the wallet containing the address.
* **Type:** Each marker has an associated type that defines the behavior the marker exhibits \(i.e. Account, Loan Pool, Stock, Syndication, Participation, etc...\)
* **Scopes:** Markers wrap assets that have been defined using the Provenance Contract Engine. Each scope UUID can only exist within a single marker on the blockchain.
* **Shares:** Ownership of a marker can be fractionalized into shares. A good example is a loan participation marker that holds many scopes. We could issue 25 equal shares and then transfer them to another owner's wallet thus securing the new owner's ownership in the loan participation.
* **Ownership:** Wallet markers can store ownership of other markers. The string in the ownership map is the address of the marker owned while the int64 is the number of shares held.
* **State:** Transaction states are stored by the blockchain for processing purposes \(i.e. Proposed, Finalized, Cancelled, Destroyed\).
* **Parent Address:** Can and should be null in most cases, but if present indicates the parent marker.

