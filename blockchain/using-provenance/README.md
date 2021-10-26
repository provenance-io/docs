---
description: Using the `provenanced` binary as a blockchain client.
---

# Using Provenanced

{% hint style="info" %}
To get started with Provenance Blockchain you first need to install Provenance Blockchain and have access to a Provenance Blockchain node.

[See Installing Provenance Blockchain](../running-a-node/) if you have not installed`provenanced.`

[See Running a Node ](../running-a-node/running-a-node-1/)if you do not have access to a node or do not want to connect to a publicly available node.
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
  init                  Initialize files for a Provenance Blockchain daemon node
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

`--testnet` to use testnet rather than mainnet

`--chain-id pio-testnet-1` assumes that we are connected to the Provenance Blockchain testnet

`--node tcp://localhost:26657` this is the default node location and port. In the examples below, we'll connect to a remote node without starting a local node. The remote node is a public testnet node hosted at `rpc-0.test.provenance.io:26657.`
{% endhint %}

The `provenanced` binary provides a command-line interface to create and query transactions. To create a transaction requires just a few items:

* a key pair capable of signing the transaction
* a key pair capable of paying the gas fee
* a blockchain node to submit the transaction to.

### Creating a Key\(s\)

All interactions with Provenance Blockchain are secured with a public/private key pair that will act as your account\(s\) on the blockchain. We use the `44'/1'/0'/0/0` BIP32 path as an example where the coin-type "1" is a reference to Provenance Blockchain testnet. We use the `44'/505'/0'/0/0` BIP32 path as an example where coin-type "505" is a reference to the HASH token on Provenance Blockchain mainnet.

