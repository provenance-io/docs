---
description: Encrypt and store any file
---

# Store File

### Description

Used to encrypt and store raw files in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.

Files are passed in as multi-part form objects.

{% hint style="info" %}
See [API Key](../../#api-key-for-test-or-production-environments) and [Permissioning Others](../../../p8e-contract-execution-environment-p8e/key-management/permissioning-others.md) sections for more detail on which keys are used for encryption.
{% endhint %}

### Usage

**URL**: `https://{host}/api/v1/eos/file`

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

**Form Data**:

| Field              | Description                                                                                                                                           | Data Type |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| objectStoreAddress | The URL to the encrypted object store to run against                                                                                                  | String    |
| id                 | File Identifier                                                                                                                                       | String    |
| file               | File to be encrypted and stored                                                                                                                       | FilePart  |
| permissions        | Object containing PermissionInfo (See [Permissioning Others](../../../p8e-contract-execution-environment-p8e/key-management/permissioning-others.md)) | JSON      |

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

To follow along, download the `sample.pdf` file below or use any file on your machine.

{% file src="../../../../../.gitbook/assets/sample.pdf" %}
A simple PDF that can be used as an example
{% endfile %}

```bash
curl --location --request POST 'localhost:8080/p8e-cee-api/external/api/v1/eos/file' \
--header 'x-uuid: deadbeef-face-479b-860c-facefaceface' \
--form 'objectStoreAddress="grpc://localhost:5001"' \
--form 'id="foo"' \
--form 'file=@"<path_to_file>"' \
--form 'permissions="{\"permissionDart\":true,\"permissionPortfolioManager\":true}"'
```

{% hint style="info" %}
Swap out the \<path\_to_\__file> with a real path to the file you wish to upload.
{% endhint %}

#### Sample Response

```json
{
    "hash": "gE8i+JHSwPss2y6sEs41m2I1o6M+NZaPeSTEudtqosw=",
    "uri": "object://localhost:8080/gE8i+JHSwPss2y6sEs41m2I1o6M+NZaPeSTEudtqosw=",
    "bucket": "/mnt/data",
    "name": "b923d6fb-ae73-4ace-8831-433512e83269"
}
```
