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
"payment": {
    "id": "XXXX",
    "tag": "XXXXXXXXXXX",
    "status": "Planified",
    "createdDate": "2015-04-24 10:50:24",
    "desiredExecutionDate": "2015-04-24 00:00:00",
    "executionDate": "2015-04-24 00:00:00",
    "type": "Transfer",
    "beneficiaryName": "John Doe",
    "beneficiaryAccountNumber": "XXXXXXXXXXXXXX",
    "amount": {amount},
    "feeOption": "SEPA",
    "communication": "Payment to Jane Doe",
    "mail": "mail@example.com"
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
| accountId | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](../objects/objects.md#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| executionDate | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response to this query, you will receive the details of the [Payment](../objects/objects.md#payment_object) created.

<hr />

#### <a id="confirm-payment"></a> Confirm a payment ####

```
Method: PUT 
URL: /payment/{payment_id}/confirm
```
Payments that has been scheduled must be confirmed in order to be released. 
If the payment is not confirmed on scheduled date of operation, it will be postponed to the next operation date available.

As a response to this query, you will receive the updated details of the [Payment](../objects/objects.md#payment_object) confirmed.

<hr />

#### <a id="get-payments-history"></a> Retrieve Payments History ####

```
Method: GET
URL: /payments
```
Request the list of payments that has been created on a specific period of time.

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| fromDate | Date | **Optionnal** A date representing the starting date to search payments. |
| toDate | Date | **Optionnal** A date representing the ending date to search payments. | 

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payments | Array[[Payment Object](../objects/objects.md#payment_object)] | An array of [Payment Object](../objects/objects.md#payment_object) describing payments you made. |

**Example:**
```
/payments/
```

<hr />

#### <a id="get-payment-details"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payment
```
Retrieve the details of a specific payment.

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required** The ID of the payment you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you requested. |

**Example:**
```
/payments/89456
```
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