{% hint style="success" %}
Refer to the [Accounts](../basics/accounts.md) section for more information[ HD Wallet ](../basics/accounts.md#hd-wallet)paths.
{% endhint %}

Create a key pair and store it in a local Keyring \(change `<name-of-key>` to a string value of your choosing\):

```text
# TESTNET (note: --hd-path "44'/1'/0'/0/0" can be used instead of coin type)
provenanced keys add <name_of_key> --coin-type=1 --testnet  -i

# MAINNET (note: the default coin type is 505 for mainnet so no extra parameters are required)
provenanced keys add <name_of_key> -i
```

```text
**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

fancy solar describe long tag soul gold boost vacuum baby famous narrow drink final smoke region pulse wrap expire fabric pause giant merit bird
```

{% hint style="info" %}
When generating a new key it is important to store the generated mnemonic in a safe location to be used in the event that the key needs to be restored.
{% endhint %}

### Restoring a Key

This command will prompt you for a mnemonic to restore the key at a specific path.

```text
# TESTNET
provenanced keys add <name_of_key>  -i --testnet

# MAINNET
provenanced keys add <name_of_key>  -i
```

```text
> Enter your bip39 mnemonic, or hit enter to generate one.
fancy solar describe long tag soul gold boost vacuum baby famous narrow drink final smoke region pulse wrap expire fabric pause giant merit bird
```

```text
# TESTNET
> Enter your bip39 passphrase. This is combined with the mnemonic to derive the seed. Most users should just hit enter to use the default, ""

- name: <name_of_key>
  type: local
  address: tp1gn8jlv9krqe23ql3ltq0ajcwwf7dyaq6uuludl
  pubkey: tppub1addwnpepqd4tpv506lhl3j8hux0ss8km84gwmgapjtuea9wzt8z8n9eqjrjxg897tw0
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

```text
# MAINNET
> Enter your bip39 passphrase. This is combined with the mnemonic to derive the seed. Most users should just hit enter to use the default, ""

- name: <name_of_key>
  type: local
  address: pb1jd9yey59p6e70wp68ufltx8eh2xvxphqwhzcm9
  pubkey: pbpub1addwnpepqfftn6g0p5hs94tp7f4h4d8tkpkjmx87nzxwmmclz89gfdtqfhstv9qd5s8
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

### Getting Hash

Hash is the digital currency used to transact on the Provenance Blockchain. In order to execute any commands beyond basic queries against a node, you'll need Hash. On testnet receiving Hash is as easy as accessing the [Provenance Blockchain Faucet](https://faucet.test.provenance.io/) and supplying your address. This small distribution of Hash on testnet allows you to develop against the public testnet as well as quickly get a feel for how the Provenance Blockchain ecosystem operates.

{% hint style="info" %}
The address associated to your key pair is a [Bech32](https://en.bitcoin.it/wiki/Bech32) address which is an encoded value of the public key portion of our key pair. Provenance Blockchain testnet Bech32 addresses begin with `tp` whereas mainnet addresses begin with `pb`.

Once we transferred Hash to our Bech32 address, it became a [Provenance Blockchain account](../basics/accounts.md).
{% endhint %}

First, find the Bech32 address of the key created in the previous section:

```bash
provenanced --testnet keys show <name-of-key>
```

The output will show the `address` of your key:

```bash
- name: <name-of-key>
  type: local
  address: tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs
  pubkey: tppub1addwnpepqgxlsrgv9znqeass2al9u9h37khapuecpp0al82wqyhzvn9hwtnzwak5uf3
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

Copy the key `address` and open the Provenance Blockchain testnet Hash faucet [https://explorer.test.provenance.io/faucet](https://explorer.test.provenance.io/faucet), paste the value, and press Get Tokens:

![Use the testnet Faucet to get Hash](../../.gitbook/assets/image%20%2811%29.png)

Your `address` will now have enough Hash to pay gas fees. Confirm your key `address` has Hash:

```bash
provenanced --testnet q bank balances tp1cuknswnphchtkwe68t4nshcaj4l4azv9ml2qhs \
 --node=tcp://rpc-0.test.provenance.io:26657
```

```bash
balances:
- amount: "1000000000"
  denom: nhash
pagination:
  next_key: null
  total: "0"
```

{% hint style="info" %}
The `--node` flag allows us to connect our `provenanced` client to a node running remotely. Thus, we're connecting to a public testnet node hosted at `rpc-0.test.provenance.io.`
{% endhint %}

### Using \`jq\` to Parse JSON Output

Using a json parsing tool is often useful for formatting data returned from `provenanced` commands. One way to accomplish this is to use the `jq` command to parse and query json data. For more info on `jq` see [https://stedolan.github.io/jq/download](https://stedolan.github.io/jq/download/).

Example `provenanced` command that uses `jq` to parse the output.

#### Before

```text
provenanced  --testnet q block
```

```text
{"block_id":{"hash":"3B27CC4E15D4022587B06787734254C2D2D14BA79C19493A5561BCCB8CD9C220","parts":{"total":1,"hash":"35A1168D5518D97CC12F09534072A8F688758A11ED37302A1E3FB0AA052FDF86"}},"block":{"header":{"version":{"block":"11"},"chain_id":"pio-testnet-1","height":"26373","time":"2021-03-11T10:59:13.427265301Z","last_block_id":{"hash":"EB40B2392FA71D21A405263A9C26E48EDD23A1AF30D5440B040C2276A1A64959","parts":{"total":1,"hash":"441E673DE088DE1916C590499716227515600D4B55323E8BD3EAFAAB1614C2BA"}},"last_commit_hash":"07FE26AEB1739B6287E698DF89D8BE563C8E59C9F4D9EC6F2F5B696C17BC8989","data_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","validators_hash":"D99B22042480A642560299B08DEB4E2F5597145B7EF8CFBDA7A15235E7CE30FD","next_validators_hash":"D99B22042480A642560299B08DEB4E2F5597145B7EF8CFBDA7A15235E7CE30FD","consensus_hash":"048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F","app_hash":"E9ADB51FB9BBAF83083E82F41C0D948221268B372FD0DE574CFC522CB6E2D27B","last_results_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","evidence_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","proposer_address":"4CC7186D6C520A9EF696A6A0D6802564593D7380"},"data":{"txs":null},"evidence":{"evidence":null},"last_commit":{"height":"26372","round":0,"block_id":{"hash":"EB40B2392FA71D21A405263A9C26E48EDD23A1AF30D5440B040C2276A1A64959","parts":{"total":1,"hash":"441E673DE088DE1916C590499716227515600D4B55323E8BD3EAFAAB1614C2BA"}},"signatures":[{"block_id_flag":2,"validator_address":"4CC7186D6C520A9EF696A6A0D6802564593D7380","timestamp":"2021-03-11T10:59:13.443821923Z","signature":"ct9KWn804qYTu3QkoSWaLaXxB5ZkgGHV+iLA3v+p3R5Vqro9+hqfr49H8RaO5M2ENaYZpeJZELRShUWdKiztBg=="},{"block_id_flag":2,"validator_address":"7100D18B251B4AB281A26BF64C81B20BABD77390","timestamp":"2021-03-11T10:59:13.427265301Z","signature":"uesfDwmY8egrBNNc7H130jnprNakHRDfTQFVmpbUSHkUDMYvT+CCRPk87Gn7ew6F7qSktUBRRp3Y+1AESxxLBQ=="},{"block_id_flag":2,"validator_address":"CF53B691AFA2EB28C3D2AE118EF8F88FC48459BC","timestamp":"2021-03-11T10:59:13.425947442Z","signature":"jA0+R+y4tzpov4qPEHhOOI1rsm/uma7qukpTEPYA5VLMcnvd93VATuehWmf/R5r95rkWwP46ZUUjoj+U7OBfCA=="},{"block_id_flag":2,"validator_address":"E354428AB16927EBBC3AC4036D8D25B59A47495B","timestamp":"2021-03-11T10:59:13.461404024Z","signature":"7+Rvqw9guBF/ET0TQwc1TRZGzqG0iIcr4/aKQIJ8+T5/dJlFQsn1uktaTpg/jLa4zKoTqXDZm8jUiuLPZUVyCg=="}]}}}
```

#### After

```text
provenanced  --testnet q block | jq ".block.last_commit.height"
```

```text
"25923"
```

