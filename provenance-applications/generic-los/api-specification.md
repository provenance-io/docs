# API Specification

The generic LOS exposes three REST API endpoints for interactions with Provenance.&#x20;

### Posting Specifications

`https://figure.com/service-loan-onboarding/secure/api/v1/specifications`

Used to write specifications to Provenance. See [Specifications](https://docs.provenance.io/p8e/p8e-usage/specifications) for additional information.&#x20;

#### Example Request

```
{
    "chainId": "pio-testnet-1",
    "nodeEndpoint": "tcp://rpc-0.test.provenance.io:26657",
    "keyMnemonic": "walnut bubble shoe neck broccoli elevator assume puzzle business baby gentle suffer equal duty nephew domain adjust spin cigar response what sniff clip garment",
    "isTestNet": true,
    "keyRingIndex": "0",
    "keyIndex": "0",
    "scopeId": "4e554cb8-56dd-48df-b3fe-71f4c5b7d2cf",
    "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
    "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f"
}
```

### Posting Objects to Object-Store

Used to store objects in the object store. See [Encrypted Object Store ](https://docs.provenance.io/p8e/overview/encrypted-object-store)for additional information.&#x20;

#### Example Request

`curl -X POST -F 'file=@path' -F 'config=<JsonConfig> https://figure.com/service-loan-onboarding/secure/api/v1/store`

`T`he configuration object is a JSON object with the following fields

```
{
  "objectStoreAddress": "grpc://object-store-v2.p8e:80",
  "audiences": [],
  "permissionDart": true,
  "permissionPortfolioManager": true,
  "onboardUri": "",
  "chainId": "pio-testnet-1",
  "nodeEndpoint": "tcp://rpc-0.test.provenance.io:26657",
  "keyMnemonic": "jealous bright oyster fluid guide talent crystal minor modify broken stove spoon pen thank action smart enemy chunk ladder soon focus recall elite pulp",
  "isTestNet": true,
  "keyRingIndex": "0",
  "keyIndex": "0",
  "explorerUri": "https://explorer.test.provenance.io",
  "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
  "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b"
}
```

#### Example Response

```
{
    "hash": "iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "uri": "object://test.figure.com/iUVSVy1QmyKSxWbxz2JEbCM2jxB9gOVta8KlbqloZsc=",
    "bucket": "/mnt/data",
    "name": "42b2f0a8-dc40-470f-8b29-6de71486d2ff"
}
```

### Posting Assets to Provenance

`https://figure.com/service-loan-onboarding/secure/api/v1/onboard`

Used to onboard assets to Provenance.&#x20;

#### Example Request

```
curl -XPOST -H "Content-type: application/json" -d '{
    "asset": {
        "id": {
            "value": "d4f6c6aa-6eef-4070-8c5d-48e622c170eb"
        },
        "type": "enote",
        "description": "Dart Enote",
        "kv": {
            "document": {
                "@type": "/tech.figure.util.v1beta1.DocumentMetadata",
                "id": {
                    "value": "c6978d46-3c3e-4175-a0d2-8f8ce47e8bb6"
                },
                "uri": "object://test.figure.com/hnxit65lhjoIKiERyjeQvQSL9Ia7l1duaO9GFiQubTM=",
                "fileName": "enote.xml",
                "documentType": "MISMO_ENOTE_SMART_DOC_XML",
                "checksum": {
                    "checksum": "hnxit65lhjoIKiERyjeQvQSL9Ia7l1duaO9GFiQubTM="
                }
            }
        }
    },
    "info": {
        "chainId": "pio-testnet-1",
        "nodeEndpoint": "grpc://192.168.1.242:9090",
        "keyMnemonic": "walnut bubble shoe neck broccoli elevator assume puzzle business baby gentle suffer equal duty nephew domain adjust spin cigar response what sniff clip garment",
        "isTestNet": true,
        "keyRingIndex": "0",
        "keyIndex": "0",
        "scopeSpecId": "551b5eca-921d-4ba7-aded-3966b224f44b",
        "contractSpecId": "f97ecc5d-c580-478d-be02-6c1b0c32235f",
        "hash": "hnxit65lhjoIKiERyjeQvQSL9Ia7l1duaO9GFiQubTM="
    }
}' 'https://figure.com/service-loan-onboarding/secure/api/v1/onboard'
```
