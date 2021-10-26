---
description: >-
  Classifications of Coins used in the blockchain and how these values are
  recorded on the ledger. Defines standard denominations and purposes.
---

# 102 - Markers, Tokens, and Coins

## Changelog

* 280420: Initial version
* 150420: Additional documentation of coins and gas combustion.

## Context

Using the blockchain as a ledger requires the ability to track fungible and non-fungible resources on chain with fractional ownership. Each of these resources requires rules governing supply and exchange. Examples of resources include fractional ownership in the network itself \(stake\), credits for network resources \(gas/fees\), fractional ownership of an arbitrary contract \(markers\), and omnibus account balances \(stable coins\). The rules governing the asset must be enforced by the blockchain itself such that the entity controlling the asset must abide by these rules and is not able to invalidate these processes. These enforced constraints are what provide the value and support trust in the platform itself.

* **Coin** - A transferable instance with a unique name matching a token and whole integer value.
* **Token** - A token is representation of digital value stored on the blockchain enumerated in coins.
* **Marker** - A special type of Token represented by a contract that maintains an ownership structure and a collection of assets.

```text
|--------------------|
|       Marker       | 
|  |--------------|  |
|  |     Token    |  |
|  |  |--------|  |  |
|  |  |  Coin  |  |  |
|  |  |        |  |  |
|  |  |  name  |  |  |
|  |  |  0000  |  |  |
|  |  |--------|  |  |
|  |--------------|  |
|--------------------|
```

### Coins

Coins on Provenance Blockchain are a simple structure composed of an integer amount and a string denomination that matches a token name. The coin construct is designed such that any two accounts can freely exchange any whole number fraction of the amount owned by one account to the other through a one way send operation. This operation requires no approval from the receiver of the coin and only a valid signature with a key address matching the address on the account sending the coins.

```text
Coin {
    Denom,      // lowercase string starting with a letter, 3-64 in length [a-z][a-z0-9/]{2,63}
    Amount,     // integer value greater than or equal to zero
}
```

#### Operations

Coins do not support decimals or floats for calculation. This means that the division operations are performed using a special coin structure called a `DecCoin` which uses a `decimal` value for the amount. As the values stored on chain are integers these calculations need to be converted back to a integers using a special truncate process that returns 'change' coins.

```go
// TruncateDecimal returns the coins with truncated decimals and returns the
// change. Note, it will not return any zero-amount coins in either the truncated or
// change coins.
func (coins DecCoins) TruncateDecimal() (truncatedCoins Coins, changeCoins DecCoins) {
    for _, coin := range coins {
        truncated, change := coin.TruncateDecimal()
        if !truncated.IsZero() {
            truncatedCoins = truncatedCoins.Add(truncated)
        }
        if !change.IsZero() {
            changeCoins = changeCoins.Add(change)
        }
    }

    return truncatedCoins, changeCoins
}
```

#### Platform Required Coins

The platform itself requires coins for two different core features; staking and fee payment. The staking aspect of the blockchain imparts ownership and control of the network. A coin with a purpose is known as a `token`.

| Denom | Purpose | Genesis Amount | Description |
| :--- | :--- | :--- | :--- |
| nhash | staking | 1x10^20 \(100B \* 1B\) | One billionth of one hash. High precision to avoid decimals |
| vspn | gas/fees | 100,000,000,000 | Used for paying gas fees within the network |

#### Gas Combustion Cycle

One of the more complex areas of the blockchain is the cycle for gas and fees that flows in through transactions and out to the validators and staking delegators. Every transactions against the blockchain is measured for size and processing cost. This measurement results in a postivie integer called gas. The gas measurement is used to assess fees for processing the transaction and to limit the total amount of work that can be included in a block.

**Gas Consumption Parameters**

The calculation of the amount of gas required for a given transaction is based on a set of parameters that are configured in the parameters of the auth module. There is a fixed cost for the number of bytes in the transaction as well for each signature that is present.

```javascript
// initial values for gas costs in the production Provenance Blockchain
{
    "sig_verify_cost_ed25519": "590",
    "sig_verify_cost_secp256k1": "1000",
    "tx_sig_limit": "7",
    "tx_size_cost_per_byte": "10"
}
```

