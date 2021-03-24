---
description: Understanding the Provenance accounts system.
---

# Accounts

On Provenance an [_account_](https://docs.cosmos.network/v0.41/basics/accounts.html) designates a pair of _public key_ `PubKey` and _private key_ `PrivKey`. The `PubKey` can be derived to generate various `Addresses`, which are used to identify users \(among other parties\) in the application. `Addresses` are also associated with [`message`s](https://docs.cosmos.network/master/building-modules/messages-and-queries.html#messages) to identify the sender of the `message`. The `PrivKey` is used to generate [digital signatures](https://docs.cosmos.network/master/basics/query-lifecycle.html#signatures) to prove that an `Address` associated with the `PrivKey` approved of a given `message`.

{% hint style="info" %}
In it's simplest form, a Provenance account is simply a Bech32 address derived from the public key in a key pair where the address holds Hash.  A key pair that exists outside of Provenance \(say in a wallet\) is not a Provenance account until Hash has been transferred to the key pair address, either by a direct transfer or purchased on an exchange, on the Provenance blockchain.
{% endhint %}



