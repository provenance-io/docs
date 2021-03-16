---
description: Tutorials to jump start using Provenance
---

# Using Provenanced

{% hint style="info" %}
To get started with Provenance you first need to install Provenance and have access to a Provenance node.

[See Installing Provenance](../running-a-node/) if you have not installed`provenanced.`

[See Running a Node ](../running-a-node/running-a-node-1/)if you do not have access to a node.
{% endhint %}

## Usage

```text
~$ provenanced --help
Provenance Blockchain App

Usage:
  provenanced [command]

Available Commands:


  add-genesis-account   Add a genesis account to genesis.json
  add-genesis-marker    Adds a marker to genesis.json
  add-genesis-root-name Add a name binding to genesis.json
  collect-gentxs        Collect genesis txs and output a genesis.json file
  debug                 Tool for helping with debugging your application
  export                Export state to JSON
  gentx                 Generate a genesis tx carrying a self delegation
  help                  Help about any command
  init                  Initialize files for a provenance daemon node
  keys                  Manage your application's keys
  migrate               Migrate genesis to a specified target version
  query                 Querying subcommands
  start                 Run the full node
  status                Query remote node for status
  tendermint            Tendermint subcommands
  testnet               Initialize files for a simapp testnet
  tx                    Transactions subcommands
  unsafe-reset-all      Resets the blockchain database, removes address book files, and resets data/priv_validator_state.json to the genesis state
  validate-genesis      validates the genesis file at the default location or at the location passed as an arg
  version               Print the application binary version information

Flags:
  -h, --help                help for provenanced
      --home string         directory for config and data (default "/Users/mconroy/Library/Application Support/Provenance")
      --log_format string   The logging format (json|plain) (default "plain")
      --log_level string    The logging level (trace|debug|info|warn|error|fatal|panic) (default "info")
  -t, --testnet             Indicates this command should use the testnet configuration (default: false [mainnet])
      --trace               print out full stack trace on errors

Use "provenanced [command] --help" for more information about a command.
```

{% hint style="info" %}
Commands used throughout these examples will use some consistent flags that are worth noting. 

`-t` to use testnet rather than mainnet

`--chain-id pio-testnet-1` assumes that we are connected to the Provenance testnet 

`--node tcp://localhost:26657` this is the default node location and port but can be specified
{% endhint %}

### Creating a Key Chain

All interactions with Provenance are secured with a public/private key pair that will act as your account\(s\) on the blockchain. 

### Getting Hash 

Hash is the digital currency used to transact on the Provenance blockchain. In order to execute any commands beyond basic queries against a node, you'll need hash. On testnet receiving Hash is as easy as accessing the [Provenance Faucet](https://faucet.test.provenance.io/) and supplying your address. This small distribution of Hash on testnet allows you to develop against the public testnet as well as quickly get a feel for how the Provenance ecosystem operates.

### Using \`jq\` to Parse JSON Output

Using a json parsing tool is often useful for formatting data returned from `provenanced` commands. One way to accomplish this is to use the `jq` command to parse and query json data. For more info on `jq` see [https://stedolan.github.io/jq/download](https://stedolan.github.io/jq/download/).

Example `provenanced` command that uses `jq` to parse the output.

#### Before

```text
~$ provenanced  -t q block
{"block_id":{"hash":"3B27CC4E15D4022587B06787734254C2D2D14BA79C19493A5561BCCB8CD9C220","parts":{"total":1,"hash":"35A1168D5518D97CC12F09534072A8F688758A11ED37302A1E3FB0AA052FDF86"}},"block":{"header":{"version":{"block":"11"},"chain_id":"pio-testnet-1","height":"26373","time":"2021-03-11T10:59:13.427265301Z","last_block_id":{"hash":"EB40B2392FA71D21A405263A9C26E48EDD23A1AF30D5440B040C2276A1A64959","parts":{"total":1,"hash":"441E673DE088DE1916C590499716227515600D4B55323E8BD3EAFAAB1614C2BA"}},"last_commit_hash":"07FE26AEB1739B6287E698DF89D8BE563C8E59C9F4D9EC6F2F5B696C17BC8989","data_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","validators_hash":"D99B22042480A642560299B08DEB4E2F5597145B7EF8CFBDA7A15235E7CE30FD","next_validators_hash":"D99B22042480A642560299B08DEB4E2F5597145B7EF8CFBDA7A15235E7CE30FD","consensus_hash":"048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F","app_hash":"E9ADB51FB9BBAF83083E82F41C0D948221268B372FD0DE574CFC522CB6E2D27B","last_results_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","evidence_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","proposer_address":"4CC7186D6C520A9EF696A6A0D6802564593D7380"},"data":{"txs":null},"evidence":{"evidence":null},"last_commit":{"height":"26372","round":0,"block_id":{"hash":"EB40B2392FA71D21A405263A9C26E48EDD23A1AF30D5440B040C2276A1A64959","parts":{"total":1,"hash":"441E673DE088DE1916C590499716227515600D4B55323E8BD3EAFAAB1614C2BA"}},"signatures":[{"block_id_flag":2,"validator_address":"4CC7186D6C520A9EF696A6A0D6802564593D7380","timestamp":"2021-03-11T10:59:13.443821923Z","signature":"ct9KWn804qYTu3QkoSWaLaXxB5ZkgGHV+iLA3v+p3R5Vqro9+hqfr49H8RaO5M2ENaYZpeJZELRShUWdKiztBg=="},{"block_id_flag":2,"validator_address":"7100D18B251B4AB281A26BF64C81B20BABD77390","timestamp":"2021-03-11T10:59:13.427265301Z","signature":"uesfDwmY8egrBNNc7H130jnprNakHRDfTQFVmpbUSHkUDMYvT+CCRPk87Gn7ew6F7qSktUBRRp3Y+1AESxxLBQ=="},{"block_id_flag":2,"validator_address":"CF53B691AFA2EB28C3D2AE118EF8F88FC48459BC","timestamp":"2021-03-11T10:59:13.425947442Z","signature":"jA0+R+y4tzpov4qPEHhOOI1rsm/uma7qukpTEPYA5VLMcnvd93VATuehWmf/R5r95rkWwP46ZUUjoj+U7OBfCA=="},{"block_id_flag":2,"validator_address":"E354428AB16927EBBC3AC4036D8D25B59A47495B","timestamp":"2021-03-11T10:59:13.461404024Z","signature":"7+Rvqw9guBF/ET0TQwc1TRZGzqG0iIcr4/aKQIJ8+T5/dJlFQsn1uktaTpg/jLa4zKoTqXDZm8jUiuLPZUVyCg=="}]}}}
```

#### After

```text
~$ provenanced  -t q block | jq ".block.last_commit.height"
"25923"
```


