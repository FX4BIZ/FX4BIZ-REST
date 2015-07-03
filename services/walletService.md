# WALLET ACCOUNT SERVICE #

In the FX4BIZ API, what we call a "wallet" account can be either a physical iban account, or a virtual iban account with FX4BIZ. 

A virtual account is an administrative "subaccount" of one physical iban account (the "masteraccount"). Cash can be earmarked as belonging to a virtual account so that you can easily allocate funds per business unit, per client, or incoming/outgoing transactions).

## Route ##

| Route | Description |
|-------|-------------|
| [`POST /wallets/`](#post_wallets) | Submit new wallet |
| [`POST /wallets/withholder`](#post_wallets_with_holder) | Submit new wallet with an holder |
| [`GET /wallets/`](#cget_wallets) | Retrieve wallets list |
| [`GET /wallets/{id}`](#get_wallets) | Retrieve wallet details |
| [`GET /wallets/{id}/balance/{date}`](#get_wallets_balance) | Retrieve wallet balance for a given date |

## Details ##


#### <a id="post_wallets"></a> Submit a new wallet ####

```
Method: POST 
URL: /wallets/
```

This request allows you to submit a new wallet for a given currency.  
**Caution :** This service is only available for FX4BIZ users that have suscribed to the wallet option program.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | Required | A three-digit code specifying the wallet currency. `USD` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet you just created. |

**Example:**
```js
POST /wallets/
{
	"currency":"EUR"
}
```

<hr />

#### <a id="post_wallets_with_holder"></a> Submit a new wallet with an holder ####

```
Method: POST 
URL: /wallets/
```

This request allows you to submit a new wallet for a given currency and a given holder.  
**Caution :** This service is only available for FX4BIZ users that have suscribed to the wallet option program.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | Required | A three-digit code specifying the wallet currency. `USD` |
| holder | [Holder Object](../objects/objects.md#beneficiary_object) | Required | The recipient details, owner of the wallet. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet you just created. |

**Example:**
```js
POST /wallets/withholder/
{
    "currency": "AUD",
    "holder": {
        "name": "Australian client #5763",
        "type": "Individual",
        "address": {
            "street": "12 1st Street",
            "postCode": "12500",
            "city": "Sidney",
            "country": "AU"
        }
    }
}
```

<hr />

#### <a id="cget_wallets"></a> Retrieve wallets list ####

```
Method: GET 
URL: /wallets/
```
With the Retrieve wallet list service, you can list obtain the list of all wallet account hold with FX4BIZ. The object return in the Array is a simplified version of the [Wallet Object](../objects/objects.md#wallet_object) providing you the main information on the wallet without any additional request. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| sort | String | Optionnal | A code representing the order of rendering wallets by their creation date. `ASC | DESC` | 


This request is applicable for the [pagination format](../conventions/formatingConventions.md#pagination).

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallets | Array[Object] | An Array of objects representing wallets. |
| Object.id | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the wallet. |
| Object.tag | String | The custom wording of the wallet. |
| Object.currency | [Currency](../conventions/formatingConventions.md#type_currency) | The three-digit code identifying the currency of the wallet. |
| Object.bookingAmount | [Amount Object](../objects/objects.md#amount_object) | The total amount booked on the account. |
| Object.valueAmount | [Amount Object](../objects/objects.md#amount_object) | The total amount available in the wallet. |
| Object.dateLastFinancialMovement | [Date](../conventions/formatingConventions.md#type_date) | The date of the last financial move in this wallet. |

**Example:**
```js
GET /wallets/?per_page=20&page=2&sort=DESC
```

<hr />

#### <a id="get_wallets"></a> Retrieve wallet details ####

```
Method: GET 
URL: /wallets/{id}
```
This request allows you to see the details related to a specific wallet. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | Required | The code identifying the external bank account you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet requested. |

**Example:**
```JS
GET /wallets/NT4d3a
```

<hr />

#### <a id="get_wallets_balance"></a> Retrieve wallet balance for a given date ####

```
Method: GET 
URL: /wallets/{id}/balance/{date}
```
This request allows you to see the details of a wallet balance at a given date. 

**Parameters:**  

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | Required | The code identifying the external bank account you want. |
| date | [Date](../conventions/formatingConventions.md#type_date) | Required | The date used to retrieve the balance of the wallet. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the wallet. |
| balance | [Balance Object](../objects/objects.md#balance_object) | An object representing the balance of the account. |

**Example:**
```js
GET /wallets/NT4eAD/balance/2015-04-30
```
