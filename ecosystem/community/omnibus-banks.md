# Omnibus Banks

## Overview

The primary role of the omnibus bank is to hold fiat deposits and correspondingly mint and burn stablecoin on the Provenance blockchain to match these fiat deposits.  Stablecoin is critical to support bilateral transactions on Provenance that are absent of counterparty and settlement risk.  Banks take ownership of and use an API to interface with Provenance.  Banks earn float on the fiat deposits underlying the stablecoin for the performance of the minting and burning functions.

The Bank is integrated with Provenance through traditional technical protocols, where Provenance has the ability to initiate stablecoin transactions, subject to fulfilling commercially negotiated conditions codified in smart contracts. The flow of funds and the transacting parties are no different from traditional, off-chain transactions, and the bank’s operational processes do not change. 

Any transacting party on Provenance that has to transfer funds to another participant will have to open an account with the relevant/selected omnibus bank by use case and region.

## Operational Recommendations

To support transaction flows, a bank will execute the following actions via API in real time:

* Initiate wire transfers to 3rd parties
* Monitor for and identify incoming wires
* Process ACH transfers and receive acknowledgements
* Report returned items \(insufficient funds, account not found...etc\)
* Retrieve daily transaction history on sub-accounts
* Create and destroy temporary \(“virtual”\) escrow accounts

## Stablecoin Context

Every stablecoin has a “minter” - the bank that created the coin.  Only nationally chartered banks with access to the fed settlement system will be recognized as omnibus banks for stablecoin.    

Fiat deposits underlying the stablecoin are not committed to escrow; rather they are a fungible liability \(or sold asset, discussed below\), and the creditworthiness of the stablecoin is the creditworthiness of the omnibus bank “minter”.  

Using the example above, if a fund had an account with the omnibus bank, the fund can simply wire the money to its own account and request it be converted to stablecoin via minting, with the fund blockchain address in the wire.  On such action, the bank is essentially selling stablecoin to the fund, creating a stablecoin liability offset by the cash asset, where the cash is unencumbered.  However, banks might want to represent the stablecoin as a special deposit \(though still fungible to general liabilities\), requiring redemption of the stablecoin to access.

Omnibus banks should look at stablecoin as the equivalent to a short term debt issuance \(though redeemable any time and at zero coupon\), and thus eliminate the need for an explicit account.  Rather the omnibus bank would mint and destroy coins in the open market, on demand.

In situations where two parties transact on Provenance using an omnibus bank’s stablecoin, the omnibus bank is removed from such transaction and not responsible for BSA/AML on either party.

The benefit of a bank being an omnibus bank is the float from minting stable coin and any transaction fees, custody fees or other fees provided to individuals and institutions transacting on Provenance.  

## Process Flow Examples

To illustrate, we'll walk through detailed process flows for a US Omnibus Bank supporting Provenance’s initial use case: loan origination, servicing, and financing. While Provenance is actively engaged with other use cases, this example aims to demonstrate typical operating processes and workflows. Note also that the illustrated process should be broadly applicable to any asset-backed financing, subject to specific regulatory requirements by jurisdiction and asset class.

### Issuing Stablecoin \(Minting\)

User must have a Provenance account \(an address on the blockchain\) to hold stablecoin.  The keys associated with that account are usually stored in a wallet

The Stablecoin Bank has the BSA/AML obligation.  They may leverage a Passport, such as Figure’s Passport, in order to fulfill its BSA/AML obligation.

1. User sends fiat to Stablecoin Bank to mint stablecoin.
2. Upon receipt and verification of funds, Stablecoin Bank uses an API call to Provenance to mint a corresponding amount of stablecoin to the Stablecoin Bank’s Provenance account.  The stablecoin is a 1:1 digital representation of the corresponding fiat on deposit at Stablecoin Bank.  
3. The Stablecoin Bank then moves the stablecoin to the account of the user who originally deposited the fiat.
4. User may then transact using stablecoin on third-party applications built on Provenance, or may send that stablecoin to another user on the platform.

### Funding an Asset

Originators fund loans at time of onboarding using stablecoin issued by an Omnibus Bank. When the Omnibus Bank receives funds from the originator, it mints a corresponding amount of stablecoin to an address representing the originator’s blockchain funding account. Loans are funded by transferring coin to Omnibus Bank's blockchain address with an identifier representing the borrower’s fiat bank information. Next, the Omnibus Bank will convert the borrower’s coin to fiat and send funds to the borrower’s bank account. The stablecoin used is burned in the process of conversion to fiat.

