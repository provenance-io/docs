# Onboarding

Please refer to the [Figure Loan Model](assets.md).

Figure executes a loan onboarding contract for each loan. Every onboarding contract creates a new scope in the P8e EOS, with the loan UUID as the scope UUID. The onboarding contract also establishes the initial values for the facts in the scope. Not all facts will be established during this onboarding process; for example, the `validation_results` fact will be populated only after the initial run of a loan validation contract. Additionally, not all facts will be established for every loan type; for example, a HELOC loan scope will include the `lien_property` fact, but a personal loan will not.

```kotlin
@Participants([ORIGINATOR])
open class OnboardFigureHELOCContract() : P8eContract() {

    @Function(ORIGINATOR)
    @Fact("loan")
    open fun loan(@Input("input_loan") loan: LoanProtos.Loan) = 
            loan.also {
                require(loan.uuid.isValid()) { "loan.uuid is missing" }
                require(loan.loanType.isNotBlank()) { "loan.loanType is missing" }
                require(loan.originatorUuid.isValid()) { "loan.originatorUuid is missing" }
                require(loan.originatorName.isNotBlank()) { "loan.originatorName is missing" }
            }

    @Function(ORIGINATOR)
    @Fact("primary_party")
    open fun primaryParty(@Input("input_primary_party") primaryParty: CustomerProtos.Party) = 
            primaryParty.also {
                require(primaryParty.uuid.isValid()) { "primaryParty.uuid is missing" }
            }

    @Function(ORIGINATOR)
    @Fact("lien_property")
    fun lienProperty(@Input("input_lien_property") lienProperty: PropertyProtos.LienProperty) = 
           lienProperty.also {
                require(lienProperty.uuid.isValid()) { "lienProperty.uuid is missing" }
            }

    // Truncated example
}
```

