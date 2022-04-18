---
description: Translating the traditional life cycle of a loan to a blockchain-enabled world
---

# Life Cycle of a Loan

In order to build the suite of products mentioned on the previous page in such a way that they share one single source of truth, the development team needed to understand the life cycle of the asset from the point of view of each participant that touches it along the way.

There are many paths a loan can take after closing. Here is a sample of events that may take place along that route:

* **Register eNote** - Assuming the signed promissory note was digitally signed by the borrower, loan originators are required to store the authoritative copy of the document in an eVault that complies with several regulatory statutes. The eVault must provide security and track control of the document. Control of the document can then be reassigned by the appropriate parties.
* **Funding** - As a loan is closed, disbursements are made to the borrower. These funds may come from the lender themselves or in combination with funds from a warehouse lender. Traditionally, funds are disbursed over the ACH network.
* **Servicing** - After closing, loans are serviced by companies that specialize in interacting with borrowers to collect repayments or take any adverse actions should the borrower fail to repay on time. Servicing companies have the latest up-to-date information investors need to value loans.
* **Pledge to Warehouse Lenders** - If they weren't involved pre-funding, loan originators can pledge the loan repayments to a warehouse lender in exchange for control over the eNote. This exchange could mean waiting for the signed promissory note to arrive in the mail while a wire transfer takes place.
* **Pool Loans for Sale** - Loan originators typically do not hold loans on their books for the entire duration of the payback period. Instead they sell loans to investors and continue the cycle of originating new loans with the money they get back. Loans can be sold in pools containing hundreds of loans.
* **Third-Party Validation** - As part of the loan sale process, investors perform due diligence which may include a third-party audit of loan data and documents. Third-party validators will validate loans against a myriad of rules, ensuring the underwriting process met certain criteria required by the investor and/or credit rating agencies.
* **Loan Sale** - Finally, loans are sold to investors and can be resold any number of times. Again, money changes hands via wire transfer while documents arrive in the mail.

As you can imagine, the participants in this ecosystem traditionally communicate directly with each other and store data in their own personally-tailored data models. Integrating with a new partner, no matter how technically capable both sides are, takes significant development time.

### Blockchain Translation

In a blockchain connected world, participants share a semi-flexible data model that leans on industry standards wherever possible. While each participant will likely integrate an existing system with the network, they adopt a mutually beneficial data model for data that gets shared with other participants. By sharing one common data model, participants don't need to integrate with each additional participant that joins the consortium.

Furthermore, participants in a blockchain network agree to rules for changing the state of an asset, which get codified in smart contracts. In the lending example, you will see several examples of contracts that get called at each stage in the asset life cycle to update the distributed world state of the Loan Package. These contracts are invoked by the party that has the data the rest of the network needs. For example, when recording validation results, a third-party validator can invoke a contract that will update the `Validation` Record with the results of their off-chain work. Those results then live with the Loan Package for the remainder of its life.

Let's explore the Loan Package data model in more detail on the next page.