### Bilateral Asset\(s\) Sale

Assume a fund is buying a pool of loans that are registered on Provenance.  That fund enters into a sale agreement with the seller and wires the transaction amount \(with an identifier\) to an omnibus bank.    


![](../../.gitbook/assets/omnibus-bank-and-stablecoin.png)

The omnibus bank uses the API call to Provenance to create stablecoin in an equal amount to the wire received and deposit the stablecoin into the fund’s blockchain account on Provenance \(signified by the identifier in the wire\).   
  


![](../../.gitbook/assets/omnibus-bank-and-stablecoin-1-.png)

Provenance then transfers the agreed amount of stablecoin from the fund’s blockchain account to the seller's blockchain and transfers ownership of the loans to the fund at the same time.  This a T+0 transaction with no counterparty or settlement risk.  


![](../../.gitbook/assets/omnibus-bank-and-stablecoin-2-%20%281%29%20%281%29.png)

Account owners on Provenance can redeem their stablecoin from the omnibus bank at any time.  In doing so, the omnibus bank destroys \(burns\) the stablecoin and transfers the corresponding amount of fiat to the redeemer's target fiat account.    


![](../../.gitbook/assets/omnibus-bank-and-stablecoin-3-.png)

### Redeeming Stablecoin \(Burning\)

Any stablecoin holder may redeem its stablecoin at the Stablecoin Bank that minted the stablecoin for a corresponding amount of fiat.  Stablecoin should be readily attributable to the Stablecoin Bank that minted that particular stablecoin.

1. The user will send the stablecoin to the Stablecoin Bank’s Provenance account for redemption.
2. Upon receipt of the stablecoin and in connection with the redemption request, the Stablecoin Bank withdraws the fiat from their account and will burn the received stablecoin to maintain the 1:1 relationship between minted stablecoin and fiat on deposit.
3. The Stablecoin Bank will either deposit the corresponding fiat into the user’s fiat account at the Stablecoin Bank or will send those funds by wire or ACH to another financial institution at the request of the user.

## Reference Implementation

A middle-tier component, that we'll simply call "omnibus," will be used to describe the software processes that would be used to integrate Provenance with a core banking system. Omnibus will bridge a core banking system API with Provenance to monitor for banking transactions that require minting of stablecoin and to monitor for blockchain transactions that require burning of stablecoin. 

### Fiat to Stablecoin \(Mint\)

The process to convert fiat to stablecoin can be handled in several different ways. Here we'll look at a few that tend to be useful.  

#### Receiving Wires

Wiring of funds tends to be used for large transactions, and it one of the simplest ways to identify received funds, but does tend to come with a high-probability for human error since the wire reference is hand entered on the transmission side of the wire. 

