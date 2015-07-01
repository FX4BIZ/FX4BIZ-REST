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
| [`PUT /payments/{id}/confirm`](#put_payments_confirm) | Confirm Payment |
| [`GET /payments/`](#cget_payments) | Retrieve Payments History |
| [`GET /payments/{id}`](#get_payments) | Retrieve Payment Details | 
| [`DELETE /payments/{id}`](#delete_payments) | Cancel Payment |

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
| beneficiaryAccountId | String | Required | The ID of the beneficiary account. |
| amount | [Amount Object](../objects/objects.md#amount_object) | Required | Amount to be sent. *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| desiredExecutionDate | String | Required | Initial execution date of you payment. `YYYY-MM-DD` |
| feeCurrency  | String | Required | A string representing the currency related to the charges applied on your payment. |
| feePaymentOption | String | Required | A string representing the charges option to be applied to this payment. |
| tag | String | Required | A custom reference that you want to be related to this payment in the system. This tag is not communicated to the beneficiary. |
| priorityPaymentOption | String | Required | A string representing wether this payment as a normal priority, or it as to be done quick. `normal | speed` |
| sourceWalletId | String | Required | Specify on which wallet the payment will be processed. |
| communication | String | Optionnal | A Free format string representing the communication for the beneficiary. (76 chars max.) |

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

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | String | Required | The ID of the payment you want to confirm. |

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

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| fromDate | String |  Optionnal | A date representing the starting date to search payments. |
| toDate | String |  Optionnal | A date representing the ending date to search payments. | 
| sort | String |  Optionnal | A String representing the order of rendering objects. | 

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payments | Array[[Payment Object](../objects/objects.md#payment_object)] | An array of [Payment Object](../objects/objects.md#payment_object) describing payments you made. |

**Example:**
```
/payments/?fromDate=2010-01-01&toDate=2015-06-30&sort=DESC
```
<hr />

#### <a id="get_payments"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payments/{id}
```
Retrieve the details of a specific payment.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | String | Required | The ID of the payment you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| payment | [Payment Object](../objects/objects.md#payment_object) | A [Payment Object](../objects/objects.md#payment_object) describing the payment you requested. |

**Example:**
```
/payments/89456
```
<hr />

#### <a id="delete_payments"></a> Cancel Payment ####

```
Method: DELETE
URL: /payment/{id}
```

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | String | Required | The ID of the payment you want to delete. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| process | Object | An object containing the result of the process. |
| Object.result | Boolean | A boolean value describing if the process succeeded or not. |
