---
description: >-
  The Metadata module provides an on-chain storage and assertion facility for
  side-chained data including asses-of-value and NFTs.
---

# Metadata

## Overview

The [Provenance Contact Execution Environment](../p8e/overview/) \(nicknamed “P8e”\) is an optional layer on top of the Provenance Blockchain to allow single and multi-party client-side contract execution while preserving data privacy. Provenance client-side contracts take encrypted data from the user \(client\) and transform the information into encrypted data in the user’s own private object store with object hashes recorded on the blockchain. 

Components of the P8e environment include a client-side contract execution engine, a locally-hosted encrypted object store, an Elasticsearch-based index into the encrypted object store, and a communication exchange for orchestrating multi-party contract execution with other parties’ own P8e environments. Each P8e instance communicates with a blockchain node to submit transactions \(contract execution hash records\) to the Provenance Blockchain.  

Applications use client-side P8e contracts and object stores for preserving PII \(Personally Identifying Information\), sensitive data, and information related to non-fungible tokens. A hash commitment of the information and its state is captured in the Metadata Module.  Effectively, P8e, from this perspective is simply a tool for structuring data to save on chain and isn’t the primary focus of the metadata module.  Thus P8e serves as _a_ side-chain information workflow manager and the Metadata Module is the ultimate on-chain state store. 

{% hint style="info" %}
The Metadata Module provides assertions about off-chain information, whereas representation of value and ownership are recorded in the [Marker Module](marker-module.md).
{% endhint %}

### State Objects

The Metadata Modules manages state using 4 core state object structures:

* Contracts executed by parties on a client-side contract execution environment
* Records that represent the facts \(data\) pertinent to the contracts
* Sessions to manage the parties that must sign and the related input and output data that can be recorded
* Scopes that wrap the Records \(facts/data\) and related Contracts that can mutate the Records in the Scope.

These structures are used to efficiently store the state data in the Metadata Module. The core concept of the Metadata module is a `Scope` that contains a list of `Records` \(facts\). Updates to `Scope` are performed in sessions with details persisted in a `Session`. Each session contains a specification that details constraints on parties that must sign, inputs, and outputs that can be recorded. Each `Scope` may contain a list of allowed specifications that may be used. 

### Scope Data Structures 

#### Metadata Scope 

A metadata scope contains a collection of records of data, each made up of a hash, data type, and ownership information that establishes provenance. 

#### Sessions 

Typically multiple records will be established within a scope at the same time \(see: Contract Memorialization\). A Session contains context for the recording of these records linking a set of records into a common process/ execution associated with a specification indicating allowed and required values. 

#### Record 

A single record collects information about a process that generated it, a collection of inputs, and one or more outputs into a structure identified by a name that is unique within a scope. 

### Specification Structures

Specifications are a heirarchy of references to existing on chain data as well as a list of requirements that incoming requests to record data against a scope must meet. Typical requirements included hashes that must be supplied corosponding to process/executable that must be ran, required signatures and sources of accounts that must sign requests, and allowed attributes/records that may be added to a scope.

#### Scope Specification 

The top level specification for a scope indicates a set of allowed Contract Specifications that are allowed to be used against a given scope to perform updates. Requests to record data that do not derive from these contract specifications are not allowed.

#### Record Specification 

The specifics of which records are allowed within a group \(and by extension the scope overall\). These considerations include required inputs, output format, and parties that must be associated with any request to record.

#### Contract Specification 

The primary function of contract specifications is to denote the Contracts/Processes that will be used to manage the data within a scope. These specifications control what information may be recorded on chain. The use of definitions for inputs can build a chain of data references that must be in place in order for records to be added to the chain.

For example, a contract Specification may list a record Specification that requires an input of type "Record" \(indicating a reference to a scope/record must exist on chain\) in order for a specific Consideration \(process/method\) to be executed and have an output specification ultimately result in a record added to the scope.

### Contract Memorialization

Memorialization is the act of commiting a contract execution and it's associated records to the blockchain.  When memorializing a contract the only pieces that matter are the results and facts. We scope these inside the contract\_group structure to represent a scope around this information and keep the controlling parties \(recitals\) attached which prevents a co-mingling of the rights to change/update these records. The proof submitted to record the facts is part of the readset \(the submitted Contract package\) and is not important now that the information has been recorded. If the source is required it can be pulled from the ReadSet and referenced \(or any of the members that stored it under the associated URI.

