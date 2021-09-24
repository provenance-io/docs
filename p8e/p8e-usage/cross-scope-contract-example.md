# Cross Scope \(Update\) Contract

Cross scope contract refers to a contract inputting information that was output, and ultimately memorialized to the blockchain, by the execution of another contract. In Provenance terms, this is executing a new session against an existing scope.

Executing a contract against an existing scope requires the existing scope as an input during execution. This is used to match on the contract class parameters against the existing scope names. When the name is found the value is fetched from EOS by hash and is available to functions inside of the contract.

```kotlin
@Participants(roles = [ORIGINATOR])
@ScopeSpecification(names = [loanScopeNamespace])
open class AddLoanDocument(
    @Record(name = "documents") val existingDocuments: DocumentList,
) : P8eContract() {
    @Function(invokedBy = ORIGINATOR)
    @Record(name = "documents")
    open fun addDocument(@Input(name = "documents") documents: DocumentList): DocumentList {
        return existingDocuments.toBuilder()
            .addAllDocuments(documents.documentsList)
            .build()
    }
}
```

