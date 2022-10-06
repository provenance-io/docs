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

### Normalization

Name records are normalized before being processed for creation or query.  Each component of the name must conform to a standard set of rules.  The sha256 of the normalized value is used internally for comparision purposes.

1. Names are always stored and compared using a lower case form or a hash derived from this normalized form.
2. Unicode values that are not graphic, lower case, or digits are considered invalid.
3. A single occurance of the hyphen-minus character is allowed unless the value conforms to a valid UUID.
```value: -
HYPHEN-MINUS
Unicode: U+002D, UTF-8: 2D
```
4. Each component of the name is subject to a length of 2 to 32 characters (inclusive). These limits are configurable in the module parameters.
5. A maximum of 16 components for a name (levels in the heirarchy) is also enforced and configurable in the module parameters.
6. Leading and trailing spaces are always trimmed off of names for consistency during processing and evaluation.

### Creation of Root Names

As every name hierarchy depends on the name above it for permissioning and control, the root names present a problem with no parent to enforce their management.  Because of this inception problem root names must be created in the genesis of the blockchain or through a governance proposal process.

