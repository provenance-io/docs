---
description: >-
  The Marker module provides an ownership structure for managing tokenized value
  backed by assets or other tokenized units of value denominated as unit(s) of
  coin.
---

# Marker

## Overview

Using the blockchain as a ledger requires the ability to track fungible and non-fungible resources on chain with fractional ownership. Each of these resources requires rules governing supply and exchange. Examples of resources include fractional ownership in the network itself \(stake\), credits for network resources \(gas/fees\), fractional ownership of an arbitrary contract \(markers\), and omnibus account balances \(stable coins\). The rules governing the asset must be enforced by the blockchain itself such that the entity controlling the asset must abide by these rules and is not able to invalidate these processes. These enforced constraints are what provide the value and support trust in the platform itself.

* **Coin** - A transferable instance with a unique name matching a token and whole integer value.
* **Token** - A token is representation of digital value stored on the blockchain enumerated in coins.
* **Marker** - A special type of Token represented by a contract that maintains an ownership structure and an optional collection of assets.

#### Coins

Coins on Provenance Blockchain are a simple structure composed of an integer amount and a string denomination that matches a token name. The coin construct is designed such that any two accounts can freely exchange any whole number fraction of the amount owned by one account to the other through a one way send operation. This operation requires no approval from the receiver of the coin and only a valid signature with a key address matching the address on the account sending the coins.

```text
Coin {
    Denom,      // lowercase string beginning with a letter, 3-64 in length
    Amount,     // integer value greater than or equal to zero
}
```

#### Token

A token is a construct to represent value using `coins` according to a set of rules. These rules include the total supply, initial ownership, inflation rules, etc. A token's issued `coins` can be transacted like any other coin. They must go through an `exchange` to change denominations. A token is either created by defining a coin denomination and distribution in the genesis block, or through a special `marker` module that provides these features on a running network.

#### Markers

A Marker instance is a special type of account that contains a management structure for controlling a token.  Markers provide the ability to manage the supply of a token and the value that backs it to to the governance process and a configurable set of accounts.  Marker administrators may increase or decrease supply as well as manage the assets in escrow.

## Marker Functions

* **Administration** - the marker module provides a granular set of permissions that can be granted to other accounts to delegate their control or suppress it.  In addition the marker module exposes these administration functions to the governance module to allow proposal based control of all existing markers.
* **Supply** - each marker has a collection of supply management functions that can be used to increase, decrease, and enforce the total configured supply of a marker's token.
* **Collateral and Escrow** - a marker instance supports holding coins like any other blockchain account and provides the ability to deposit and withdraw 

### Administration

Management functions are permissioned on a per-instance basis and are restricted to a specific address\(es\) with a list of permissions, or subject to a **Governance proposal**. A governance proposal is a special construct handled by the `gov` module that takes a submitted configuration proposal and posts it for vote by stake holders. If the vote passes then the proposed changes are applied.

When a `marker` is created the address that submits the transaction is bound as the manager of the marker. While the `marker` is in this initial `proposed` state the manager address is allowed to submit transactions to modify without further permission checks. After a marker transitions to the `active` state the manager is removed and the normal permission controls are fully enforced.

| Action | Permission | Notes |
| :--- | :--- | :--- |
| Create New Marker | _unrestricted, governance_  | Depending on the module configuration the system may allow any account with appropriate gas to create a marker or the process may be restricted to requiring a successful governance proposal |
| Grant Access | `grant` | Allows adding additional access grants to the marker.  Accounts with this permission are considered full administrators of the marker and may delegate  or remove access for accounts. |
| Increase Supply | `mint` | Allows `mint` \(increase supply\) for an active status marker |
| Decrease Supply | `burn` | Allows `burn` \(decrease supply\) for an active status marker |
| Add Asset to Pool | `deposit` | Allows assignment of a `scope` against the marker \(Note: scope permission checks are separately performed, address must have access to both marker and scope to perform this action\) |
| Withdraw Collateral | `withdraw` | For an active marker allows withdrawing coin or removing assets from pool |
| Finalize | n/a | For a `proposed` status marker, finalizes configuration \(manager only\) |
| Activate | n/a | For a `finalized` status marker, activates configuration \(manager only\) |
| Cancel | `delete` | Move a `finalized`, `active`, or `proposed` marker to a cancelled state |
| Delete | `delete` | Destroys a `canceled` state marker |
| Transfer | `transfer` | For active markers of denominations that are not `send_enabled`, transfer authority allows the coins to be sent between accounts when the send transaction includes a signature from the account holding this permission using the marker module's send function. This permission only applies to markers with a type of RESTRICTED.  **Note: only `coin` of the `marker` denom may be transfered using this capability** |