The overall cost of a transaction is known as a _fee_ and is currently paid in _vspn_ \(**vV**irtual **S**take \(in the\) **P**rovenance **N**etwork\). The conversion between an amount of gas and a fee priced in _vspn_ is set by each Validator in the network independently. Currently a default floor value for the price of gas is set to `0.025vspn`.

_**WARNING: Technically a fee can be paid in any coin denomination or a combination of coin denominations. It is important for the protection of an accounts assets, holdings, and hash stake that only vspn coins are offered for payment.**_

```go
// For a transaction that is 123457 bytes in size with a single secp256k1 signature
gasRequired := (123457 * 10) + (1 * 1000)
// Total fee required in vspn
feeAmount := gasRequired * 0.025 // 30,889.25 or 30,890vspn minimum
```

The fee for a transaction must be provided as a parameter on the first signature \(the payee\) and will be deducted from the account when the transaction is processed. A fee greater than the minimum may be paid to ensure the transaction is not rejected by a validator which may have a higher than minimum gas price set.

**Fee Distribution**

The fees collected for transactions assembled into a block are distributed to the community pool \(at a percentage listed in the distribution module parameters\) and the validator that processed the block. For the validator itself there is a commission percentage that goes to the operator and the remaining amount is distributed to each of the stake holders that have delegated to the validator node at the time the block was cut according to their percentage of the total stake in the validator's pool. The rewards may be with drawn by the delegators at any point after they have been rewarded by issuing the appropriate redemption transaction.

### Token

A token is a construct to represent value using `coins` according to a set of rules. These rules include the total supply, initial ownership, inflation rules, etc. A token's issued `coins` can be transacted like any other coin. They must go through an `exchange` to change denominations. A token is either created by defining a coin denomination and distribution in the genesis block, or through a special `marker` module that provides these features on a running network.

#### Inflation

A token can be configured with a flexible set of parameters governing inflation. These values can be static or adjusted dynamically against a bonded ratio for staking tokens.

| Key | Type | Example |
| :--- | :--- | :--- |
| MintDenom | string | "stake" |
| InflationRateChange | string \(dec\) | "0.13000000" |
| InflationMax | string \(dec\) | "0.20000000" |
| InflationMin | string \(dec\) | "0.07000000" |
| GoalBonded | string \(dec\) | "0.67000000" |
| BlocksPerYear | string \(uint64\) | "6311520" |

_Table: Mint parameters for token inflation._

Coins that are minted through inflation are deposited in the Fee Collection pool and are subject to network wide rules for distribution. For more information on fees and distribution see [ADR-103 Transaction Fees and Gas](adr-103-transaction-fees-gas.md).

#### Total Supply

A marker has a defined overall supply. This is enforced through the use of a blockchain invariant. An invariant check is run on the begin block event by nodes that enable these checks. An invariant check either succeeds or fails. In the case of a total supply invariant if the total number of coins within the network does not match the declared rule the node will panic and halt the chain. Subsequently an administrative action must be taken to restore balance to the chain before normal operation can continue.

#### Mint

A token will typically be created as a set of assigned amounts of coins during the genesis of the blockchain. Once the blockchain has started the coin supply is fixed unless mint or burn operations are performed. The mint operation is a special capability right that is granted to certain modules within the node software. This right is carefully controlled to ensure the stable supplies of the coins in the platform.

#### Burn

Similar to the `mint` operation a `burn` operation can be performed to destroy a number of coins from the overall supply. This operation can only be performed against an amount of coin held by the module account or user that invokes the operation. Coins may not be removed from supply by burning from indiscriminate accounts.

### Marker

The `marker` module is used to construct a `token` based on a declared set of assets provided as collateral. Similar to defining a `token` at genesis, creating a `marker` uses an initial genesis action that defines the rules for the life of the `marker`. The `coin` issued from the `marker`'s `token` represents the fractional ownership in the `marker` itself. Each `marker` instance's token is unique and not interchangeable. This uniqueness is represented with a globally unique coin denomination.

#### Collateral

An instance of a `marker` may be backed with collateral. This collateral is a collection of assets that maybe be native to the blockchain \(`coin`\) or it may be references to assets that exist separately \(metadata `scope`s\). The asset pool is governed by the rules of the `marker` itself. Asset pools may be fixed upon creation or managed to grow and shrink over time.

#### Management

