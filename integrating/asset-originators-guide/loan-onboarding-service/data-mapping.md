---
description: How to build data facts for the Figure Loan Model
---

# Data Mapping

## Data Format

Each [Fact](../../../p8e/overview/#facts) in a [Scope](../../../p8e/overview/#scopes) is a key-value pair, where the key is a String name and the value is a protobuf object. [Google Protocol Buffers](https://developers.google.com/protocol-buffers) support code generation in many languages. The [Figure Loan Model ](../../../provenance-applications/loan-origination-system-los/assets.md)lists the fact names and protobuf types used in Figure's loan scope.&#x20;

{% hint style="info" %}
Data type documentation, `.proto` files, and Java bindings for the loan model are available by request from Figure.
{% endhint %}

## Examples

Building the `"loan"` data fact from the `Loan` protobuf:

```kotlin
import io.provenance.proto.loan.LoanProtos

val loan = LoanProtos.Loan.newBuilder().apply {
    uuid = randomProtoUuidProv()
    originatorUuid = originatorUUID
    monthlyPaymentAmount = "100".toProtoMoneyProv()
    originatorName = "FIGURE EXAMPLE LENDER"
    loanNumber = "LOAN_NUM-1234"
    loanType = "FIGURE_HELOC"
}.build()
```

Using the `loan` as an input to a P8e contract:

```kotlin
contract.addProposedFact("loan", loan)
```

An example loan with minimal fields populated:

```kotlin
{
  "funding": {
    "complete": true,
    "completedDate": "2020-09-10T22:34:13.703547Z"
  },
  "loan": {
    "uuid": {
      "value": "b3df92bc-3d23-4970-b542-740647f6f896"
    },
    "loanNumber": "LOAN_NUM-1234",
    "loanType": "PERSONAL_LOAN",
    "originatorUuid": {
      "value": "deadbeef-face-479b-860c-facefaceface"
    },
    "originatorName": "EXAMPLE LENDER",
    "monthlyPaymentAmount": {
      "amount": "100",
      "currency": "USD"
    }
  },
  "primaryParty": {
    "uuid": {
      "value": "b071fd29-c49a-4950-9f9f-cd3447632a29"
    },
    "name": {
      "firstName": "Jane",
      "lastName": "Smith"
    }
  },
  "servicing": {
    "uuid": {
      "value": "deadbeef-face-479b-860c-facefaceface"
    }
  },
  "underwritingPacket": {
    "selectedOffer": {
      "amount": {
        "amount": "25000",
        "currency": "USD"
      },
      "termInYears": 5,
      "intRate": {
        "value": "0.075"
      }
    },
    "propertyAttributes": {
      "postLoanAdjCltv": {
        "value": "0.05"
      }
    }
  }
}
```