```bash
# Grant 'mint' is one of [mint,burn,deposit,withdraw,delete,grant,transfer]
provenanced tx marker \
    grant \
    tp1ev04582v67phlu4huw046fpaq94ppm3mks9eku \
    custom-denom \
    mint
```

### Token Supply

Each marker instance is configured with a supply of coin.  The supply can range from `1..∞` bounded only by the maximum amounts supported by the SDK for storing coin values.  Depending on the type of marker a supply invariant will be enforced that will mint coin directly into the marker's account if an external burn is performed \(for example if a slashing penalty has been applied\).

A marker's supply may be administered directly or though governance proposals depending on the configured access using the `mint` and `burn` functions.

#### **Mint**

The `mint` function is used to issue `token` `coin` against the marker. This action is implicitly done for any amount greater than zero for `Supply` when the finalized marker is `activated`. `mint` may also be performed \(as allowed\) post activation. Minted coins are assigned to the marker account itself after which they maybe withdrawn by an authorized user into their own account. Once withdrawn these coins are moved through normal transfer or exchange processes.

#### **Burn**

An authorized address may "burn" the markers coin, removing it from the global supply coins. The burn function is limited to acting upon coins that are currently held by the marker account itself. For burn operations invoked by a successful governance proposal only coin held in deposit against the marker may be burned. **Note: only `coin` of the current `marker` denom may be burned in any case regardless of what coin denominations are held in the marker's account.**

### Collateral and Escrow

A marker account may hold coin directly similar to any other blockchain account.  The coin's held within the marker may be administered by accounts holding the `deposit` and `withdraw` permissions or though the governance process.  

Similar to other accounts any `send_enabled` coin may be sent to a marker account.  These amounts are subject to the same `deposit` and `withdraw` permissions regardless of their denomination.

The Metadata module extends this capability to provide pools of assets managed using these permissions such that a marker may represent a logical pool.  Through this concept a hierarchy of markers can be created that represents a complex ownership structure.

## Use Cases

### Fiat Backed Stable Coin

A fiat backed managed stable coin is a special type of `token` controlled against an omnibus bank account. The bank itself \(or its designated integrator\) is responsible for ensuring the supply of the token always matches value of the fiat account balance in its omnibus account.

For this type of configuration the `marker` module is used to issue the `token` on chain. The integrator's account address must configure the marker to reserve the right to mint and burn coin issuances in order to match the account balance changes. An initial supply may be set to reflect the starting account balance. The deposit and withdraw features for assets backing the marker do not apply and should be disabled to prevent their accidental use. The ability to modify the settings of the marker should be reserved by the integrator's account address \(if allowed at all\).

The integrator is responsible for distributing any coin minted to the appropriate depositor. The depositor may exchange these coin freely as they see fit for any purpose that elects to accept the `token` denomination. The integrator is also responsible for providing a facility to accept coin and remit value from the omnibus account to the entity presenting the coin. When a withdraw is made from the omnibus bank account the removed fiat balance must be burned from the supply of coin issued from the marker. As the coin in circulation can not be burned directly, this will require the integrator to ensure that any withdraw has an associated transfer of the coin back to the integrator or marker accounts such that the integrator has control of the coin asset to fill the burn request. This process keeps the account in sync with the value represented on chain.

### Hash Staking

In the Provenance Blockchain the power to control the network is measured with the amount of stake an account possesses. This stake is represented as a special type of token denominated as `hash`. As the power to control the network is defined up front, the initial allocations of `hash` coins are distributed in the `genesis` process. The terms of the supply are fixed such that there is no need to use the `marker` module to support any kind of on going management of the `token`. To support these restrictions the marker module list of permissions is empty.  

The `inflation` concept is not used for `hash` and as such the overall supply will always remain at exactly `100 billion` and is enforced through an `invariant` constraint. 

While the specific rules for the `hash` stake token on the Provenance Blockchain are not expected to change, if provisions for this process are desired then the Governance based management process can be used to adjust the parameters of the `hash` marker.