When a `marker` is created the address that submits the transaction is bound as the manager of the marker. While the `marker` is in the `proposed` state the manager address is allowed to submit transactions to modify without further permission checks. After a marker transitions to the `active` state the manager is removed and the normal permission controls are fully enforced.

| Action | Permission | Notes |
| :--- | :--- | :--- |
| Create New Marker | _unrestriced_ | Any user with appropriate gas on an account may create a marker |
| Grant Access | `grant` | Allows adding additional access grants to the marker |
| Increase Supply | `mint` | Allows `mint` \(increase supply\) for an active status marker |
| Decrease Supply | `burn` | Allows `burn` \(decrease supply\) for an active status marker |
| Add Asset to Pool | `deposit` | Allows assignment of a `scope` against the marker \(Note: scope permission checks are separately performed, address must have access to both marker and scope to perform this action\) |
| Withdraw Collateral | `withdraw` | For an active marker allows withdrawing coin or removing assets from pool |
| Finalize | n/a | For a `proposed` status marker, finalizes configuration \(manager only\) |
| Activate | n/a | For a `finalized` status marker, activates configuration \(manager only\) |
| Cancel | `delete` | Move a `finalized`, `active`, or `proposed` marker to a cancelled state |
| Delete | `delete` | Destroys a `canceled` state marker |

```bash
# Access grant 'mint' below is one of [mint,burn,deposit,withdraw,delete,grant]
provenanced tx marker grant tp1ev04582v67phlu4huw046fpaq94ppm3mks9eku cheesedog mint
```

Management functions are restricted to a specific address with a list of permissions, or subject to a **Governance proposal**. A governance proposal is a special construct handled by the `gov` module that takes a submitted configuration proposal and posts it for vote by stake holders. If the vote passes then the proposed changes are applied.

#### Creating Markers

The `marker` module is responsible for creating instances of `markers`. The process for creating a marker can be handled as a single transaction or as a set of many transactions pending the `finalize` call. Any `token` issued by the marker is distributed to the `marker` address when the `marker` is activated. The `marker` instances are designed to be reliable contracts given the exchange of `coin` issued from their `token` offering may be subsequently traded. This targeted reliability means that there are substantial restrictions on the modification of `markers`. An incorrectly created marker may be permanently locked and unusable. This includes the transfer of any assets to the `marker` upon creation, especially any `coin` deposited as an asset.

```bash
# create a new marker with the denomination of 'cheesedog' and a total supply of 1000
provenanced tx marker new 1000cheesedog
```

```bash
# change the status of the marker 'cheesedog' where 'activate' is one of [finalize,activate,cancel,destroy]
provenanced tx marker activate cheesedog
```

#### Structure of a Marker

A marker is identified by a standard account address on chain. Unlike typical addresses that are created by using the 20 bytes from a hash of the public key, the address of a marker is made up of the hash of the denomination of coin the marker will issue.

The marker structure is based on an extended basic account which supports all the features of a normal account except there is no associated key pair which prevents any signing actions by the account. This means that it is not possible to use the traditional signing approach for moving these coins, they must be moved by the `marker` module.

```go
type MarkerAccount struct {
    Address        sdk.AccAddress `json:"address"`
    Coins          sdk.Coins      `json:"coins"`
    PubKey         string         `json:"public_key"`        // NOTE: always empty
    AccountNumber  uint64         `json:"account_number"`
    Sequence       uint64         `json:"sequence"`          // NOTE: always zero
    Manager        sdk.AccAddress `json:"manager,omitempty"` // NOTE: only proposed, finalized states
    AccessControls []AccessGrant  `json:"permissions"`
    Status         MarkerStatus   `json:"status"`
    Denom          string         `json:"denom"`
    Supply         sdk.Int        `json:"total_supply"`
}
```

#### Mint

The `mint` function is used to issue `token` `coin` against the marker. This action is implicitly done for any amount greater than zero for `Supply` when the finalized marker is `activated`. `mint` may also be performed \(as allowed\) post activation. Minted coins are assigned to the marker account itself after which they maybe withdrawn by an authorized user into their own account. Once withdrawn these coins are moved through normal transfer or exchange processes.

#### Burn

