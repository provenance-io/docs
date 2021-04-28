---
description: Example contracts used in loan onboarding
---

# Example Loan Contracts

These contracts are examples of what might be used for our loan onboarding example application.

In our example, "onboarding a loan" is the process to establish a scope for a loan and save the loan data to the EOS. Each function \(annotated with `@Function`\) establishes one of the facts in the scope. For each function, the originator should add input checks based on their own data model. For any check that does not pass, an `Exception` should be thrown, and the contract execution will fail. \(Loan scope will not be established or recorded on-chain.\) In the example below, only a few functions have examples of input checks.

{% code title="OnboardLoanContract.kt" %}
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
    @Fact(LoanScopeFacts.additionalParties)
    open fun additionalParties(@Input(LoanScopeFacts.additionalParties) additionalParties: LoanProtos.PartiesList) = additionalParties
            .also {
                // Each function should perform checks on the input data
                // Any exception thrown will fail the contract execution
                    additionalParties.partiesList.forEachIndexed { index, party ->
                        require(party.uuid.isValidSrv()) { "additionalParties[$index].uuid is missing" }
                    }
            }

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.blockchainCustody)
    open fun blockchainCustody() = LoanProtos.BlockchainCustody.newBuilder().setStatus(BlockChainCustody.ON_CHAIN.name).build()

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.creditReports)
    open fun creditReports(@Input(LoanScopeFacts.creditReports) creditReports: LoanProtos.CreditReportsList) = creditReports
            .also {
                    creditReports.reportsList.forEachIndexed { index, report ->
                        require(report.uuid.isValidSrv()) { "creditReports[$index].uuid is missing" }
                        require(report.partyUuid.isValidSrv()) { "creditReports[$index].partyUuid is missing" }
                    }
            }
    
    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.documents)
    open fun documents(@Input(LoanScopeFacts.documents) documents: DocumentProtos.DocumentList) = documents

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.digitalSignaturePackets)
    open fun digitalSignaturePackets(@Input(LoanScopeFacts.digitalSignaturePackets) digitalSignaturePackets: DocumentProtos.DocumentWithDataList) = digitalSignaturePackets

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.funding)
    open fun funding(@Input(LoanScopeFacts.funding) funding: LoanProtos.Funding) = funding
            .also {
                // If loan is already funded, perform checks on funding info
                // If loan is not yet funded (e.g. will fund with stablecoin), consider setting funding fact to default/empty proto
            }

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.incomeRecords)
    open fun incomeRecords(@Input(LoanScopeFacts.incomeRecords) incomeRecords: LoanProtos.IncomeRecordsList) = incomeRecords

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.loan)
    open fun loan(@Input(LoanScopeFacts.loan) loan: LoanProtos.Loan) = loan
            .also {
                    require(loan.uuid.isValidSrv()) { "loan.uuid is missing" }
                    require(loan.loanType.isNotBlank()) { "loan.loanType is missing" }
                    require(loan.originatorUuid.isValidSrv()) { "loan.originatorUuid is missing" }
                    require(loan.originatorName.isNotBlank()) { "loan.originatorName is missing" }
            }

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.primaryParty)
    open fun primaryParty(@Input(LoanScopeFacts.primaryParty) primaryParty: CustomerProtos.Party) = primaryParty
    
    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.servicing)
    open fun servicing(@Input(LoanScopeFacts.servicing) servicing: io.provenance.proto.asset.LoanProtos.LoanServicing) = servicing

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.signedPromNote)
    open fun signedPromNote(@Input(LoanScopeFacts.signedPromNote) signedPromNote: DocumentProtos.Disclosure) = signedPromNote

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.triMergeReports)
    open fun triMergeReports(@Input(LoanScopeFacts.triMergeReports) triMergeReports: LoanProtos.TriMergeReportsList) = triMergeReports

    @Function(ORIGINATOR)
    @Fact(LoanScopeFacts.underwritingPacket)
    open fun underwritingPacket(@Input(LoanScopeFacts.underwritingPacket) underwritingPacket: UnderwritingProtos.UnderwritingPacket) = underwritingPacket

}
```
{% endcode %}

