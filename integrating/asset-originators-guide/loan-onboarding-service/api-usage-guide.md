---
description: How to onboard a loan
---

# API Usage Guide

At this point you've learned more about some key concepts, and looked under the covers at the loan package data model, p8e contract specifications, and onboarding API specifications. Whether you're operating your own p8e Contract Execution Environment or just playing with the sandbox, you're ready to onboard a loan to Provenance. This page will point you in the right direction.

{% hint style="info" %}
The intended audience for this page is a loan originator who is already operating their own Loan Origination System (LOS). It describes onboarding a mortgage, but could be loosely followed to onboard any type of loan or asset.
{% endhint %}

## Loan Onboarding

As a borrower moves throughout the application stage of the mortgage process, data and documents are collected by the loan originator in their LOS. Once the application reaches an appropriate milestone, such as closing, the loan originator can start onboarding the loan to Provenance.

### Onboard Loan Documents

Recall that in the [loan package data model](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/data-mapping), documents are stored separately from the loan application and underwriting data. For this reason, documents get inserted into the p8e Encrypted Object Store (EOS) separately from the loan package. Rather than store every byte of every loan document within the loan package scope, the scope will end up containing a list of loan document metadata, with each entry containing the URI to the actual object.

Therefore, step 1 of the loan onboarding process is to start inserting documents into the EOS as they come. The [Create Object](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-object-in-object-store) endpoint handles creating individual objects in the object store without memorializing them as scopes on Provenance. The curl command below provides an example.

```
// curl command to insert document(s)
...
```

The "asset" in this case is the loan document represented as a base 64 encoded byte array. It's important to consider whether or not to permission certain other participants at this stage. Figure Tech recommends permissioning the following:

* **Portfolio Manager -** Permission if you intend on accessing, pledging, or selling this loan on Portfolio Manager.&#x20;
* **DART** - Permission if you intend on registering this loan and the associated eNote with DART, which will automatically track life of loan updates.
* **3rd Party Validators** - Permission to 3rd party validators if you intend on requesting validation services for this loan.

{% hint style="info" %}
Figure can also integrate with 3rd party document preparation vendors to receive documents directly. However, it is important for loan originators to keep track of the metadata returned by the object store when it creates a document. They will need that list when it comes time to create the loan scope.
{% endhint %}

{% hint style="info" %}
Figure Tech will provide the appropriate object store path for loan originators following the white-label approach.
{% endhint %}

### Onboard Loan Package

Once documents are stored in EOS, and the loan application process is in a stage where it is worth onboarding the loan data to Provenance, the loan originator should again hit the [Create Object](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#create-object-in-object-store) endpoint. This time, however, instead of a document being passed as the "asset", you will use the Loan Package protocol buffer.

{% hint style="info" %}
Ultimately the stage at which an originator onboards a loan to Provenance is a personal business decision. Remember that there is a fee to write each update to the loan scope in the form of [gas](../../../blockchain/basics/gas-and-fees.md). A suggestion would be to onboard the loan either at the point in which you would no longer want to update the base loan data, or at the point you would want to share one common set of loan application data with a 3rd party, such as a validator.
{% endhint %}

The curl command below will onboard a loan package to the EOS by executing the Record Loan p8e contract, and return a fully formed Provenance Blockchain transaction proposal containing the appropriate hashes of the inputs and outputs of that contract.

```
// curl command to onboard loan scope
...
```

{% hint style="info" %}
Again, it is recommended that loan originators carefully consider which downstream services, such as Portfolio Manager and DART, they'd like to permission at this stage. It can save time and gas fees later to permission business partners like Figure Tech and/or 3rd Party Validators during the initial onboard.
{% endhint %}

### Submit Transaction Proposal to the Provenance Blockchain Network

Now that the p8e scope has been created for the loan package, the loan originator will call the API one last time to send the transaction proposal to the Provenance Blockchain memory pool, where it will get picked up by validators and memorialized as a scope on the Provenance Blockchain ledger.

Taking the fully formed transaction that was returned in the previous step, loan originators can hit the [Onboard](https://docs.provenance.io/integrating/asset-originators-guide/loan-onboarding-service/api-specification#onboard-on-provenance) endpoint to sign and send the transaction proposal to the memory pool. The curl command below provides and example.

```
// curl command to send the transaction proposal
...
```

The API will automatically estimate gas fees associated with this transaction. For more information, see the [Gas and Fees](../../../blockchain/basics/gas-and-fees.md) documentation.

{% hint style="info" %}
Executing a transaction on any blockchain network is an asynchronous process, and Provenance Blockchain is not different. There will be time between sending a transaction proposal to the memory pool and that transaction being picked up and inserted into a block that gets written to the Provenance Blockchain chain. Be sure to read on to learn how to handle common errors.
{% endhint %}

### What could go wrong?



### Why not use one endpoint for all of the above?



## Querying for Existing Loan Scope



1. Loan onboarding order of operations and what gets created/returned with each step
   1. Include images with data flow
   2. How to calculate the gas/fees
   3. How to interpret errors/what could wrong at each stage
2. Querying for data you've onboarded
3. More code
   1. Client side code to hit the API
   2. curl calls
   3. Postman collection?
4. Playground/sandbox
5. Eventually put a video up
