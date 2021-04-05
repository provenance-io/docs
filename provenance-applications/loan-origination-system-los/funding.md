# Funding

Figure will fund a loan at the time of onboarding using stablecoin issued by an Omnibus Bank. When the Omnibus Bank receives funds from the originator, it mints a corresponding amount of stablecoin into a Marker account representing the originator’s funding source. Loans are funded by transferring stablecoin to Omnibus Bank's Marker account with an identifier representing the borrower’s fiat bank information. Next, the Omnibus Bank will convert the borrower’s coin to fiat and send funds to the borrower’s bank account. The stablecoin used is burned in the process of conversion to fiat.

Example Code - fund a loan using stablecoin on Provenance Blockchain:

```kotlin
fun transferCoin(denom: String, amount: Int, toAddress: String, memo: String) =
        pbClient().estimateTx {
            prepare { this.banks.transferFunds(toAddress, listOf(Coin(denom, amount.toString()))) }
        }.let {
            val gasFeePair = adjustGasEstimates(it)
            log.info("Transfer: $amount $denom from keystone ID: ${keystone.provenanceConfig.memberUuid()} " +
                    "address: ${keystone.address()} index: ${keystone.addressIndex()} to address: $toAddress memo: $memo " +
                    "est gas: ${gasFeePair.first} est txFees: ${gasFeePair.second}")
            pbClient().runTx(gas = gasFeePair.first, fees = listOf(Coin(Denom.vspn.name, gasFeePair.second)), memo = memo) {
                this.banks.transferFunds(toAddress, listOf(Coin(denom, amount.toString())))
            }
        }

fun adjustGasEstimates(gasEstimate: GasEstimates): Pair<String, String> =
        gasEstimate.total.toBigDecimal().times(GAS_AND_FEE_ADJUSTMENT)
                .setScale(0, RoundingMode.HALF_UP).let { gas ->
                    val fees = gas.times(FEE_PERCENTAGE).setScale(0, RoundingMode.HALF_UP)
                    Pair(
                            gas.toLong().toString(),
                            fees.toLong().toString()
                    )
                }
```



Throughout the funding process, additional P8e contracts are invoked to save funding-related metadata to the scope.



