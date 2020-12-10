---
description: >-
  Name service that provides a hierarchical registry of names similar to the DNS
  and Ethereum Name Service.
---

# 200 - Name Service

## Changelog

* 20200202: Initial version

## Context

The name service is intended to provide a system for creating human-readable names as aliases for addresses and to imply ownership and control. These names can be used to provide a stable reference to a changing address or collection of addresses.

One issue with a blockchain is that addresses are complex strings of characters that are difficult to type and remember. On the other hand the name service can provide a potentially shorter and easier to remember alias such as `provenance.pb` or `attribute.user.pb` to use in place of the address.

### A Name Hierarchy

Another challenge for users of a blockchain is establishing authority and delegating control. A specific example of this is the definition of the authoritative source of a piece of information. Where did this information come from? Who created it/vetted it? How can this control be distributed in such a way that the right people can control the information? A narrow aspect of this type of control can be satisfied through the creation of a hierarchical name system modeled after DNS. If the address `passport.pb` has been created and is owned by the Provenance Passport application, then `level-3.accredited.passport.pb` can be expected to be under the direct or delegated control of the passport application.

### Delegating Control

Every label in a name is owned by an address. Starting from the root address each level can be configured to allow any user to add a new child or for the exclusive control of the creator to add child names. The `Restricted` flag is used to indicate the permission requirements for adding child nodes.

```go
// NameRecord is an address with a flag that indicates if the name is restricted
type NameRecord struct {
    // The bound name
    Name string `json:"name" yaml:"name"`
    // The address the name resolved to.
    Address sdk.AccAddress `json:"address" yaml:"address"`
    // Whether owner signature is required to add sub-names.
    Restricted bool `json:"restricted,omitempty" yaml:"restricted"`
}
```

## Decision

The implementation of a name service will be created to satisfy the stated requirements.

## Status

Accepted

## Consequences

The creation of this structure will result in a system with some additional coordination complexity \(assigning keys to the name system entries\) and a potential for abuse with an excessive number of keys added to an account. The end result is a system that would be easy to discover and use both as an api but also through command line and web tools. The delegation of control is flexible and explicit through the name service ownership hierarchy.

### Positive

* This implementation adds an easy to remember naming convention to associate complex addresses with a friendly name \(i.e. `provenance1zmfjfu84hxv6vtpr4v0glf6qudmct3y3fahwpz` to `example.pio`\)
* The hierarchical names provide a logical and easy to understand structure that can be used for delegation of control.

### Negative

* The name service may be abused with excessive amounts of non-useful names.
* An address may be unintentionally published in the address mapping revealing its use to the public with a loss of privacy.

### Neutral

n/a

## References

An Ethereum implementation of a name service contract that supports delegation of sub keys to accounts

\[ENS - Ethereum Name Service\]: [https://app.ens.domains](https://app.ens.domains)

