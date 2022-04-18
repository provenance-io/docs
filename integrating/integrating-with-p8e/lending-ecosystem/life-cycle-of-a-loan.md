---
description: Translating the traditional life cycle of a loan to a blockchain-enabled world
---

# Life Cycle of a Loan

In order to build the suite of products mentioned on the previous page in such a way that they share one single source of truth, the development team needed to understand the life cycle of the asset from the point of view of each participant that touches it along the way.

There are many paths a loan can take after closing. Here is a sample of events that may take place along that route:

* **Register eNote** - Assuming the signed promissory note was digitally signed by the borrower, loan originators are required to store the authoritative copy of the document in an eVault that complies with several regulatory statutes. The eVault must provide security and track control of the document. Control of the document can then be reassigned by the appropriate parties.
* **Funding** - As a loan is closed, disbursements are made to the borrower. These funds may come from the lender themselves or in combination with funds from a warehouse lender. Traditionally, funds are disbursed over the ACH network.
* **Servicing** - After closing, loans are serviced by companies that specialize in interacting with borrowers to collect repayments or take any adverse actions should the borrower fail to repay on time.
* **Pledge to Warehouse Lenders** - If they weren't involved pre-funding, loan originators can pledge the loan repayments to a warehouse lender in exchange for control over the eNote.
* **Pool Loans for Sale** - Loan originators typically do not hold loans on their books for the entire duration of the payback period. Instead they sell loans to investors and continue to cycle of originating new loans with the money they get back. Loans are sold in pools containing hundreds of loans.
* **Third-Party Validation** - As part of the loan sale process, investors perform due diligence which may include a third-party audit of loan data and documents. Third-party validators will validate loans against a myriad of rules, ensuring the underwriting process met certain criteria required by the investor and/or credit rating agencies.
* **Loan Sale** - Finally, loans are sold to investors and can be resold any number of times.

As you can imagine, the participants in this ecosystem traditionally communicate directly with each other and store data in their own personally-tailored data models. Integrating with a new partner takes significant development time.

### Blockchain Translation

In a blockchain connected world, participants share a semi-flexible data model that leans on industry standards wherever possible. While each participant will likely integrate an existing system with the network, they adopt a mutually beneficial data model for data that gets shared with other participants. By sharing one common data model, participants don't need to integrate with each additional participant that joins the consortium.

Let's explore the Loan Package data model on the next page.
