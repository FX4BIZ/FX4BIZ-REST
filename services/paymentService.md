# PAYMENT SERVICE #

Sending funds from your FX4BIZ wallet account to your own bank account or a third-party recipient involves two steps:

1. Generate the payment object with the [Submit Payment method](#post_payment). 
When you submit a payment to be scheduled, you assign a unique id to that payment. 

*Caution:* The payment created will be automatically rolled to the next closest working days if not confirmed in the scheduled date of operation.

2. Confirm the payment to the API for processing, using the [Confirm Payment method](#put_payments_confirm). 
When you confirm a payment for processing, make sure you have sufficient funds in your wallet account balance. The funds transfer will be automatically locked-in if the wallet account balance is not sufficient. Make sure you always have enough funds on your wallet.

*Caution:* If the balance of your wallet account is not sufficient to cover the payment amount, funds may be locked-in by FX4BIZ.

As an example, a response for `GET /payment/{id}` looks like this:
```js
"payment": {
    "id": "XXXX",
    "tag": "XXXXXX",
    "status": "Finalized",
    "createdDate": "2015-01-01 00:00:00",
    "desiredExecutionDate": "2015-01-01 12:00:00",
    "type": "Transfer",
    "beneficiaryName": "John Doe",
    "beneficiaryAccountNumber": "XXXXXXXXXXXXXXX",
    "amount": {
        "value": 10000.00,
        "currency": "USD"
    },
    "feeOption": "OUR",
    "communication": null
}
```

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /payment`](#post_payment)| Submit Payment |
| [`PUT /payment/{payment_id}/confirm`](#put_payments_confirm) | Confirm Payment |
| [`GET /payments`](#cget_payments) | Retrieve Payments History |
| [`GET /payment/{payment_id}`](#get_payments) | Retrieve Payment Details | 
| [`PUT /payment/{payment_id}`](#put-payment-details) | Update Payment Details |
| [`DELETE /payment/{payment_id}`](#delete-payment) | Cancel Payment |

## Details ##

#### <a id="post_payment"></a> Submit a payment ####

```
Method: POST 
URL: /payment
```
Use this path in order to schedule a new payment. 

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| accountId | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](../objects/objects.md#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| executionDate | Date | Initial execution date of you payment. `YYYY-MM-DD` |
| communication | String | A string representing the communication for the beneficiary. (76 chars max.) |
| tag | String | **Optionnal** The wording concerning the payment. |
| feeCurrency  | String | **Optionnal** A string representing the fee currency. By default, this is the payment currency. |
| chargeFeeOption | String | **Optionnal** A string representing the fee change option for this payment. The default fee option is `OUR`. `BEN | OUR` |
| priorityFeeOption | String | **Optionnal** A string representing wether this payment as a normal priority, or it as to be done quick. `normal | speed` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you submitted. |

**Example:**
```
/payments/
```
<hr />

#### <a id="put_payments_confirm"></a> Confirm a payment ####

```
Method: PUT 
URL: /payment/{id}/confirm
```
Payments that has been scheduled must be confirmed in order to be released. 
If the payment is not confirmed on scheduled date of operation, it will be postponed to the next operation date available.

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required** The ID of the payment you want to confirm. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you confirmed. |

**Example:**
```
/payments/89456/confirm
```
<hr />

#### <a id="cget_payments"></a> Retrieve Payments History ####

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

#### <a id="get_payments"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payments/{id}
```
Retrieve the details of a specific payment.

**Parameters:**

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

**Parameters:**

-> TBD

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you modified. |

**Example:**
```
/payments/89456
```
<hr />

#### <a id="delete-payment"></a> Cancel Payment ####

```
Method: DELETE
URL: /payment/{id}
```

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required** The ID of the payment you want to delete. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| process | Object | An object containing the result of the process. |
| Object.result | Boolean | A boolean value describing if the process succeeded or not. |
