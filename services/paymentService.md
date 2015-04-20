# PAYMENT SERVICE #

Sending funds from your FX4BIZ wallet account to your own bank account or a third-party recipient involves two steps:

1. Generate the payment object with the [Create Payment method](#submit-payment). 
When you submit a payment to be scheduled, you assign a unique id to that payment. 

*Caution:* The payment created will be automatically rolled to the next closest working days if not confirmed in the scheduled date of operation.

2. Confirm the payment to the API for processing, using the [Confirm Payment method](#confirm-payment). 
When you confirm a payment for processing, make sure you have sufficient funds in your wallet account balance. The funds transfer will be automatically locked-in if the wallet account balance is not sufficient. Make sure you always have enough funds on your wallet.

*Caution:* If the balance of your wallet account is not sufficient to cover the payment amount, funds may be locked-in by FX4BIZ.

As an example, a response for `GET /payment/{:id}` looks like this:
```js
{
    "payment": {
        "id": "xxx",
        "status": "Awaiting Confirmation",
        "type": "Standard",
        "tag": "Invoice xxx",
        "created_date": "2014-01-12T00:00:00+00:00",
        "initial_execution_date": "2014-01-12T00:00:00+00:00",
        "amount": {
            "value": "125000.00",
            "currency": "USD"
        "account": {
            "account_id": "xxx"
            "status": "active",
            "type": "wallet",
            "created_date": "2014-01-12T00:00:00+00:00",
            "created_by": "api",
            "tag": "John Doe wallet account with FX4BIZ",
            "number": "xxx4548",
            "currency": "EUR",
            "correspondant_bank":{
                "bic": "AGRIFRPP",
                "name": "CREDIT AGRICOLE SA",
                "address": {
                    "street": "BUILDING PASTEUR, BLOC 1: 91-93, BOULEVARD PASTEUR",
                    "post_code": "75015",
                    "city_name": "Paris",
                    "state_or_province": "",
                    "country": "FRANCE"
                }
            },
            "beneficiary_bank": {
                "bic": "FXBBBEBB",
                "name": "FX4BIZ SA",
                "address": {
                    "street": "Avenue Louise, 350",
                    "post_code": "1050",
                    "city_name": "Bruxelles",
                    "state_or_province": "Bruxelles-Capitale",
                    "country": "FR"
                }
            },
            "beneficiary": {
                "name": "John Doe",
                "type": "individual",
                "address": {
                    "street": "1 My Road",
                    "post_code": "ZIP",
                    "city_name": "London",
                    "state_or_province": "",
                    "country": "UK"
                }
            }
        }
    }
}
```

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /payment`](#submit-payment)| Submit Payment |
| [`PUT /payment/{payment_id}/confirm`](#confirm-payment) | Confirm Payment |
| [`GET /payments`](#get-payment-history) | Retrieve Payments History |
| [`GET /payment/{payment_id}`](#get-payment-history) | Retrieve Payment Details | 
| [`PUT /payment/{payment_id}`](#put-payment-details) | Update Payment Details |
| [`DELETE /payment/{payment_id}`](#delete-payment) | Cancel Payment |

## Details ##

#### <a id="submit-payment"></a> Submit a payment ####

```
Method: POST 
URL: /payment
```
Use this path in order to schedule a new payment. 

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| account_id | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](../objects/objects.md#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response to this query, you will receive the details of the [Payment](../objects/objects.md#payment_object) created.

<hr />

#### <a id="confirm-payment"></a> Confirm a payment ####

```
Method: PUT 
URL: /payment/{payment_id}/confirm
```
Payments that has been scheduled must be confirmed in order to be release. 
If the payment is not confirmed on scheduled date of operation, it will be postponed to the next operation date available.

As a response to this query, you will receive the updated details of the [Payment](../objects/objects.md#payment_object) confirmed.

<hr />

#### <a id="get-payments-history"></a> Retrieve payments history ####

```
Method: GET
URL: /payments
```
Request the list of payments that has been created on a specific period of time.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| from_date | Date | `YYYY-MM-DD` |
| to_date | Date | `YYYY-MM-DD` | 

As a response to this query, you will receive an Array with the `payment_id`, `creation_date`, `execution_date` and `amount`  of the [Payment] of all payment created.

<hr />

#### <a id="get-payment-details"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payment
```
Retrieve the details of a specific payment.

As a response to this query, you will receive the details of the [Payment](../objects/objects.md#payment_object).

<hr />

#### <a id="put-payment"></a> Update Payment Details####

```
Method: PUT
URL: /payment/{payment_id}
```
Update information on a specific payment.

As a response to this query, you will receive the updated details of the [Payment](../objects/objects.md#payment_object).

<hr />

#### <a id="delete-payment"></a> Cancel Payment ####

```
Method: DELETE
URL: /payment/{payment_id}
```

As a response to this query, you will receive a confirmation that the [Payment](../objects/objects.md#payment_object) is deleted properly.
