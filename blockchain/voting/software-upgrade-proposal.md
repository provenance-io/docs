---
description: 'Information on how to propose, vote, and track a software upgrade proposal'
---

# Software Upgrade Proposal

Software upgrade proposals will occur when major upgrades are required on the provenance network. When this does occur a governance proposal will be made to request all validators vote on the new software upgrade. All software upgrades will be sourced from the [provenance-io/provenance](https://github.com/provenance-io/provenance) repository.

### Submit a software upgrade proposal

Once a new release for the provenance binary has been created, a software upgrade proposal can be run. The proposal should have a name, title, a description of the changes, a url of the plan with the necessary binaries, upgrade block height, required deposit, and chain-id

```text
export PIO_HOME=~/.provenanced
provenanced tx gov submit-proposal software-upgrade test1 \
--title "test1" \
--description "upgrade provenance to version test1" \
--upgrade-info  https://github.com/provenance-io/provenance/releases/download/test1/plan-test1.json\
--from <name_of_key> \
--upgrade-height 1000 \
--deposit 10000000nhash \
--chain-id pio-testnet-1 \
--testnet
```

### Reviewing the newly created proposal

After submitting the software upgrade proposal you can review that proposal by running the following command:

```text
provenanced query gov proposal 1 --testing
```

```text
content:
  '@type': /cosmos.upgrade.v1beta1.SoftwareUpgradeProposal
  description: upgrade provenance to version test1
  plan:
    height: "1000"
    info: https://github.com/provenance-io/provenance/releases/download/test1/plan-test1.json
    name: test1
    time: "0001-01-01T00:00:00Z"
    upgraded_client_state: null
  title: test1
deposit_end_time: "2021-03-19T15:41:31.171271855Z"
final_tally_result:
  abstain: "0"
  "no": "0"
  no_with_veto: "0"
  "yes": "0"
proposal_id: "1"
status: PROPOSAL_STATUS_VOTING_PERIOD
submit_time: "2021-03-17T15:41:31.171271855Z"
total_deposit:
- amount: "10000000"
  denom: nhash
voting_end_time: "2021-03-17T15:46:31.171271855Z"
voting_start_time: "2021-03-17T15:41:31.171271855Z"
```

This shows that the proposal is live on the chain and the voting period has begun. 

### Voting on the Proposal

At this time the proposal is live on the provenance network and in order to pass will require a greater than 2/3 majority `yes` vote. Voting has 4 different parameters `abstain, no, no_with_veto, and yes`. To vote on the proposal you will run the following:

```text
provenanced tx gov vote 1 yes \
--from $KEY_NAME \
--chain-id pio-testnet-1 \
--fees 1395nhash \
--gas auto \
--testing
```

### Tally current vote 

You can review the current state of the governance proposal by running the following:

```text
provenanced query gov tally 1 --testing
```

```text
abstain: "0"
"no": "0"
no_with_veto: "0"
"yes": "0"
```

### End of Voting Period

Once the voting period has expired the votes will be tallied and if the proposal was voted on in the affirmative it will pass and take affect on the agreed upon block height.

```text
provenanced query gov proposal 1 --testing
```

```text
content:
  '@type': /cosmos.upgrade.v1beta1.SoftwareUpgradeProposal
  description: upgrade provenance to version test1
  plan:
    height: "1000"
    info: https://github.com/provenance-io/provenance/releases/download/test1/plan-test1.json
    name: test1
    time: "0001-01-01T00:00:00Z"
    upgraded_client_state: null
  title: test1
deposit_end_time: "2021-03-19T15:41:31.171271855Z"
final_tally_result:
  abstain: "0"
  "no": "0"
  no_with_veto: "0"
  "yes": "10000000"
proposal_id: "1"
status: PROPOSAL_STATUS_VOTING_PASSED
submit_time: "2021-03-17T15:41:31.171271855Z"
total_deposit:
- amount: "10000000"
  denom: nhash
voting_end_time: "2021-03-17T15:46:31.171271855Z"
voting_start_time: "2021-03-17T15:41:31.171271855Z"
```

