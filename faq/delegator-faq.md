---
description: Frequently asked questions about delegators.
---

# Delegator FAQ

## What is a delegator?

People that cannot or do not want to operate validator nodes can still participate in the staking process as delegators. Indeed, validators are not chosen based on their self-delegated stake but based on their total stake, which is the sum of their self-delegated stake and of the stake that is delegated to them. This is an important property, as it makes delegators a safeguard against validators that exhibit bad behavior. If a validator misbehaves, their delegators will move their Hash away from them, thereby reducing their stake. Eventually, if a validator's stake falls under the top 125 addresses with the highest stake, they will exit the validator set.

Delegators share the revenue of their validators, but they also share the risks. In terms of revenue, validators and delegators differ in that validators can apply a commission on the revenue that goes to their delegator before it is distributed. This commission is known to delegators beforehand and can only change according to predefined constraints \(see the [section](https://app.gitbook.com/@provenance/s/provenance-docs/~/drafts/-MW0u33Y0GlSHBhBNdTo/faq/delegator-faq/@drafts#validator-commission) below\). In terms of risk, delegators' Hash can be slashed if their validator misbehaves. For more, see the [Risks ](https://app.gitbook.com/@provenance/s/provenance-docs/~/drafts/-MW0u33Y0GlSHBhBNdTo/faq/delegator-faq/@drafts#risks)section.

To become delegators, Hash holders need to send a "Delegate transaction" where they specify how many Hash they want to bond and to which validator. A list of validator candidates will be displayed in Provenance Blockchain explorers. Later, if a delegator wants to unbond part or all of their stake, they need to send an "Unbond transaction". From there, the delegator will have to wait three weeks to retrieve their Hash. Delegators can also send a "Rebond Transaction" to switch from one validator to another, without having to go through the three weeks waiting period.

## Choosing a validator

To choose their validators, delegators have access to a range of information directly in any Provenance Blockchain block explorer.

* Validator's moniker: Name of the validator candidate.
* Validator's description: Description provided by the validator operator. 
* Validator's website: Link to the validator's website. 
* Initial commission rate: The commission rate on revenue charged to any delegator by the validator \(see below for more detail\). 
* Commission max change rate: The maximum daily increase of the validator's commission. This parameter cannot be changed by the validator operator. 
* Maximum commission: The maximum commission rate this validator candidate can charge. This parameter cannot be changed by the validator operator. 
* Minimum self-bond amount: Minimum amount of Hash the validator candidate needs to have bonded at all time. If the validator's self-bonded stake falls below this limit, their entire staking pool \(i.e. all its delegators\) will unbond. This parameter exists as a safeguard for delegators. Indeed, when a validator misbehaves, part of their total stake gets slashed. This included the validator's self-delegated stake as well as their delegators' stake. Thus, a validator with a high amount of self-delegated Hash has more skin-in-the-game than a validator with a low amount. The minimum self-bond amount parameter guarantees to delegators that a validator will never fall below a certain amount of self-bonded stake, thereby ensuring a minimum level of skin-in-the-game. This parameter can only be increased by the validator operator.

## Directives of delegators

Being a delegator is not a passive task. Here are the main directives of a delegator:

* Perform careful due diligence on validators before delegating. If a validator misbehaves, part of their total stake, which includes the stake of their delegators, can be slashed. Delegators should therefore carefully select validators they think will behave correctly.
* Actively monitor their validator after having delegated. Delegators should ensure that the validators to whom they delegate behave correctly. Meaning that they have good uptime, do not double sign or get compromised, and participate in governance. They should also monitor the commission rate that is applied. If a delegator is not satisfied with its validator, they can unbond or switch to another validator \(Note: Delegators do not have to wait for the unbonding period to switch validators. Rebonding takes effect immediately\).
* Participate in governance. Delegators can and are expected to actively participate in governance. A delegator's voting power is proportional to the size of their bonded stake. If a delegator does not vote, they will inherit the vote of their validator\(s\). If they do vote, they override the vote of their validator\(s\). Delegators, therefore, act as a counterbalance to their validators.

## Revenue

Validators and delegators earn revenue in exchange for their services using transaction fees. Every transaction on Provenance Blockchain incurs a transaction fee \(refer to [Gas and Fees section](../blockchain/basics/gas-and-fees.md)\). These fees can be paid in any currency that is whitelisted by Provenance Blockchain's governance, with Hash being the current default currency. Collected transaction fees are pooled globally and distributed to bonded Hash holders in proportion to their stake.

## Validator Commission

Each validator receives revenue based on their total stake. Before this revenue is distributed to delegators, the validator can apply a commission. In other words, delegators have to pay a commission to their validators on the revenue they earn. Let's look at an example:

We consider a validator whose stake \(i.e. self-delegated stake + delegated stake\) is 10% of the total stake of all validators. This validator has a 20% self-delegated stake and applies a commission of 10%. Now let us consider a block with the following revenue:

* 990 Hash in block provisions
* 10 Hash in transaction fees.

This amounts to a total of 1000 Hash to be distributed among all staking pools.

Our validator's staking pool represents 10% of the total stake, which means the pool obtains 100 Hash. Now let us look at the internal distribution of revenue:

* Commission = `10% * 80% * 100` Hash = 8 Hash
* Validator's revenue = `(20% * 100` Hash\) + Commission = 28 Hash
* Delegators' total revenue = `(80% * 100` Hash\) - Commission = 72 Hash

Then, each delegator in the staking pool can claim their portion of the delegators' total revenue.

## Risks

Staking Hash is not free of risk. First, staked Hash are locked up, and retrieving them requires a three-week waiting period called the unbonding period. Additionally, if a validator misbehaves, a portion of their total stake can be slashed \(i.e. destroyed\). This includes the stake of their delegators.

There is one main slashing condition.

* Double signing: If someone reports that a validator signed two different blocks with the same chain ID at the same height, this validator will get slashed.

This is why Hash holders should perform careful due diligence on validators before delegating. It is also important that delegators actively monitor the activity of their validators. If a validator behaves suspiciously or is too often offline, delegators can choose to unbond from them or switch to another validator.  Delegators can also mitigate risk by distributing their stake across multiple validators.

## How To Stake Your Hash 

{% hint style="info" %}
Please take a moment to thoroughly understand [how delegation and staking work](delegator-faq.md#what-is-a-delegator) along with the [risks](delegator-faq.md#risks) outlined in the previous section.
{% endhint %}

### Staking Using the Provenance Blockchain Explorer

The easiest way to stake your Hash to a [Validator](validator-faq.md) is by using the [Provenance Blockchain Explorer.](https://explorer.provenance.io)

#### Connect Your Wallet to Explorer

If you have a Provenance Blockchain or Figure Wallet, navigate to the Provenance Blockchain Explorer and click the "Connect Wallet" key in the upper right hand corner and select your wallet type:

![Select a Wallet Type to Connect](../.gitbook/assets/image%20%2822%29.png)

Once your wallet is connected, the Explorer will show the wallet address is in use and the Key will change to a User profile icon:

![](../.gitbook/assets/image%20%287%29.png)

With your wallet connected, click the **Staking** menu option to display the list of Validators:

![Available Validators List](../.gitbook/assets/image%20%2817%29.png)

The Validator list shows the Validators that are available to Delegate to.  Clicking the `Moniker` or `Address` column for the Validator will show details about the Validator.  These details are important when considering a Validator as they demonstrate the Validator's shares, commissions, and delegators:

![](../.gitbook/assets/image%20%2820%29.png)

From the Validator List, click the **Delegate** button next to the Validator you wish to stake with:

![Click Delegate to Stake Hash with Selected Validator](../.gitbook/assets/image%20%2819%29.png)

Explorer will display important information about the selected Validator and provide an input to enter the amount of Hash you wish to delegate:

![](../.gitbook/assets/image%20%2812%29.png)

Once the **Delegate** button is clicked, Explorer will prompt you sign the transaction with your wallet.  Once signed and submitted, the delegation will show in the Provenance Blockchain Validators List:

![](../.gitbook/assets/image%20%2821%29.png)

### How to Manage Your Delegations from Explorer

Once you have delegated Hash to a Validator you can undelegate or redelegate your Hash.  When you undelegate, your Hash will not be available for 21 days.  You can redelegate to a different Validator at any time.

From the **Staking** menu the My Validators list shows the Validators you have delegated to.  Click the **Manage** button to adjust your delegations:

![](../.gitbook/assets/image%20%288%29.png)

{% hint style="info" %}
Your rewards for a given validator are immediately claimed when you change your delegation against it, which is reflected in the transaction log.
{% endhint %}



