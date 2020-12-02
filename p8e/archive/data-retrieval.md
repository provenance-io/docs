# Data Retrieval

When transacting on Provenance, contract hashed execution results are memorialized to the blockchain. Asset data is saved in an encrypted object store hosted by each participant involved in a transaction.

This section describes how data is retrieved from the blockchain and encrypted object store.

Information is retrieved from the blockchain in a data structure referred to as a scope. A scope contains information about a contract execution including the parties involved in the transaction, the contract invoked, and the hashed execution results of the contract. The ContractManager provides a function to find the scope based on a UUID. The following example simply retrieves the information for a given UUID and logs the output.

```kotlin
val scopeWrapper = contractManager.indexClient
                   .findLatestScopeByUuid(<scope UUID>)
if (scopeWrapper?.scope?.recordGroupList.isNullOrEmpty()) {
    log.info("No records found for the provided scope UUID")
} else {
    log.info(scopeWrapper?.scope.toString())
}
```

The ContractManager also provides a function to find scopes based on a list of UUIDs. The following example simply retrieves the information for each of the given UUIDs in the list and logs the output.

```kotlin
val results = contractManager.indexClient.findLatestScopesByUuids(<scope UUID list>)
if (results.scopesList.isNullOrEmpty())
    log.info("No records found for the provided scope UUID list")
else {
    log.info("List size: ${results.scopesCount}")
    results.scopesList.forEach { result ->
        val data = contractManager.hydrate(result.scope.uuid.toUuidProv(), HelloWorldData::class.java)
        log.info(data.scope.toString())
        log.info(data.name.toString())
    }
}
```

There are a couple of ways information can be retrieved from the encrypted object store. For the first, a helper function \(hydrate\) is included in the ContractManager to assist in asset data retrieval. The hydrate function uses the provided UUID to retrieve the data and transform it to the provided predefined output format.

```kotlin
contractManager.hydrate(<scope UUID>, <Data Class Name>::class.java)
```

Output formats are defined using Kotlin data classes or POJO’s in Java. These classes are required to have at least one constructor where all parameters implement a Protobuf message and have a Fact annotation. Scope information can also be extracted by including the scope in the data class.

The following is an example of a class used to extract name and scope information for a transaction memorialized to the blockchain by the Hello World contract.

```kotlin
data class HelloWorldData(@Fact(name = "name") val name: ExampleName,
                          val scope: Scope) {}
```

The second way to retrieve information from the encrypted object store is by executing queries. An index is included with the SDK. Before information is saved to the encrypted object store, predefined fields are added to the index. These predefined fields are identified in the Protobufs and have index metadata information included. The following contains the name Protobuf used in the Hello World example.

```kotlin
message ExampleName {
    UUID uuid = 1;
    string first_name = 2 [(index) = { index: ALWAYS}];
    string last_name = 3 [(index) = { index: ALWAYS}];
    string middle_name = 4;
    string prefix = 5;
    string suffix = 6;
    AuditFields audit_fields = 99;
}
```

The information in the first\_name and last\_name fields will be included in the index. However, middle\_name, prefix, and suffix will not be included in the index. Only information included in the index can be utilized in a query. The four following different indexing options are currently supported:

* NEVER - Never index the field.
* ALWAYS - Always index the field.
* INDEX\_DEFER\_PARENT - Index unless a Parent object states otherwise
* NO\_INDEX\_DEFER\_PARENT - Don’t index unless a Parent object states otherwise

Indexing is also supported at the message level instead of setting each field individually. In the following example, all fields will be indexed except for prefix since field descriptors override message descriptors.

```kotlin
message ExampleName {
    UUID uuid = 1;
    string first_name = 2;
    string last_name = 3;
    string middle_name = 4;
    string prefix = 5 [(index) = { index: NEVER }];
    string suffix = 6;
    AuditFields audit_fields = 99;
    option (message_index) = { index: ALWAYS };
}
```

Queries are built by defining fields, comparators, and values.

```kotlin
"<path to field>" equal "value"
```

The &lt;path to field&gt; is where the field exists within the Protobuf definition, starting with the fact name. For example, to search by first name in the Hello World example the path would be "name.firstName". Compound statements can be defined using and/or to join conditions.

```kotlin
(("<path to field>" equal "value") or ("<path to field>" equal "value") and
 ("<path to field>" equal "value") or ("<path to field>" equal "value"))
```

Predefined operation types are provided and are based on whether the field is numerical or a string. The following operations are provided for numerical fields:

* EQUAL
* GREATER
* GREATER\_EQUAL
* LESS
* LESS\_EQUAL

The following operations are provided for string fields:

* EQUAL
* LIKE
* REGEXP \(Regular Expressions\)

Queries are executed using the ContractManager. The constructor for the Query class provides optional parameters for limiting the size of result list returned as well as for implementing paging. If the parameters are not provided, the size defaults to 100 and the from defaults to 0. The following example uses the first and last names in the Hello World example to search for results. For each result found, the scope UUID is used to hydrate a data class and log the output.

```kotlin
val results = contractManager.indexClient.query(Query(("name.firstName" equal "Hello")
                                                  and ("name.lastName" equal "World"), 50, 0))
if (results.scopesList.isEmpty()) {
    log.info("No records found for the provided query params")
} else {
    results.scopesList.forEach { result ->
        val data = contractManager.hydrate(result.scope.uuid.toUuidProv(), HelloWorldData::class.java)
        log.info(data.scope.toString())
        log.info(data.name.toString())
    }
}
```

