---
description: Approve and sign a p8e contract envelope
---

# Approve Contract

### Description

Used by additional participants in a multi-party contract execution to respond with approval by executing the p8e contract and updating the scope to include their approval, sending the record of their approval to the Provenance Blockchain network for memorialization.

Approval creates an ephemeral Authz grant on behalf of the approver, allowing the proposing party to submit the fully-formed, signed transaction to the Provenance Blockchain network prior to the expiration date set by the approver.

Upon successful approval, contract execution passes back to the original proposing party via the Object Store's mailbox feature.

### Usage

**URL**: `https://{host}/api/v1/cee/approve`

**Method**: POST

**Headers**:

{% tabs %}
{% tab title="Local" %}
{% hint style="info" %}
Supply one `x-uuid` header when running locally.
{% endhint %}

| Key          | Value                     |
| ------------ | ------------------------- |
| Content-Type | application/json          |
| x-uuid       | \<Provenance Member UUID> |
{% endtab %}

{% tab title="Test/Production" %}
{% hint style="info" %}
Supply one `apikey` header when running in test or production environments..
{% endhint %}

| Key          | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |
| apikey       | \<API Key>       |
{% endtab %}
{% endtabs %}

**Request Body**:

```
{
    "config": {
        "account": {
            "partyType": ""
        },
        "client": {
            "objectStoreUrl": ""
        },
        "provenanceConfig": {
            "chainId": "",
            "nodeEndpoint": "",
            "gasAdjustment": 1.5
        }
    },
    "envelope": [
        ""
    ],
    "expiration": "YYYY-MM-DDTHH:mm:ss.SSSZ"
}
```

| Field                          | Description                                                                                                                                | Data Type     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------- |
| account/partyType              | Party type of the provenance member invoking the contract. This type should be associated with the UUID (or API Key) passed in the header. | String        |
| client/objectStoreUrl          | URL to the encrypted object store to run against.                                                                                          | String        |
| provenanceConfig/chainId       | Unique identifier (name) of the Provenance Blockchain network.                                                                             | String        |
| provenanceConfig/nodeEndpoint  | URL to the Provenance Blockchain node where the transaction will be submitted.                                                             | String        |
| provenanceConfig/gasAdjustment | Multiplier applied to estimated gas prior to submitting the transaction.                                                                   | Double        |
| envelope                       | List of objects containing signatures and information about the contract execution including its status and expiration.                    | List\<String> |
| expiration                     | Expiration date and time of the approval                                                                                                   | Timestamp     |

**Response Status Codes**:

| Code | Meaning              |
| ---- | -------------------- |
| 204  | Transaction approved |
| 400  | Illegal arguments    |
| 500  | Server error         |

### Example

#### Sample Request

Coming Soon!