### Asset Pool

The deposit and withdraw permissions on a marker are used by the Metadata module to implement asset pools.  A collection of scope's can be made that reference a marker address and leverage the permission controls to limit who can add or remove scopes from the collection.  This approach allows a strongly controlled list of assets to be associated with a marker in order to represent a pool.

The marker module further provides a helpful set of query functions against the Metadata module in order to return a list of scoped assets assigned to the marker.  Complex ownership structures can be created using these concepts.

### Securities and KYC/AML

Certain types of tokens require that holders meet certain criteria in order to possess the asset.  Typically these restrictions are associated with securities and accredited investors however other applications that require validating the receiving accounts also exist.  For these applications the marker module can be used along with a `send_enabled: false` configuration on the denomination to prevent users from directly transferring value between addresses.

When used in this configuration, the marker module's `transfer` permission allows an address possessing it to directly exchange value using the marker module send function.  This permission can be granted to a smart contract address for example to provide strict controls on the transfer of value of the marker's denominated coins between accounts.

**Warning**: The `transfer` permission is very powerful and provides behavior that user's may not expect as coin's of the marker denomination they hold may be removed from their account directly without their signature when this permission is enabled.  Due to the potential for abuse this permission may only be assigned to accounts on a marker of type RESTRICTED.  Further the type of a marker is fixed when it is created and may not be changed.

### Marker Details

The Provenance Blockchain provides the direct benefit of facilitating transfer of value between two parties without an intermediary. Provenance Blockchain uses a simple construct called a marker to manage full and fractional asset ownership on the blockchain. Markers are defined using Protocol Buffers \(Protobuf\) and consist of the following structure.

```text
type MarkerAccount struct { 
// cosmos base_account including address and account number Address string AccountNumber uint64
PubKey        *types.Any // NOTE: not used for marker, it is not possible to sign for a marker account directly
Sequence      uint64     // NOTE: always zero on marker

// Address that owns the marker configuration.  This account must sign any requests
// to change marker config (only valid for statuses prior to finalization)
Manager string

// Access control lists.  Account addresses are assigned control of the marker using these entries
AccessControl []AccessGrant

// Indicates the current status of this marker record. (Pending, Active, Cancelled, etc)
Status MarkerStatus

// value denomination.
Denom string

// the total supply expected for a marker.  This is the amount that is minted when a marker is created.  For
// SupplyFixed markers this value will be enforced through an invariant that mints/burns from this account to
// maintain a match between this value and the supply on the chain (maintained by bank module).  For all non-fixed
// supply markers this value will be set to zero when the marker is activated.
Supply Int

// Marker type information.  The type of marker controls behavior of its account.
MarkerType MarkerType

// A fixed supply will mint additional coin automatically if the total supply decreases below a set value.  This
// may occur if the coin is burned or an account holding the coin is slashed. (default: true)
SupplyFixed bool

// indicates that governance based control is allowed for this marker
AllowGovernanceControl bool
}
```

#### Marker Types

There are currently two basic types of markers.

* **Coin** - A marker with a type of coin represents a standard fungible token with zero or more coins in circulation
* **Restricted Coin** - Restricted Coins work just like a regular coin with one important difference--the bank module "send\_enabled" status for the coin is set to false. This means that a user account that holds the coin can not send it to another account directly using the bank module. In order to facilitate exchange there must be an address set on the marker with the "Transfer" permission grant. This address must sign calls to the marker module to move these coins between accounts using the `transfer` method on the api.

#### Access Grants

Control of a marker account is configured through a list of access grants assigned to the marker when it is created or applied afterwards through the API calls to add or remove access.

```text
const (
	// ACCESS_UNSPECIFIED defines a no-op vote option.
	Access_Unknown Access = 0
	// ACCESS_MINT is the ability to increase the supply of a marker
	Access_Mint Access = 1
	// ACCESS_BURN is the ability to decrease the supply of the marker using coin held by the marker.
	Access_Burn Access = 2
	// ACCESS_DEPOSIT is the ability to set a marker reference to this marker in the metadata/scopes module
	Access_Deposit Access = 3
	// ACCESS_WITHDRAW is the ability to remove marker references to this marker in from metadata/scopes or
	// transfer coin from this marker account to another account.
	Access_Withdraw Access = 4
	// ACCESS_DELETE is the ability to move a proposed, finalized or active marker into the cancelled state. This
	// access also allows cancelled markers to be marked for deletion
	Access_Delete Access = 5
	// ACCESS_ADMIN is the ability to add access grants for accounts to the list of marker permissions.
	Access_Admin Access = 6
	// ACCESS_TRANSFER is the ability to invoke a send operation using the marker module to facilitate exchange.
	// This capability is useful when the marker denomination has "send enabled = false" preventing normal bank transfer
	Access_Transfer Access = 7
)

// A structure associating a list of access permissions for a given account identified by is address
type AccessGrant struct {
	// A bech32 encoded address string of the account the permissions are assigned to
	Address     string
	 // An array of enum values as defined above
	Permissions AccessList
}
```

