# Onboard to Servicing

After the Figure loan is onboarded, funded, and validated, the loan is onboarded to the Figure Servicing system. Loans are onboarded to the servicing platform by jointly executing a multi-party P8e contract with Figure Servicing. The LOS provides the loan data to the contract and identifies which servicer it is using by specifying the servicing application’s public key as the counter-party. The “assign-to-servicer” contract ensures that the requirements for servicing the loan are met. If the loan data is malformed in some way, the servicing application rejects the contract, and the onus is on the origination system to correct the issue.

Partial example of an “assign-to-servicing” contract to onboard a loan into the servicing system:

```kotlin
@Participants([ORIGINATOR, SERVICER])
open class AssignLoanToServicer(
        // ------------------------------------
        // These are Cross-Scope Facts coming from the loan scope. They do not become facts in the scope
        // in which this contract is executed.
        // ------------------------------------
        // Facts are alphabetical by fact name:
        @Fact(LoanScopeFacts.additionalParties) private val additionalParties: LoanProtos.PartiesList,
        @Fact(LoanScopeFacts.blockchainCustody) private val blockchainCustody: LoanProtos.BlockchainCustody,
        @Fact(LoanScopeFacts.creditReports) private val creditReports: LoanProtos.CreditReportsList,
        @Fact(LoanScopeFacts.digitalSignaturePackets) private val digitalSignaturePackets: DocumentProtos.DocumentWithDataList,
        @Fact(LoanScopeFacts.documents) private val documents: DocumentProtos.DocumentList,
        @Fact(LoanScopeFacts.funding) private val funding: LoanProtos.Funding,
        @Fact(LoanScopeFacts.incomeRecords) private val incomeRecords: LoanProtos.IncomeRecordsList,
        @Fact(LoanScopeFacts.loan) private val loan: LoanProtos.Loan,
        @Fact(LoanScopeFacts.primaryParty) private val primaryParty: CustomerProtos.Party,
        @Fact(LoanScopeFacts.servicing) private val servicing: io.provenance.proto.asset.LoanProtos.LoanServicing,
        @Fact(LoanScopeFacts.signedPromNote) private val signedPromNote: DocumentProtos.Disclosure,
        @Fact(LoanScopeFacts.triMergeReports) private val triMergeReports: LoanProtos.TriMergeReportsList,
        @Fact(LoanScopeFacts.underwritingPacket) private val underwritingPacket: UnderwritingProtos.UnderwritingPacket
) : P8eContract() {

    @Function(SERVICER)
    @Fact(LoanScopeFacts.servicingScopeId)
    fun validateServicingRequirements(@Input(LoanScopeFacts.servicingScopeId) servicingScopeId: Util.UUID ) : Util.UUID = servicingScopeId.also {
        
        // "loan" fact validation
        val selectedOffer = underwritingPacket.selectedOffer
        ValidationUtil.validateMoneyField(selectedOffer, selectedOffer.amount, "amount")?.also { failure(it) }
        ValidationUtil.validateMoneyField(selectedOffer, selectedOffer.drawAmount, "drawAmount")?.let { failure(it) }
        ValidationUtil.validateMoneyField(loan, loan.monthlyPaymentAmount, "monthlyPaymentAmount")?.also { failure(it) }
        if (selectedOffer.amount.isValidSrv() && selectedOffer.drawAmount.isValidSrv()) {
            val originationFeeAmount = view.originationFeeAmount
            groupedRule("Derived origination fee should be at least zero, but was [amount (${view.loanAmount}) - drawAmount (${view.drawAmount}) = origination fee ($originationFeeAmount)]", {
                originationFeeAmount gte BigDecimal.ZERO
            }, ONBOARDING to FATAL, UPDATE to WARN)
            if (originationFeeAmount gt BigDecimal.ZERO) {
                rule("Loans with an origination fee should have an origination fee type, but had type [${view.originationFeeType}]") {
                    selectedOffer.origFeeType.let { it.isNotBlank() && it != NO_FEE }
                }
            }
        }

        // "primary_party" fact validation
        rule("Primary borrower was missing uuid") {
            primaryParty.hasUuid()
        }
        rule("Primary borrower was missing name details. First Name: [${primaryParty.name.firstName}] | Last Name: [${primaryParty.name.lastName}]") {
            primaryParty.hasName() && primaryParty.name.firstName.isNotBlank() && primaryParty.name.lastName.isNotBlank()
        }
        
        // "funding" fact validation
        rule("Funding must have valid start/complete") { funding.completedDate.isValidSrv() && funding.startedDate.isValidSrv() }
        rule("Funding must be set true") { funding.complete }
        rule("Funding block must be non-default") { funding.isValidSrv() }
        rule("Funding start is too early") { funding.startedDate.toCSTLocalDateSrv().isAfterInclusiveSrv(ValidationUtil.EARLIEST_DATE_ALLOWABLE.toLocalDate()) }
        rule("Funding complete is too early") { funding.completedDate.toCSTLocalDateSrv().isAfterInclusiveSrv(ValidationUtil.EARLIEST_DATE_ALLOWABLE.toLocalDate()) }
        rule("Funding complete cannot be in the future") { funding.completedDate.toOffsetDateTimeProv().isBeforeInclusiveSrv(ServicingTimeUtil.getServerTime()) }

```

The contract execution to handoff a loan to the servicer has several unique characteristics:

1. The `@Participants` annotation on the class indicates that two parties are required to execute this contract, one acting in the role of ORIGINATOR and the counter-party in the role of SERVICER.
2. This contract is executed in a unique scope, distinct from the loan scope.
3. The facts supplied in the contract constructor are pre-existing facts. However, the loan facts exist in the loan scope, not the new scope created for the servicing handoff. Nonetheless, the loan scope facts in their current state can be supplied as inputs to the `AssignLoanToServicerContract` as cross-scope facts. 
4. The ORIGINATOR initiates the contract execution and supplies the cross-scope loan facts.
5. The SERVICER supplies one additional input, the UUID of the new scope the servicer establishes in order to store loan servicing data.
6. If any of the servicing checks in this contract fail, they will throw an Exception and fail the contract execution.
7. Either party can terminate the contract execution at any time during the process. When the servicer receives the request to execute the contract, the servicing system can perform checks about whether it is willing to jointly execute the contract with the originator. The servicing system will check, for example, that the public key of the originator matches one or more of the keys expected by the set of originators handled by the system, or it might check that the `loan.loanType` is one the system knows how to service.