![](https://i.imgur.com/JWTPqkr.png)

#### Virtual Accounts

Assigning a user an account/routing number can be a useful tool that makes converting deposits to stablecoin simple. In this use case the client simply sends funds via ACH to an account number at the institution. The account/routing number is 1:1 with a blockchain address and the omnibus facilitates the mapping, minting and transfer of stablecoin.

![](https://i.imgur.com/iPz9GdT.png)

#### ACH Pull

In some cases it is necessary to allow a small scale user to initiate a pull of funds from their current brick-and-mortar account to be delivered to their blockchain address as stablecoin. This use case is by far the most complex since ACH has many clearing issues that an institution has to consider.

![](https://i.imgur.com/7wbQjWU.png)

### Stablecoin to Fiat \(Burn\) <a id="MintingBurningTransferring"></a>

Once a blockchain address holds stablecoin, it is necessary to provide a process to convert it to fiat at some time in the future. To achieve this conversion the omnibus must understand where the outbound transfer of fiat will be sent and what reference to watch for on the blockchain when stablecoin is received for redemption. 

#### Memo Field

![](https://i.imgur.com/tZ7c5Np.png)



### Verified Coins

While coins in general can be created on Provenance by any entity, not all coins are issued by trusted entities. Trusting the creator of the coin is important because the store of value is a bridge for t-0 settlement with fiat. While the coin denom \(name\) is secured on Provenance by an encryption key, it is necessary to know the issuer\(s\) when accepting stablecoins as payment to guarantee the validity of the entity backing the issued coin.

Determining a method to verify trust lines between entities on the blockchain is an exercise that each entity should determine on their own, but using a trusted list of partner addresses can simplify choosing known good actors on the network. 

## Legal Framework

### Federally-Chartered Banks May Issue Stablecoin

* In January 2021, the OCC issued an [Interpretive Letter](https://www.occ.gov/news-issuances/news-releases/2021/nr-occ-2021-2a.pdf) stating that nationally-chartered banks and Federal savings associations “may buy, sell, and issue stablecoin to facilitate payments.”
* In a previous [Interpretive Letter](https://www.occ.gov/topics/charters-and-licensing/interpretations-and-actions/2020/int1172.pdf), the OCC stated that national banks may receive deposits from stablecoin issuers, including deposits that constitute stablecoin, and may perform incidental activities related to that deposit-taking activity.

### Stablecoin Is Likely Not a Security if Issued by a Bank

* The SEC has [stated](https://www.sec.gov/news/public-statement/sec-finhub-statement-occ-interpretation) that whether a stablecoin is a security or not depends on a variety of factors, including the rights it purports to convey, and how it is offered and sold.
* In [Marine Bank](https://caselaw.findlaw.com/us-supreme-court/455/551.html), the Supreme Court found that a certificate of deposit that was issued by a federally-chartered bank was not a security, since the purchaser of that instrument was already protected by the federal banking laws governing the issuer bank, including deposit insurance \(and therefore had no risk of loss\).
* A federal appeals court subsequently used the rationale of Marine Bank to reach the opposite result in [Gary Plastic](https://law.resource.org/pub/us/case/reporter/F2/756/756.F2d.230.84-7671.84-7541.252.365.html), finding that the federal banking laws did not eliminate risk \(including risk of loss\) to the purchaser of CDs that were created for and marketed by a broker-dealer.
* Here, similar to Marine Bank, the issuer of the stablecoin would be a regulated bank, and the corresponding fiat on deposit would be federally insured.

## **FAQs**

### **What is stablecoin?**

Stablecoin is a digital asset that is pegged to an external reference, such as another asset or basket of assets, with the purpose of minimizing price volatility.  Some stablecoins, such as the stablecoin here, have a “hard peg” of 1:1 to a fiat currency, such as the U.S. Dollar.  As a digital asset, stablecoins are issued and operate on blockchains, although the external reference, such as USD, may be maintained off-chain.

### **What are the benefits of stablecoin?**

Since stablecoins are blockchain-based, transactions that use stablecoin leverage the benefits of blockchain technology, including transaction immutability, finality, transparency and traceability.  Since a stablecoin is, by definition, designed to minimize price volatility, it provides certainty for use in transactions and therefore is a more effective medium of exchange than other types of digital assets.  Stablecoin is fully funded, and will move at the same time as the corresponding asset transfer for real-time settlement, thereby eliminating counterparty and settlement risk.

### **As a regulated financial institution, I have certain regulatory obligations.  What is Passport, and how will it help me meet those obligations?**

The Stablecoin Bank may leverage Figure’s Passport to comply with its BSA/AML regulatory obligations.

_Passport is a Provenance-based Figure application that was developed to assist Figure and other Provenance users to comply with laws and regulations including OFAC and the Bank Secrecy Act.  Passport will collect certain information from the applicant, including a valid, government-issued photo ID, tax identification number, and date of birth.  For corporate entities, Passport will collect documents including certificates of good standing and articles of incorporation.  The information that is submitted by the applicant is reviewed for authenticity and completeness.  Depending on the type of the applicant, Passport may also conduct a credit check.  In compliance with regulatory obligations, Passport maintains these on-boarding records for a prescribed period of time._

### **What fees are charged when creating and redeeming stablecoin?**

In order to facilitate the adoption of stablecoin, we would prefer that Stablecoin Banks initially collect minimal fees in connection with stablecoin creation or redemption.  A Stablecoin Bank, however, would collect the float from minting stablecoin, and could potentially collect fees related to minting, redeeming, or custodying stablecoin.

### **How would I account for stablecoin?**

A Stablecoin Bank could treat the stablecoin as either a sold asset or a fungible liability.  You should consult with your tax advisor to confirm.  