#### Fixed Supply vs Floating

A marker can be configured to have a fixed supply or one that is allowed to float. A marker will always mint an amount of coin indicated in its `supply` field when it is activated. For markers that have a fixed supply an invariant check is enforced that ensures the supply of the marker alway matches the configured value. For a floating supply no additional checks or adjustments are performed and the supply value is set to zero when activated.

**When a Marker has a Fixed Supply that does not match target**

Under certain conditions a marker may begin a block with a total supply in circulation less than its configured amount. When this occurs the marker will take action to correct the balance of coin supply.

**A fixed supply marker will attempt to automatically correct a supply imbalance at the start of the next block**

This means that if the supply in circulation exceeds the configured amount the attempted fix is to burn a required amount from the marker's account itself. If this fails an invariant will be broken and the chain will halt.

If the requested supply is greater than the amount in circulation \(as occurs when a coin is burned in a slash\) the marker module will mint the difference between expected supply and circulation and place the created coin in the marker's account.

A supply imbalance typically occurs during the genesis of a blockchain when a fixed supply for a marker is less than the initial balances assigned to accounts. It may also occur if the marker is associated with the bind denom of the chain and a slash penalty is assessed resulting in the burning of a portion of coins.



## State Transitions

This document describes the state transition operations pertaining markers:

