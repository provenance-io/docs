---
description: >-
  The Name module provides a hierarchical structure of canonical names
  associated with blockchain addresses.
---

# Name

## Overview

The name service is intended to provide a system for creating human-readable names as aliases for addresses and to imply ownership and control. These names can be used to provide a stable reference to a changing address or collection of addresses.

One issue with a blockchain is that addresses are complex strings of characters that are difficult to type and remember. On the other hand the name service can provide a potentially shorter and easier to remember alias such as `provenance.pb` or `attribute.user.pb` to use in place of the address.

### A Name Hierarchy

Another challenge for users of a blockchain is establishing authority and delegating control. A specific example of this is the definition of the authoritative source of a piece of information. Where did this information come from? Who created it/vetted it? How can this control be distributed in such a way that the right people can control the information? A narrow aspect of this type of control can be satisfied through the creation of a hierarchical name system modeled after DNS. If the address `passport.pb` has been created and is owned by the Provenance Blockchain Passport application, then `level-3.accredited.passport.pb` can be expected to be under the direct or delegated control of the passport application.

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

### Creation of Root Names

As every name hierarchy depends on the name above it for permissioning and control, the root names present a problem with no parent to enforce their management.  Because of this inception problem root names must be created in the genesis of the blockchain or through a governance proposal process.

