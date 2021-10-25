# Data Retrieval

As previously mentioned, executed contracts will have their resulting hashes saved to EOS, and resulting scopes memorialized to Provenance Blockchain. The mechanism used to convert Provenance Blockchain scopes into their original data is referred to hydration within the SDK. This process is straightforward, it parses the protobuf datatypes and hashes from the Provenance Blockchain scope and pulls the objects associated with those hashes from EOS. Assuming they have the correct permission to pull and decrypt those objects, the raw bytes can be converted into their associated datatype and returned.

A subset of a Provenance Blockchain scope is provided to show the relationship between EOS and Provenance Blockchain.

```yaml
records:
- record:
    inputs:
    - hash: s+siKv96ekn8y22I79zjWHIfJRmnZFC18q1rZ0vgvhM=
      name: name
      status: RECORD_INPUT_STATUS_PROPOSED
      type_name: io.p8e.proto.example.HelloWorldExample$ExampleName
    name: name
    outputs:
    - hash: vpb21hMS23JTPYqtW9BN3c5/DT1Z4bgRLFJFTnRQAc0=
      status: RESULT_STATUS_PASS
    process:
      hash: Qc2bHdrg+3LxlItTZSbzhJnUn5Btha0LCXaiSk34Hhk=
      method: name
      name: io.p8e.proto.example.HelloWorldExample$ExampleName
    session_id: session1q8dkpadudgr5fq5zaejt8c2w44l4s23wjztxqjg63qkfq7kzu7glzyuppf2
    specification_id: recspec1qhvwfpz7d96gwrahg6qfdhx4kssg9g6n0lcdhnn7as6ad8ku8gvfu526tn4
scope:
  scope:
    data_access: []
    owners:
    - address: tp1vz99nyd2er8myeugsr4xm5duwhulhp5arsr9wt
      role: PARTY_TYPE_OWNER
    scope_id: scope1qrdkpadudgr5fq5zaejt8c2w44ls30k6p6
    specification_id: scopespec1qjkyp28sldx5r9ueaxqc5adrc5wszy6nsh
    value_owner_address: ""
sessions:
- contract_spec_id_info:
    contract_spec_addr: contractspec1q0vwfpz7d96gwrahg6qfdhx4kssq7kz46e
    contract_spec_id: contractspec1q0vwfpz7d96gwrahg6qfdhx4kssq7kz46e
    contract_spec_uuid: d8e4845e-6974-870f-b746-8096dcd5b420
  session:
    audit:
      created_by: tp1vz99nyd2er8myeugsr4xm5duwhulhp5arsr9wt
      created_date: "2021-09-10T17:29:48.987310700Z"
      message: ""
      updated_by: ""
      updated_date: "0001-01-01T00:00:00Z"
      version: 1
    context: null
    name: io.p8e.contracts.examplekotlin.HelloWorldContract
    parties:
    - address: tp1vz99nyd2er8myeugsr4xm5duwhulhp5arsr9wt
      role: PARTY_TYPE_OWNER
    session_id: session1q8dkpadudgr5fq5zaejt8c2w44l4s23wjztxqjg63qkfq7kzu7glzyuppf2
    specification_id: contractspec1q0vwfpz7d96gwrahg6qfdhx4kssq7kz46e
```

The associated contract that would produce such a scope.

```kotlin
@Participants(roles = [OWNER])
@ScopeSpecification(names = ["io.p8e.contracts.examplekotlin.helloWorld"])
open class HelloWorldContract(): P8eContract() {
    @Function(invokedBy = OWNER)
    @Record(name = "name")
    open fun name(@Input(name = "name") name: ExampleName) =
        name.toBuilder()
            .setFirstName(name.firstName.plus("-hello"))
            .setLastName(name.lastName.plus("-world"))
            .build()
}

@ScopeSpecificationDefinition(
    uuid = "<UUID>",
    name = "io.p8e.contracts.examplekotlin.helloWorld",
    description = "A generic scope that allows for a lot of example hello world contracts.",
    partiesInvolved = [OWNER],
)
open class HelloWorldScopeSpecification() : P8eScopeSpecification()
```

Lastly, the associated protobuf definition.

```java
option java_package = "io.p8e.proto.example";
option java_outer_classname = "HelloWorldExample";

message ExampleName {
    string first_name = 1;
    string last_name = 2;
    string middle_name = 3;
    string prefix = 4;
    string suffix = 5;
}
```

