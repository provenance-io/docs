# Governance

## Overview <a id="Governance-Proposal-Types"></a>

Provenance Blockchain includes an on-chain governance mechanism for managing software updates and improvements, as well as for governing the use of the Provenance community funds. Users holding staked Hash tokens can participate in voting on governance proposals.

## Governance Proposal Types <a id="Governance-Proposal-Types"></a>

### Basic Proposals <a id="Basic-Proposals"></a>

* **Community Spend** – a proposal for use of community funds, together with how many coins are proposed to be spent, and to which recipient account.
* **Text** – a standard text proposal whose changes need to be manually updated in case of approval. Can be a simple document that is put to vote by the stake holders. An example from Cosmos would be the ADR \(architecture decision records\) which are voted in to show endorsement of high-impact changes or development initiatives.
* **Parameter Change** defines a proposal to change one or more parameters \(see list of parameters below\).
* **Software Upgrade** is a proposal for initiating a software upgrade. Include the specific version of the software to run and the blockheight when it takes effect. Used for coordinated network upgrades.
* **Client Update \(IBC\)** the client is updated with the provided header. The update may fail if the header is not valid given certain conditions specified by the client implementation. A client is the interface between blockchains for IBC.

### Smart Contract Governance Proposals <a id="Smart-Contract-Governance-Proposals"></a>

* **Store Code** - upload a smart contract build
* **Instantiate Contract** - instantiate a smart contract
* **Migrate Contract** - migrate a smart contract to a new code version
* **Update Admin** - set a new admin for a smart contract
* **Clear Admin** - remove admin from a smart contract to prevent further migrations

### Proposal Statuses <a id="Proposal-Statuses"></a>

```text
enum ProposalStatus {
  option (gogoproto.goproto_enum_prefix) = false;

  // PROPOSAL_STATUS_UNSPECIFIED defines the default propopsal status.
  PROPOSAL_STATUS_UNSPECIFIED = 0 [(gogoproto.enumvalue_customname) = "StatusNil"];
  // PROPOSAL_STATUS_DEPOSIT_PERIOD defines a proposal status during the deposit
  // period.
  PROPOSAL_STATUS_DEPOSIT_PERIOD = 1 [(gogoproto.enumvalue_customname) = "StatusDepositPeriod"];
  // PROPOSAL_STATUS_VOTING_PERIOD defines a proposal status during the voting
  // period.
  PROPOSAL_STATUS_VOTING_PERIOD = 2 [(gogoproto.enumvalue_customname) = "StatusVotingPeriod"];
  // PROPOSAL_STATUS_PASSED defines a proposal status of a proposal that has
  // passed.
  PROPOSAL_STATUS_PASSED = 3 [(gogoproto.enumvalue_customname) = "StatusPassed"];
  // PROPOSAL_STATUS_REJECTED defines a proposal status of a proposal that has
  // been rejected.
  PROPOSAL_STATUS_REJECTED = 4 [(gogoproto.enumvalue_customname) = "StatusRejected"];
  // PROPOSAL_STATUS_FAILED defines a proposal status of a proposal that has
  // failed.
  PROPOSAL_STATUS_FAILED = 5 [(gogoproto.enumvalue_customname) = "StatusFailed"];
}
```

## Blockchain Parameters <a id="Blockchain-Parameters"></a>

The following list of parameters exist in the blockchain and can be adjusted through governance parameter proposals

### Auth <a id="Auth"></a>

These parameters control the gas rates used for the network as well as some limits on the size of transactions and transaction metadata.

* Max memo field characters
* Maximum number of message signatures
* Transaction gas cost
  * per byte
  * per ed25519 signature
  * per secp256k1 signature

### Bank <a id="Bank"></a>

These parameters control if accounts are allowed to send certain denominations of coin \(or send any coins at all\).

* Send Enabled \(general for all coin\)
* Send Enabled \(specific coin\)

### Distribution <a id="Distribution"></a>

These parameter cover the distribution of gas fees paid into the network by users. The community tax rate is the amount of gas fees paid to take off the top and deposit into a network pool of funds that can be spent via governance proposal vote. Bonus rates are the amount of extra allocation given to the validator node when it is selected to cut the current block. The Bonus rate is an extra allocation given if the validator node collects signatures from all of the other active validators. This balances the cut early/wait for others in the network incentive.

* Community Tax Rate
* Base Proposer Bonus Rate
* Bonus Proposer Bonus Rate

### Mint <a id="Mint"></a>

The minting module controls inflation parameters for the staking currency. The inflation rate gives an annual yield for actively staked tokens within the network.

* Denomination to Mint
* Inflation
  * Annual Rate of Change
  * Maximum Rate
  * Minimum Rate
* Bonded Token Target Percentage
* Estimated Blocks per Year

### Slashing <a id="Slashing"></a>

These settings control the penalties assessed as a fraction of the delegated stake held by a validator. Window size and number of blocks required per window control the uptime requirements for an active validator.

* Minimum signed blocks per Window
* Window Size
* Double Sign Penalty Fraction
* Downtime Penalty Fraction

### Staking <a id="Staking"></a>

These settings control which currency is used for staking in the network as well as the number of total active validators allowed in the network. The unbonding period controls how long it takes for a bonded delegation to be released. This value must be long enough to keep stake at risk for slashing when bad behavior is detected \(21 days should be used presently\)

* Maximum Number of Active Validators
* Unbonding Period
* Max Entries
* Historical Entries
* Staking Currency Denomination

### Transfer \(IBC\) <a id="Transfer-IBC"></a>

Controls if coins can flow into the network or out of the network over IBC. For control of specific coins the transfer parameters on the bank module are used to override the global settings defined here.

* Send Enabled – sets global send enable for all denominations over IBC
* Receive Enabled – sets the global receive enabled for all denominations over IBC.

### Smart Contracts <a id="Smart-Contracts"></a>

There are a few parameters that can be set related to smart contracts running on the blockchain. These settings are support through a standard parameter change proposal and are in addition to the other governance proposals that are specific to the smart contracts module.

* Code Upload Access - who can upload a smart contract binary: Nobody, Everybody, OnlyAddress
* Instantiate Default Permission - platform default, who can instantiate a wasm binary when the code owner has not set it

