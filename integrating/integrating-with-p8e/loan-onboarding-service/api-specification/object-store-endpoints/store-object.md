---
description: Encrypt and store any object represented as a Protocol Buffer
---

# Store Object

### Description

Used to encrypt and store objects in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

Objects must be passed as a JSON representation of a [Google Protocol Buffer message](https://developers.google.com/protocol-buffers/docs/overview). The message will be parsed, then stored.

{% hint style="info" %}
See [API Key](../../#api-key-for-test-or-production-environments) and [Permissioning Others](../../../p8e-contract-execution-environment-p8e/key-management/permissioning-others.md) sections for more detail on which keys are used for encryption.
{% endhint %}

### Usage

**URL**: `https://{host}/api/v1/eos`

**Method**: POST

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

**Request Body**:

```json
{
  "objectStoreAddress": "",
  "permissions": {
    "audiences": [],
    "permissionDart": true,
    "permissionPortfolioManager": true,
  },
  "message": {}
  "type": ""
}
```

| Field                                  | Description                                                                                                                                    | Data Type                     |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| objectStoreAddress                     | The URL to the encrypted object store to run against                                                                                           | String                        |
| permissions/audiences                  | Additional audiences that should be allowed permission to query against the saved data in EOS                                                  | List\<Base64EncodedPublicKey> |
| permissions/permissionDart             | If the dart product should be allowed permission against the saved data in EOS.                                                                | Bool                          |
| permissions/permissionPortfolioManager | If the portfolio manager product should be allowed permission against the saved data in EOS.                                                   | Bool                          |
| message                                | The asset that is stored against the EOS. This can be any JSON formatted Google Protocol Buffer that matches the type provided in this request | JSON                          |
| type                                   | The fully qualified type url matching the protobuf object provided                                                                             | String                        |

**Response Status Codes**:

| Code | Meaning                  |
| ---- | ------------------------ |
| 200  | File accepted and stored |
| 400  | Illegal arguments        |
| 500  | Server error             |

**Response Body**:

```json
{
  "hash": "string",
  "uri": "string",
  "bucket": "string",
  "name": "string"
}
```

| Field  | Description                                  | Data Type |
| ------ | -------------------------------------------- | --------- |
| hash   | The returned hash of the saved object in EOS | String    |
| uri    | The location of the saved object             | String    |
| bucket | The volume path of the saved data            | String    |
| name   | The name of the saved data                   | String    |

### Example

#### Sample Request

The following example stores a `LoanState` object, representing the current state of an outstanding loan.

```shell
curl --location --request POST 'localhost:8080/p8e-cee-api/external/api/v1/eos' \
--header 'x-uuid: deadbeef-face-479b-860c-facefaceface' \
--header 'Content-Type: application/json' \
--data-raw '{
    "objectStoreAddress": "grpc://localhost:5001",
    "permissions": {
        "permissionDart": true,
        "permissionPortfolioManager": true
    },
    "message": {
        "id": {
            "value": "65baf01d-ee8b-4ad2-b646-cffe26640df3"
        },
        "effectiveTime": "2022-05-22T00:00:00.000Z",
        "servicerName": "Servicer One",
        "totalUnpaidPrinBal": {
            "amount": 250000.00,
            "currency": "USD"
        },
        "accruedInterest": {
            "amount": 50.00,
            "currency": "USD"
        },
        "dailyIntAmount": {
            "amount": 5.00,
            "currency": "USD"
        },
        "loanStatus": {
            "status": "IN REPAY",
            "effectiveTime": "2022-05-22T00:00:00.000Z"
        }
    },
    "type": "tech.figure.servicing.v1beta1.LoanStateOuterClass$LoanState"
}'
```

#### Sample Response

```json
{
    "hash": "o0iERVQp7v7WdSkq8dlREr+yP8kEuAHvBMmprazYr0k=",
    "uri": "object://localhost:8080/o0iERVQp7v7WdSkq8dlREr+yP8kEuAHvBMmprazYr0k=",
    "bucket": "/mnt/data",
    "name": "NOT_STORAGE_BACKED"
}
```
