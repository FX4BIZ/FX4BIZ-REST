# PAYMENT SERVICE # 

Sending funds from your FX4BIZ wallet account to your own bank account or a third-party recipient involves two steps:

1. Generate the payment object with the [Submit Payment method](#post_payment). 
When you submit a payment to be scheduled, you assign a unique id to that payment. <br />**Caution:** The payment created will be automatically rolled to the next closest working days if not confirmed in the scheduled date of operation.

2. Confirm the payment to the API for processing, using the [Confirm Payment method](#put_payments_confirm). 
When you confirm a payment for processing, make sure you have sufficient funds in your wallet account balance. 
<br />**Caution:** If the balance of your wallet account is not sufficient to cover the payment amount, funds may be locked-in by FX4BIZ.

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /payments/`](#post_payments)| Submit a Payment |
| [`PUT /payments/-{id}/confirm`](#put_payments_confirm) | Confirm Payment |
| [`GET /payments/`](#cget_payments) | Retrieve Payments History |
| [`GET /payments/-{id}`](#get_payments) | Retrieve Payment Details | 
| [`DELETE /payments/-{id}`](#delete_payments) | Cancel Payment |

## Details ##

#### <a id="post_payments"></a> Submit a payment ####

```
Method: POST 
URL: /payments/
```
You can use this request in order to schedule a new payment. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| sourceWalletId | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the wallet from which the payment must be processed. |
| externalBankAccountId | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the recipient account. |
| amount | [Amount Object](../objects/objects.md#amount_object) | Required | Amount to be sent. <br />**Caution.** The currency of the amount sent must be equal to the currency of the beneficiary account. |
| desiredExecutionDate | [Date](../conventions/formattingConventions.md#type_date) | Required | The initial execution date of your payment. `YYYY-MM-DD` |
| feeCurrency  | [Currency](../conventions/formattingConventions.md#type_currency) | Required | A three digit code representing the currency related to the charges applied on your payment. |
| feePaymentOption | String | Required | A code representing the charges option to be applied to this payment. `BEN | OUR | SHARE` |
| priorityPaymentOption | String | Required | A code representing whether this payment as a standard priority, or a priority treatment in FX4BIZ an correspondent bank systems. `normal | urgent` |
| tag | String(50) | Optional | A custom reference that you want to be related to this payment in the system. This tag is not communicated to the beneficiary. |
| communication | String(76) | Optional | A Free format string representing the communication for the beneficiary. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | An encoded JSON body describing payment instruction submitted. |

**Example:**
```js
POST /payments/
{
    "externalBankAccountId": "ND4xBE",
    "amount": {
        "value": "100000",
        "currency": "USD"
    },
    "desiredExecutionDate": "2015-09-15",
    "feeCurrency": "USD",
    "feePaymentOption": "BEN",
    "tag": "payment #1535",
    "priorityPaymentOption": "urgent",
    "communication": "billing  for order #15135413",
    "sourceWalletId": "ND5aHB"
}
```
<hr />

#### <a id="put_payments_confirm"></a> Confirm a payment ####

```
Method: PUT 
URL: /payment/-{id}/confirm
```
Payments that has been scheduled must be confirmed in order to be released.<br />
If the payment is not confirmed before the end of scheduled date of operation, it will be automatically postponed to the next operation date available.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the payment you want to confirm. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | An encoded JSON body advising the modification on payment status. |

**Example:**
```js
PUT /payments/-TDK4eB/confirm
```
<hr />

#### <a id="cget_payments"></a> Retrieve Payments History ####

```
Method: GET
URL: /payments/_{status}/
```
Request the list of payments that has been created on a specific period of time.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| status | String |  Required | A code representing the status of the trades you want to get. `all | planified | rejected | finalized | canceled | refused | blocked | waitingconfirmation ` | 
| fromDate | [Date](../conventions/formattingConventions.md#type_date) |  Optional | The starting date to search payments. |
| toDate | [Date](../conventions/formattingConventions.md#type_date) |  Optional | The ending date to search payments. | 
| sort | String |  Optional | A code representing the order of rendering objects. `ASC | DESC`| 

[Pagination format](../conventions/formattingConventions.md#pagination) can be applied on this request.

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payments | Array[[Payment Object](../objects/objects.md#payment_object)] | An array of [Payment Object](../objects/objects.md#payment_object) advising on all the payments made. |

**Example:**
```js
GET /payments/_all/?fromDate=2010-01-01&toDate=2015-06-30&sort=DESC
```
<hr />

#### <a id="get_payments"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payments/-{id}
```
Retrieve the details of a specific payment.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the payment you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you requested. |

**Example:**
```js
GET /payments/-AB4bsE
```
<hr />

#### <a id="delete_payments"></a> Cancel Payment ####

```
Method: DELETE
URL: /payment/-{id}
```

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the payment you want to delete. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you deleted. |


**Example:**
```js
DELETE /payments/-TE4Ba3
```
