---
description: >-
  A module that stores attribute values against addresses/accounts.  Keys are
  managed by the Name module.
---

# Attribute

## Overview

Accounts on the Provenance Blockchain are based around the concept of holding a collection of coins under an address derived from a public key. All transactions with the blockchain must be associated with an account.  The Attribute module defines a registry of key/value/type data associated with an address where the data itself is written and controlled by the account that owns the name used in the key.

### Attribute Structure

The Attribute structure is simply a key/value structure used to assocate a value to an address.

```go
// Attribute holds a typed key/value structure for data associated with an account
type Attribute struct {
	// The attribute name.
	Name string `protobuf:"bytes,1,opt,name=name,proto3" json:"name,omitempty"`
	// The attribute value.
	Value []byte `protobuf:"bytes,2,opt,name=value,proto3" json:"value,omitempty"`
	// The attribute value type.
	AttributeType AttributeType `protobuf:"varint,3,opt,name=attribute_type,json=attributeType,proto3,enum=provenance.attribute.v1.AttributeType" json:"attribute_type,omitempty"`
	// The address the attribute is bound to
	Address string `protobuf:"bytes,4,opt,name=address,proto3" json:"address,omitempty"`
}
```

#### Attribute Types

Attributes associated to an address are typed:

```go
// AttributeType defines the type of the data stored in the attribute value
type AttributeType int32

const (
	// ATTRIBUTE_TYPE_UNSPECIFIED defines an unknown/invalid type
	AttributeType_Unspecified AttributeType = 0
	// ATTRIBUTE_TYPE_UUID defines an attribute value that contains a string value representation of a V4 uuid
	AttributeType_UUID AttributeType = 1
	// ATTRIBUTE_TYPE_JSON defines an attribute value that contains a byte string containing json data
	AttributeType_JSON AttributeType = 2
	// ATTRIBUTE_TYPE_STRING defines an attribute value that contains a generic string value
	AttributeType_String AttributeType = 3
	// ATTRIBUTE_TYPE_URI defines an attribute value that contains a URI
	AttributeType_Uri AttributeType = 4
	// ATTRIBUTE_TYPE_INT defines an attribute value that contains an integer (cast as int64)
	AttributeType_Int AttributeType = 5
	// ATTRIBUTE_TYPE_FLOAT defines an attribute value that contains a float
	AttributeType_Float AttributeType = 6
	// ATTRIBUTE_TYPE_PROTO defines an attribute value that contains a serialized proto value in bytes
	AttributeType_Proto AttributeType = 7
	// ATTRIBUTE_TYPE_BYTES defines an attribute value that contains an untyped array of bytes
	AttributeType_Bytes AttributeType = 8
)

```

Keys may appear multiple times in an attribute list. This is allowed so long as the value \(more specifically the hash of the value\) is unique. This allows flexibility in creating a list of assets \(per this example\) or any other type of information which may have multiple instances for a single address.

Each of the values is associated with some basic type information. This is required for proper serialization. The intent is to support basic types \(int, string\) along with platform specific concepts like Bech32 addresses, URIs, JSON structures, and UUIDs. Type information can be associated with limits to protect the system. Examples include bounds on integers, length restrictions for strings, and possibly reference checks for blockchain URIs. These limits will be configured using parameters for the handler running in the daemon process.

