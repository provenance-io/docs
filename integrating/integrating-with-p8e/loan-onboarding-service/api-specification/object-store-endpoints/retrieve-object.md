---
description: Retrieve and decrypt an object
---

# Retrieve Object

### Description

Used to retrieve and decrypt objects from the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

An object's `hash` and `type` must be passed such that the API can parse the fetched Protocol Buffer.

### Usage

**URL**: `https://{host}/p8e-cee-api/external/api/v1/eos`

**Method**: GET

**Headers**:

{% tabs %}
{% tab title="Local" %}
{% hint style="info" %}
Supply one `x-uuid` header when running locally.
{% endhint %}

| Key          | Value                     |
| ------------ | ------------------------- |
| Content-Type | multipart/form-data       |
| x-uuid       | \<Provenance Member UUID> |
{% endtab %}

{% tab title="Test/Production" %}
{% hint style="info" %}
Supply one `apikey` header when running in test or production environments..
{% endhint %}

| Key          | Value               |
| ------------ | ------------------- |
| Content-Type | multipart/form-data |
| apikey       | \<API Key>          |
{% endtab %}
{% endtabs %}

**Query Parameters**:

| Field              | Description                                                              | Data Type |
| ------------------ | ------------------------------------------------------------------------ | --------- |
| objectStoreAddress | The URL to the encrypted object store to run against                     | String    |
| hash               | Hash of the stored object                                                | String    |
| type               | Fully qualified class associated with the Protocol Buffer that is stored | String    |

**Response Status Codes**:

| Code | Meaning                   |
| ---- | ------------------------- |
| 200  | File retrieved            |
| 400  | Illegal/missing arguments |
| 404  | Object not found          |
| 500  | Server error              |

**Response Body**: JSON formatted Protocol Buffer

### Example

Assuming you followed the example found in the [Store Object Example](store-object.md#example) section to store a single `LoanState` Protocol Buffer, and received a resulting hash of `o0iERVQp7v7WdSkq8dlREr+yP8kEuAHvBMmprazYr0k=`, you could fetch that object using the following request:

```bash
curl --location --request GET 'localhost:8080/p8e-cee-api/external/api/v1/eos?objectStoreAddress=grpc://localhost:5001&hash=o0iERVQp7v7WdSkq8dlREr+yP8kEuAHvBMmprazYr0k=&type=tech.figure.servicing.v1beta1.LoanStateOuterClass$LoanState' \
--header 'x-uuid: deadbeef-face-479b-860c-facefaceface'
```

and receive the following output:

```json
{
  "id" : {
    "value" : "65baf01d-ee8b-4ad2-b646-cffe26640df3"
  },
  "effectiveTime" : "2022-05-22T00:00:00Z",
  "servicerName" : "Servicer One",
  "totalUnpaidPrinBal" : {
    "amount" : 250000.0,
    "currency" : "USD"
  },
  "accruedInterest" : {
    "amount" : 0.0,
    "currency" : "USD"
  },
  "dailyIntAmount" : {
    "amount" : 5.0,
    "currency" : "USD"
  },
  "loanStatus" : {
    "status" : "IN REPAY",
    "effectiveTime" : "2022-05-22T00:00:00Z"
  },
  "daysDelinquent" : 0,
  "remainingTermMonths" : 0,
  "autopayFlag" : false,
  "daysForbearance" : 0,
  "kv" : { }
}
```
