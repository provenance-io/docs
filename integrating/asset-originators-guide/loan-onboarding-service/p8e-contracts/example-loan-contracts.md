---
description: Establishing the loan in the Encrypted Object Store and Blockchain
---

# Onboard Loan Contract

In our example, "onboarding a loan" is the process to establish a scope for a loan and save the loan data to the Encrypted Object Store (EOS). Each function (annotated with `@Function`) establishes one of the facts in the scope. For each function, the originator should add input checks based on their own data model. For any check that does not pass, an `Exception` should be thrown, and the contract execution will fail. (Loan scope will not be established or recorded on-chain.) In the example below, only a few functions have examples of input checks.

### Sample Contract - Record Loan Contract

```kotlin
import io.dartinc.v1beta1.ENote
import io.p8e.annotations.Fact
import io.p8e.annotations.Function
import io.p8e.annotations.Input
import io.p8e.annotations.Participants
import io.p8e.proto.ContractSpecs.PartyType.ORIGINATOR
import io.p8e.spec.P8eContract
import tech.figure.asset.v1beta1.Asset
import tech.figure.loan.LoanScopeFacts
import tech.figure.servicing.v1beta1.LoanStateList
import tech.figure.servicing.v1beta1.ServicingRights
import tech.figure.util.v1beta1.DocumentList
import tech.figure.validation.v1beta1.ValidationResults


@Participants([OWNER])
@ScopeSpecification(["tech.figure.loan"])
open class RecordLoanContract(
    @Record(LoanScopeFacts.asset) val existingAsset: Asset,
    @Record(LoanScopeFacts.eNote) val existingENote: ENote,
) : P8eContract() {

    @Function(OWNER)
    @Record(LoanScopeFacts.asset)
    open fun recordAsset(@Input(LoanScopeFacts.asset) asset: Asset) = asset
            .also {
                if (existingAsset != null) {
                    require(existingAsset.kv.loan.isENote.isFalse()) { "asset cannot be updated" }
                    // optional: make sure nothing important changed
                    // examples:
                    require(existingAsset.id == asset.id) { "cannot change asset ID" }
                    require(existingAsset.type == asset.type) { "cannot change asset type" }
                    require(existingAsset.kv.loan.originatorUuid == asset.kv.loan.originatorUuid) { "cannot change loan originator UUID" }
                    require(existingAsset.kv.loan.originatorName == asset.kv.loan.originatorName) { "cannot change loan originator name" }
                } else {
                    // other validation rules, such as:
                    require(asset.id.isValid()) { "asset.id is missing" }
                    require(asset.type.isNotBlank()) { "asset.type is missing" }
                    require(asset.kv.loan.originatorUuid.isValid()) { "asset.kv.loan.originatorUuid is missing" }
                    require(asset.kv.loan.originatorName.isNotBlank()) { "asset.kv.loan.originatorName is missing" }
                }
            }

    @Function(OWNER)
    @Record(LoanScopeFacts.servicingRights)
    open fun recordServicingRights(@Input(LoanScopeFacts.servicingRights) servicingRights: ServicingRights) = servicingRights

    @Function(OWNER)
    @Record(LoanScopeFacts.documents)
    open fun recordDocuments(@Input(LoanScopeFacts.documents) documents: DocumentList) = documents

    @Function(OWNER)
    @Record(LoanScopeFacts.loanStates)
    open fun recordLoanStates(@Input(LoanScopeFacts.loanStates) loanStates: LoanStateList) = loanStates

    @Function(OWNER)
    @Record(LoanScopeFacts.validationResults)
    open fun recordValidationResults(@Input(LoanScopeFacts.validationResults) validationResults: ValidationResults) = validationResults

    @Function(OWNER)
    @Record(LoanScopeFacts.eNote)
    open fun recordENote(@Input(LoanScopeFacts.eNote) eNote: ENote) = eNote?
            .also {
                if (existingENote != null) {
                    require(existingENote.checksum == eNote.checksum) { "cannot modify or remove eNote during loan onboarding" } // use specific contract instead
                }
                // TODO: Decide which fields should only be required if DART is listed as mortgagee of record/active custodian
                require(eNote.controller.controllerUuid.isValid()) { "ENote missing controller UUID" }
                require(eNote.controller.controllerName.isNotBlank()) { "ENote missing controller Name" }
                require(eNote.eNote.id.isValid()) { "ENote missing ID" }
                require(eNote.eNote.uri.isNotBlank()) { "ENote missing uri" }
                require(eNote.eNote.content_type.isNotBlank()) { "ENote missing content type" }
                require(eNote.eNote.document_type.isNotBlank()) { "ENote missing document type" }
                require(eNote.eNote.checksum.isNotBlank()) { "ENote missing checksum" }
                require(eNote.signedDate.isNotBlank()) { "ENote missing signed date" }
                require(eNote.vaultName.isNotBlank()) { "ENote missing vault name" }
            }

}
```

{% hint style="info" %}
`Input` names are independent of (output)`Fact` names. In our example, it is merely convenient to use the same String label as the object input. In our example, the input is the same object we expect to store as a fact. In other contracts, one might calculate or construct the object in the body of the function instead, and inputs to the `Function` might be different types entirely.
{% endhint %}

### Result

Upon execution of this contract:

* A [P8e Scope](https://github.com/provenance-io/p8e/blob/main/simple-client/src/main/proto/contract/scope.proto#L33) object is established in the EOS, containing the full record of the execution of this Contract and the head state of the facts (data output by this contract) in the scope.
* A [Provenance Blockchain Scope](https://github.com/provenance-io/provenance/blob/main/proto/provenance/metadata/v1/scope.proto) object is established on the blockchain. The originator, who acts as OWNER in the contract, is designated as the value owner of the asset in the scope.
* A [Provenance Blockchain MarkerAccount](https://github.com/provenance-io/provenance/blob/main/proto/provenance/marker/v1/marker.proto) object is established on the blockchain.

### What does it look like on Provenance?

#### Example P8e Scope (Truncated):

{% code title="" %}
```javascript
{
  "blocknumber": 5191103,
  "blocktransactionindex": 0,
  "scope": {
    "uuid": {
      "value": "6e1632bd-2016-47fe-a35b-1eb09b8535c7"
    },
    "parties": [
      {
        "signerRole": "OWNER",
        "signer": {
          "signingPublicKey": {
            "publicKeyBytes": "BOMGXlqov...",
            "type": "ELLIPTIC",
            "curve": "SECP256K1",
            "compressed": false
          },
          "encryptionPublicKey": {
            "publicKeyBytes": "BOMGXlqove6...",
            "type": "ELLIPTIC",
            "curve": "SECP256K1",
            "compressed": false
          }
        },
        "address": "GOrX55pQULrlronarLFe8U4U3bc="
      }
    ],
    "recordGroup": [
      {
        "specification": "t/OyOQF/kRfJHV1kB6JxiB...",
        "groupUuid": {
          "value": "23fb0e7d-a07a-4be4-b6d0-a6b844c443c7"
        },
        "executor": {
          "signingPublicKey": {
            "publicKeyBytes": "BOMGXlqov...",
            "type": "ELLIPTIC",
            "curve": "SECP256K1",
            "compressed": false
          },
          "encryptionPublicKey": {
            "publicKeyBytes": "BOMGXlqove...",
            "type": "ELLIPTIC",
            "curve": "SECP256K1",
            "compressed": false
          }
        },
        "parties": [
          {
            "signerRole": "OWNER",
            "signer": {
              "signingPublicKey": {
                "publicKeyBytes": "BOMGXlqove...",
                "type": "ELLIPTIC",
                "curve": "SECP256K1",
                "compressed": false
              },
              "encryptionPublicKey": {
                "publicKeyBytes": "BOMGXlqove6...",
                "type": "ELLIPTIC",
                "curve": "SECP256K1",
                "compressed": false
              }
            },
            "address": "GOrX55pQULrlronarLFe8U4U3bc="
          }
        ],
        "records": [
          {
            "name": "asset",
            "hash": "qqwX2VwAgG1Z+SVswX4aqH1HqriY1pDTOBnlOwbJjb13FPq/N1iCUIbRe7kH2pT78Gc3vR/Bdu87zWHOjsXFCg==",
            "classname": "tech.figure.asset.v1beta1$Asset",
            "inputs": [
              {
                "name": "perform_input_checks",
                "hash": "IEMjS35lfhIjmdLCpDCbk5j/QmxrorZr6Pua2MaULLEkzGoye51oewwl2yDplvi0HWuzieH/1wGfglhLGe7iTw==",
                "classname": "io.p8e.proto.Common$BooleanResult",
                "type": "NO_DEF_TYPE"
              },
              {
                "name": "asset",
                "hash": "y9Dh0IIIoFeMdYl2VDDHw7iPxTgWbJHgiGO06LauLTscKrI0faHJXKgZKpjz33Qf7arQv3HK3dBpNAxSXmWJQw==",
                "classname": "tech.figure.asset.v1beta1$Asset",
                "type": "NO_DEF_TYPE"
              }
            ],
            "result": "PASS",
            "resultName": "asset",
            "resultHash": "y9Dh0IIIoFeMdYl2VDDHw7iPxTgWbJHgiGO06LauLTscKrI0faHJXKgZKpjz33Qf7arQv3HK3dBpNAxSXmWJQw=="
          },
          {
            "name": "servicingRights",
            "hash": "qqwX2VwAgG1Z+SVswX4aqH1HqriY1pDTOBnlOwbJjb13FPq/N1iCUIbRe7kH2pT78Gc3vR/Bdu87zWHOjsXFCg==",
            "classname": "tech.figure.servicing.v1beta1$ServicingRights",
            "inputs": [
              {
                "name": "perform_input_checks",
                "hash": "IEMjS35lfhIjmdLCpDCbk5j/QmxrorZr6Pua2MaULLEkzGoye51oewwl2yDplvi0HWuzieH/1wGfglhLGe7iTw==",
                "classname": "io.p8e.proto.Common$BooleanResult",
                "type": "NO_DEF_TYPE"
              },
              {
                "name": "servicingRights",
                "hash": "7nWMRI8o5/7Q+8FhNVVxal4TMd+t12ra3KzIKAuFTdK+Se+ym8etviPCkpbI5I29KEnkhhfBjhwzrQEVChLYQA==",
                "classname": "tech.figure.servicing.v1beta1$ServicingRights",
                "type": "NO_DEF_TYPE"
              }
            ],
            "result": "PASS",
            "resultName": "servicingRights",
            "resultHash": "7nWMRI8o5/7Q+8FhNVVxal4TMd+t12ra3KzIKAuFTdK+Se+ym8etviPCkpbI5I29KEnkhhfBjhwzrQEVChLYQA=="
          },
          ... Other Facts ...
        ],
        "classname": "tech.figure.contracts.RecordLoanContract",
        "audit": {
          "createdDate": "2022-03-29T00:01:11Z",
          "createdBy": "tp1rr4d0eu62pgt4edw38d2ev27798pfhdhp5ttha",
          "updatedBy": "",
          "version": 1,
          "message": ""
        }
      }
    ],
    "lastEvent": {
      "groupUuid": {
        "value": "579291a8-b062-4a54-86dc-b175a2e70950"
      },
      "executionUuid": {
        "value": "e8069fc7-178a-4b49-88c4-2d7848908b03"
      }
    }
  }
}
```
{% endcode %}
