---
description: Establishing the loan in the Encrypted Object Store and Blockchain
---

# Onboard Loan Contract

In our example, "onboarding a loan" is the process to establish a scope for a loan and save the loan data to the EOS. Each function \(annotated with `@Function`\) establishes one of the facts in the scope. For each function, the originator should add input checks based on their own data model. For any check that does not pass, an `Exception` should be thrown, and the contract execution will fail. \(Loan scope will not be established or recorded on-chain.\) In the example below, only a few functions have examples of input checks.

```kotlin
import io.p8e.annotations.Fact
import io.p8e.annotations.Function
import io.p8e.annotations.Input
import io.p8e.annotations.Participants
import io.p8e.loan.LoanScopeFacts
import io.p8e.proto.ContractSpecs.PartyType.ORIGINATOR
import io.p8e.spec.P8eContract
import io.provenance.constants.BlockChainCustody
import io.provenance.proto.CustomerProtos
import io.provenance.proto.PropertyProtos
import io.provenance.proto.UnderwritingProtos
import io.provenance.proto.common.DocumentProtos
import io.provenance.proto.loan.LoanProtos


@Participants([ORIGINATOR])
open class OnboardLoanContract : P8eContract() {

    // Alphabetical by fact name:

    @Function(ORIGINATOR)
    @Fact("additionalParties")
    open fun additionalParties(@Input("additionalParties") additionalParties: LoanProtos.PartiesList) = additionalParties
            .also {
                // Each function should perform checks on the input data
                // Any exception thrown will fail the contract execution
                additionalParties.partiesList.forEachIndexed { index, party ->
                    require(party.uuid.isValid()) { "additionalParties[$index].uuid is missing" }
                }
            }

    @Function(ORIGINATOR)
    @Fact("blockchainCustody")
    open fun blockchainCustody() = LoanProtos.BlockchainCustody.newBuilder().setStatus(BlockChainCustody.ON_CHAIN.name).build()

    @Function(ORIGINATOR)
    @Fact("creditReports")
    open fun creditReports(@Input("creditReports") creditReports: LoanProtos.CreditReportsList) = creditReports
            .also {
                creditReports.reportsList.forEachIndexed { index, report ->
                    require(report.uuid.isValid()) { "creditReports[$index].uuid is missing" }
                    require(report.partyUuid.isValid()) { "creditReports[$index].partyUuid is missing" }
                }
            }

    @Function(ORIGINATOR)
    @Fact("documents")
    open fun documents(@Input("documents") documents: DocumentProtos.DocumentList) = documents

    @Function(ORIGINATOR)
    @Fact("digitalSignaturePackets")
    open fun digitalSignaturePackets(@Input("digitalSignaturePackets") digitalSignaturePackets: DocumentProtos.DocumentWithDataList) = digitalSignaturePackets

    @Function(ORIGINATOR)
    @Fact("funding")
    open fun funding(@Input("funding") funding: LoanProtos.Funding) = funding
            .also {
                // If loan is already funded, perform checks on funding info
                // If loan is not yet funded (e.g. will fund with stablecoin), consider setting funding fact to default/empty proto
            }

    @Function(ORIGINATOR)
    @Fact("incomeRecords")
    open fun incomeRecords(@Input("incomeRecords") incomeRecords: LoanProtos.IncomeRecordsList) = incomeRecords

    @Function(ORIGINATOR)
    @Fact("lienProperty")
    fun lienProperty(@Input("lienProperty") lienProperty: PropertyProtos.LienProperty) = lienProperty
    
    @Function(ORIGINATOR)
    @Fact("loan")
    open fun loan(@Input("loan") loan: LoanProtos.Loan) = loan
            .also {
                require(loan.uuid.isValid()) { "loan.uuid is missing" }
                require(loan.loanType.isNotBlank()) { "loan.loanType is missing" }
                require(loan.originatorUuid.isValid()) { "loan.originatorUuid is missing" }
                require(loan.originatorName.isNotBlank()) { "loan.originatorName is missing" }
            }

    @Function(ORIGINATOR)
    @Fact("primaryParty")
    open fun primaryParty(@Input("primaryParty") primaryParty: CustomerProtos.Party) = primaryParty

    @Function(ORIGINATOR)
    @Fact("servicing")
    open fun servicing(@Input("servicing") servicing: io.provenance.proto.asset.LoanProtos.LoanServicing) = servicing

    @Function(ORIGINATOR)
    @Fact("signedPromNote")
    open fun signedPromNote(@Input("signedPromNote") signedPromNote: DocumentProtos.Disclosure) = signedPromNote

    @Function(ORIGINATOR)
    @Fact("triMergeReports")
    open fun triMergeReports(@Input("triMergeReports") triMergeReports: LoanProtos.TriMergeReportsList) = triMergeReports

    @Function(ORIGINATOR)
    @Fact("underwritingPacket")
    open fun underwritingPacket(@Input("underwritingPacket") underwritingPacket: UnderwritingProtos.UnderwritingPacket) = underwritingPacket

}
```

{% hint style="info" %}
`Input` names are independent of \(output\) `Fact` names. In our example, it is merely convenient to use the same String label as the object input. In our example, the input is the same object we expect to store as a fact. In other contracts, one might calculate or construct the object in the body of the function instead, and inputs to the `Function` might be different types entirely.
{% endhint %}

Upon execution of this contract:

* A [P8e Scope](https://github.com/provenance-io/p8e/blob/main/simple-client/src/main/proto/contract/scope.proto#L33) object is established in the EOS, containing the full record of the execution of this Contract and the head state of the facts \(data output by this contract\) in the scope. 
* A [Provenance Scope](https://github.com/provenance-io/provenance/blob/main/proto/provenance/metadata/v1/scope.proto) object is established on the blockchain. The originator is designated as the value owner of the asset in the scope.
* A [Provenance MarkerAccount](https://github.com/provenance-io/provenance/blob/main/proto/provenance/marker/v1/marker.proto) object is established on the blockchain.

#### Example P8e Scope:

{% code title="// Fetching the scope through the P8e contract manager" %}
```kotlin
contractManager.indexClient.findLatestScopeByUuid(scopeUuid)
```
{% endcode %}

{% code title="// Truncated example of P8e Scope" %}
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
        "signerRole": "ORIGINATOR",
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
            "signerRole": "ORIGINATOR",
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
            "name": "additionalParties",
            "hash": "qqwX2VwAgG1Z+SVswX4aqH1HqriY1pDTOBnlOwbJjb13FPq/N1iCUIbRe7kH2pT78Gc3vR/Bdu87zWHOjsXFCg==",
            "classname": "io.provenance.proto.loan.LoanProtos$PartiesList",
            "inputs": [
              {
                "name": "perform_input_checks",
                "hash": "IEMjS35lfhIjmdLCpDCbk5j/QmxrorZr6Pua2MaULLEkzGoye51oewwl2yDplvi0HWuzieH/1wGfglhLGe7iTw==",
                "classname": "io.p8e.proto.Common$BooleanResult",
                "type": "NO_DEF_TYPE"
              },
              {
                "name": "additional_parties",
                "hash": "y9Dh0IIIoFeMdYl2VDDHw7iPxTgWbJHgiGO06LauLTscKrI0faHJXKgZKpjz33Qf7arQv3HK3dBpNAxSXmWJQw==",
                "classname": "io.provenance.proto.loan.LoanProtos$PartiesList",
                "type": "NO_DEF_TYPE"
              }
            ],
            "result": "PASS",
            "resultName": "additional_parties",
            "resultHash": "y9Dh0IIIoFeMdYl2VDDHw7iPxTgWbJHgiGO06LauLTscKrI0faHJXKgZKpjz33Qf7arQv3HK3dBpNAxSXmWJQw=="
          },
          {
            "name": "digitalSignaturePackets",
            "hash": "qqwX2VwAgG1Z+SVswX4aqH1HqriY1pDTOBnlOwbJjb13FPq/N1iCUIbRe7kH2pT78Gc3vR/Bdu87zWHOjsXFCg==",
            "classname": "io.provenance.proto.common.DocumentProtos$DocumentWithDataList",
            "inputs": [
              {
                "name": "perform_input_checks",
                "hash": "IEMjS35lfhIjmdLCpDCbk5j/QmxrorZr6Pua2MaULLEkzGoye51oewwl2yDplvi0HWuzieH/1wGfglhLGe7iTw==",
                "classname": "io.p8e.proto.Common$BooleanResult",
                "type": "NO_DEF_TYPE"
              },
              {
                "name": "digital_signature_packets",
                "hash": "7nWMRI8o5/7Q+8FhNVVxal4TMd+t12ra3KzIKAuFTdK+Se+ym8etviPCkpbI5I29KEnkhhfBjhwzrQEVChLYQA==",
                "classname": "io.provenance.proto.common.DocumentProtos$DocumentWithDataList",
                "type": "NO_DEF_TYPE"
              }
            ],
            "result": "PASS",
            "resultName": "digital_signature_packets",
            "resultHash": "7nWMRI8o5/7Q+8FhNVVxal4TMd+t12ra3KzIKAuFTdK+Se+ym8etviPCkpbI5I29KEnkhhfBjhwzrQEVChLYQA=="
          },
          {
            "name": "loan",
            "hash": "qqwX2VwAgG1Z+SVswX4aqH1HqriY1pDTOBnlOwbJjb13FPq/N1iCUIbRe7kH2pT78Gc3vR/Bdu87zWHOjsXFCg==",
            "classname": "io.provenance.proto.loan.LoanProtos$Loan",
            "inputs": [
              {
                "name": "perform_input_checks",
                "hash": "IEMjS35lfhIjmdLCpDCbk5j/QmxrorZr6Pua2MaULLEkzGoye51oewwl2yDplvi0HWuzieH/1wGfglhLGe7iTw==",
                "classname": "io.p8e.proto.Common$BooleanResult",
                "type": "PROPOSED"
              },
              {
                "name": "loan",
                "hash": "pCkuNk9tLDX/gsO8zVQVRWGlb4etDEwp3OCNNynkuPMDrxaWDNuGPdhnnc3k2THQpGJKcxcPsl/MBSn48e7+2A==",
                "classname": "io.provenance.proto.loan.LoanProtos$Loan",
                "type": "PROPOSED"
              }
            ],
            "result": "PASS",
            "resultName": "loan",
            "resultHash": "pCkuNk9tLDX/gsO8zVQVRWGlb4etDEwp3OCNNynkuPMDrxaWDNuGPdhnnc3k2THQpGJKcxcPsl/MBSn48e7+2A=="
          }
        ],
        "classname": "com.example.contracts.OnboardLoanContract",
        "audit": {
          "createdDate": "2021-03-29T00:01:11Z",
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