An authorized address may "burn" the markers coin, removing it from the global supply coins. The burn function is limited to acting upon coins that are currently held by the marker account itself. For burn operations invoked by a successful governance proposal only coin held in deposit against the marker may be burned. **Note: only `coin` of the current `marker` denom may be burned in any case regardless of what coin denominations are held in the marker's account.**

#### Exchange

A token maybe be exchanged for coins in any other token through the exchange process. 

### Example Uses of Coins, Tokens, and Markers

Coin on the blockchain

#### Omnibus Stable Coin

An omnibus stable coin is a special type of `token` managed against an omnibus bank account. The bank itself \(or its designated integrator\) is responsible for ensuring the supply of the token always matches value of the fiat account balance in its omnibus account.

For this type of configuration the `marker` module is used to issue the `token` on chain. The integrator's account address must configure the marker to reserve the right to mint and burn coin issuances in order to match the account balance changes. An initial supply may be set to reflect the starting account balance. The deposit and withdraw features for assets backing the marker do not apply and should be disabled to prevent their accidental use. The ability to modify the settings of the marker should be reserved by the integrator's account address \(if allowed at all\).

The integrator is responsible for distributing any coin minted to the appropriate depositor. The depositor may exchange these coin freely as they see fit for any purpose that elects to accept the `token` denomination. The integrator is also responsible for providing a facility to accept coin and remit value from the omnibus account to the entity presenting the coin. When a withdraw is made from the omnibus bank account the removed fiat balance must be burned from the supply of coin issued from the marker. As the coin in circulation can not be burned directly, this will require the integrator to ensure that any withdraw has an associated transfer of the coin back to the integrator or marker accounts such that the integrator has control of the coin asset to fill the burn request. This process keeps the account in sync with the value represented on chain.

#### Staking

In the Provenance Blockchain the power to control the network is measured with the amount of stake an account possesses. This stake is represented as a special type of token denominated as `hash`. As the power to control the network is defined up front, the initial allocations of `hash` coins are distributed in the `genesis` process. The terms of the supply are fixed such that there is no need to use the `marker` module to support any kind of on going management of the `token`. The `inflation` concept is not used for `hash` and as such the overall supply will always remain at exactly `100 billion` and can be enforced through an `invariant` constraint. While the specific rules for the `hash` stake token on the Provenance Blockchain are not expected to change, if provisions for this process are desired then the Governance based management control of a `marker` would be the most appropriate configuration.

#### Gas Economy

The Gas economy within the Provenance Blockchain is governed by the use of an arbitrary `token` called `vspn`. This token is defined by a `marker` with a special configuration that does not use assets to back it and makes use of a special administrator account for disbursing allocations. This configuration of a marker uses a configuration that allows the Governance process to update settings as well and perform mint/burn functions. The administrator account address is the target for any coin that is minted/burned against the marker. These coins are then distributed through transfers for exchange out to all of the other accounts which will use them to pay gas fees. The distribution of gas coin can be handled through the Exchange system, or through a "faucet" application/api that dispenses coin to authorized callers. There are several aspects to how fees are subsequently aggregated after being paid and then distributed which are detailed separately in the [Distribution and Exchange ADR-XXX]()

## Decision

Within the Provenance Blockchain the staking and fee purposes are handled using two separate denominations, `hash` for staking and `vspn` for fee payment. Because of the restriction for not using decimals hash will be denominated in nanohash \(`nhash`\).

All units of value on the blockchain will be represented as coin denominations. The rules governing the issue and mangement of these coins will be enforced and curated using the Marker module. The HASH coin used for staking is the only coin not managed through this process as its behavior is defined through the staking process of the blockchain itself.

## Status

**PROPOSED**

## Consequences

> This section describes the resulting context, after applying the decision. All consequences should be listed here, not just the "positive" ones. A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

### Positive

* Holding coins in positive integer amounts associated with addresses will greatly simplify accounting
* Using a separate currency for fees and gas from stake simplifies accounting rules
* Use of the marker module provides a standardized method for managing all tokens on the blockchain similar to the ERC20 standard on Ethereum.

### Negative

* Demarking hash using nano increases the size of hash amount significantly
* Using the standard coin construct of the blockchain for markers may result in an excessive number of coin/token definitions.
* The distributed nature of accounts holding coins limits the ability to rework the structure of a marker once established.

### Neutral

* The marker module will require a separate exchange module to be created to transact value between markers.

## References

