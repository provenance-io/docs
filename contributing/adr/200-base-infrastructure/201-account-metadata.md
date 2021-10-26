---
description: >-
  A structure for collecting key/value metadata attributes against an address
  (account) on the blockchain. Uses the Name Service to control the allowed set
  of Keys.
---

# 201 - Account Metadata

## Changelog

* 30042020: Initial Version

## Context

Accounts on the Provenance Blockchain are based around the concept of holding a collection of coins under an address derived from a public key. All transactions with the blockchain must be associated with an account.

## Decision

* We will define a system to assign exclusive control of names to accounts.  These accounts will be able to create sub names \(reverse DNS style\) which grant control to other accounts using their address.
* We will create a metadata registry of key/value/type data associated with an address.  The data itself will be written and controlled by the account that owns the name used in the key.
* We will create standard query interfaces that allow these keys to be returned as a set for an address and optionally return values that are filtered to a specific key name.

```bash
# Example of a query from the cli for information associated with an address
%%> Provenance Blockchain query account-info provenance1zmfjfu84hxv6vtpr4v0glf6qudmct3y3fahwpz

  address: provenance1zmfjfu84hxv6vtpr4v0glf6qudmct3y3fahwpz
  attributes:
  - name: "accred-investor.passport.provenance.pio"
    value: "{status:\"true\"}"
    type: "json"
  - name: "id.sso.provenance.pio"
    value: "cee2dc96-08fa-450e-ab5c-a6bf549394f3"
    type: "uuid"
  - name: "preferred-currency.coin.provenance.pio"
    value: "HASH"
    type: "string"
  - name: "loan.asset.provenance.pio"
    value: "pb://metadata/scope-bd3564da-4d84-437b-afe0-0d7593fe191f"
    type: "uri"
  - name: "loan.asset.provenance.pio"
    value: "pb://metadata/scope-bcc05e71-2339-4b67-b0e2-6bc1e39842b6"
    type: "uri"
  - name: "loan.asset.provenance.pio"
    value: "pb://metadata/scope-d6717f59-33d3-4f42-8c1d-a44fdfee1bbf"
    type: "uri"
  account_number: 7
  sequence: 24
```

In the above example there are a few points to consider.

### Each of the keys is an owned name.

The `id.sso.provenance.pio` identifier would be owned by an account that delegated control from `root > Provenance Blockchain > sso > id` such that at each level there is an address \(requiring the associated private key to sign a transaction creating the record\). The address may or may not be the same at each level as a parent may create a sub-key and assign its ownership to another address.

### There may be more than one instance of a key.

The `loan.asset.provenance.pio` key appears multiple times in the list. This is allowed so long as the value \(more specifically the hash of the value\) is unique. This allows flexibility in creating a list of assets \(per this example\) or any other type of information which may have multiple instances for a single address.

### The type information is for serialization

Each of the values is associated with some basic type information. This is required for proper serialization. The intent is to support basic types \(int, string\) along with platform specific concepts \(bech32 addresses, uris, json structures, uuids, etc\). Type information can be associated with limits to protect the system. Examples include bounds on integers, length restrictions for strings, and possibly reference checks for blockchain URIs. These limits will be configured using parameters for the handler running in the daemon process.

_NOTE: While something like a protobuf could be included here this presents a challenge as the interpretation of the value then depends on a specification which is not included and thus is not recommended._

## Status

Accepted

## Consequences

The creation of this structure will result in a system with some additional coordination complexity \(assigning keys to the name system entries\) and a potential for abuse with an excessive number of keys added to an account. The end result is a system that would be easy to discover and use both as an api but also through command line and web tools. The delegation of control is flexible and explicit through the name service ownership hierarchy.

### Positive

* Applications can extend the account metadata without modifying or creating a blockchain handler.  All extended metadata is displayed in a common standard interface.
* Applications can be built around well known metadata from an implicitly trusted source associated with an account.

### Negative

* A generic key/value store concept on an address has a high probability of abuse through excessive numbers of values or proliferation of keys
* Account holders are ceding an amount of control to other entities that are making statements about their account without their approval.

### Neutral

{neutral consequences}

## References

