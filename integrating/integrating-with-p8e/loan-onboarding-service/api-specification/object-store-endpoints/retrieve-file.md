---
description: Retrieve and decrypt a file
---

# Retrieve File

### Description

Used to retrieve and decrypt raw files from the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

### Usage

**URL**: `https://{host}/p8e-cee-api/external/api/v1/eos/file`

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

**Form Data**:

| Field              | Description                                          | Data Type |
| ------------------ | ---------------------------------------------------- | --------- |
| objectStoreAddress | The URL to the encrypted object store to run against | String    |
| hash               | Hash of the stored file                              | String    |

**Response Status Codes**:

| Code | Meaning                   |
| ---- | ------------------------- |
| 200  | File retrieved            |
| 400  | Illegal/missing arguments |
| 404  | File not found            |
| 500  | Server error              |

**Response Body**: File bytes

### Example

Assuming you followed the example found in the [Store File Example](store-file.md#example) section to store a single file, and received a resulting hash of `gE8i+JHSwPss2y6sEs41m2I1o6M+NZaPeSTEudtqosw=`, you could retrieve that file with the following request:

```bash
curl --location --request GET 'localhost:8080/p8e-cee-api/external/api/v1/eos/file?objectStoreAddress=grpc://localhost:5001&hash=gE8i+JHSwPss2y6sEs41m2I1o6M+NZaPeSTEudtqosw=' \
--header 'x-uuid: deadbeef-face-479b-860c-facefaceface'
```

{% hint style="info" %}
Swap out the hash in the example with the resulting hash from your call to store the file if it differs from the one above.
{% endhint %}