* [Undefined](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/02_state_transitions.md#undefined)
* [Proposed](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/02_state_transitions.md#proposed)
* [Finalized](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/02_state_transitions.md#finalized)
* [Active](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/02_state_transitions.md#active)
* [Cancelled](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/02_state_transitions.md#cancelled)
* [Destroyed](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/02_state_transitions.md#destroyed)

### Undefined

The undefined status is not allowed and its use will be flagged as an error condition.

### Proposed

The proposed status is the initial state of a marker. A marker in the `proposed` status will accept changes to supply via the `mint`/`burn` methods, updates to the access list, and state transitions when called by the address set in the `manager` property.

On Transition:

* Proposed is the initial state of a marker by default. It is not possible to transition to this state from any other.

Next Status:

* **Finalized**
* **Cancelled**

### Finalized

The finalized state of the marker is used to verify the readiness of a marker before activating it.

Requirements:

* Marker must exist
* Caller address must match the `manager` address on the marker
* Current status of marker must be `Proposed`
* Supply of the marker must meet or exceed the amount of any existing coin in circulation on the network of the denom of the marker. \(This will only apply \)

On Transition:

* Marker status is set to `Finalized`
* A marker finalize typed event is dispatched

Next Status:

* **Active**
* **Cancelled**

### Active

An active marker is considered ready for use.

On Transition:

* Marker status is set to `Active`
* Requested coin supply is minted and placed in the marker account
* For markers with a `fixed_supply` the Invariant checks are performed in `begin_block`
* Permissions as assigned in the access list are enforced for any management actions performed
* The `manager` field is cleared. All management actions require explicit permission grants.
* A marker activate typed event is dispatched

Next Status:

* **Cancelled**

### Cancelled

A cancelled marker will have no coin supply in circulation. Markers may remain in the Cancelled state long term to prevent their denom reuse by another future marker. If a marker is no longer needed at all then the **Destroyed** status maybe appropriate.

Requirements:

* Caller must have the `delete` permission assigned to their address or
* Caller must be the manager of the marker \(applies only to proposed markers that are Cancelled\)
* The supply of the coin in circulation outside of the marker account must be zero.

On Transition:

* Marker status is set to `Cancelled`
* A marker Cancelled typed event is dispatched

Next Status:

* **Destroyed**

### Destroyed

A destroyed marker is denoted as available for subsequent removal from the state store by clean up processes. Markers in the destroyed status will be removed in the Begin Block ABCI handler at the beginning of the next block \(v1.3.0+\).

On Transition:

* All supply of the coin denom will be burned.
* Marker status is set to `Destroyed`
* Marker will ultimately be deleted from the KVStore during the next ABCI Begin Block \(v1.3.0+\)

Next Status:

* **None**

\*\*\*\*

## Governance Proposal Control

The marker module supports an extensive amount of control over markers via governance proposal. This allows a marker to be defined where no single account is allowed to make modifications and yet it is still possible to issue change requests through passing a governance proposal.

* [Add Marker Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#add-marker-proposal)
* [SupplyIncrease Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#supplyincrease-proposal)
* [SupplyDecrease Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#supplydecrease-proposal)
* [SetAdministrator Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#setadministrator-proposal)
* [RemoveAdministrator Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#removeadministrator-proposal)
* [ChangeStatus Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#changestatus-proposal)
* [WithdrawEscrow Proposal](https://github.com/provenance-io/provenance/blob/main/x/marker/spec/10_governance.md#withdrawescrow-proposal)

### Add Marker Proposal

AddMarkerProposal defines a governance proposal to create a new marker.

In a typical add marker situation the `UnrestrictedDenomRegex` parameter would be used to enforce longer denom values \(preventing users from creating coins with well known symbols such as BTC, ETH, etc\). Markers added via governance proposal are only limited by the more generic Coin Validation Denom expression enforced by the bank module.

A further difference from the standard add marker flow is that governance proposals to add a marker can directly set a marker to the `Active` status with the appropriate minting operations performed immediately.

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L15-L30](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L15-L30)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* The marker request contains an invalid denom value
* The marker already exists
* The amount of coin in circulation could not be set.
  * There is already coin in circulation \[perhaps from genesis\] and the configured supply is less than this amount and it is not possible to burn sufficient coin to make the requested supply match actual supply
* The mint operation fails for any reason \(see bank module\)

### SupplyIncrease Proposal

SupplyIncreaseProposal defines a governance proposal to administer a marker and increase total supply of the marker through minting coin and placing it within the marker or assigning it directly to an account.

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L34-L43](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L34-L43)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* The requested supply exceeds the configuration parameter for `MaxTotalSupply`

### SupplyDecrease Proposal

SupplyDecreaseProposal defines a governance proposal to administer a marker and decrease the total supply through burning coin held within the marker

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L47-L55](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L47-L55)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* Marker does not allow governance control \(`AllowGovernanceControl`\)
* The marker account itself is not holding sufficient supply to cover the amount of coin requested to burn
* The amount of resulting supply would be less than zero

The chain will panic and halt if:

* The bank burn operation fails for any reason \(see bank module\)

### SetAdministrator Proposal

SetAdministratorProposal defines a governance proposal to administer a marker and set administrators with specific access on the marker

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L59-L67](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L59-L67)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* The marker does not exist
* Marker does not allow governance control \(`AllowGovernanceControl`\)
* Any of the access grants are invalid

### RemoveAdministrator Proposal

RemoveAdministratorProposal defines a governance proposal to administer a marker and remove all permissions for a given address

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L71-L79](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L71-L79)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* The marker does not exist
* Marker does not allow governance control \(`AllowGovernanceControl`\)
* The address to be removed is not present

### ChangeStatus Proposal

ChangeStatusProposal defines a governance proposal to administer a marker to change its status

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L82-L90](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L82-L90)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* Marker does not allow governance control \(`AllowGovernanceControl`\)
* The requested status is invalid
* The new status is not a valid transition from the current status
* For destroyed markers
  * The supply of the marker is greater than zero and the amount held by the marker account does not equal this value resulting in the failure to burn all remaining supply.

### WithdrawEscrow Proposal

WithdrawEscrowProposal defines a governance proposal to withdraw escrow coins from a marker

+++ [https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto\#L93-L103](https://github.com/provenance-io/provenance/blob/2e713a82ac71747e99975a98e902efe01286f591/proto/provenance/marker/v1/proposals.proto#L93-L103)

This request is expected to fail if:

* The governance proposal format \(title, description, etc\) is invalid
* Marker does not allow governance control \(`AllowGovernanceControl`\)
* The marker account is not holding sufficient assets to cover the requested withdraw amounts.



