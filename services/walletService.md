# WALLET ACCOUNT SERVICE #  

In the FX4BIZ API, what we call a "wallet" account can be either a physical iban account, or a virtual iban account with FX4BIZ. 

A virtual account is an administrative "subaccount" of one physical iban account (the "masteraccount"). Cash can be earmarked as belonging to a virtual account so that you can easily allocate funds per business unit, per client, or incoming/outgoing transactions).

## Route ##

| Route | Description |
|-------|-------------|
| [`POST /wallets/`](#post_wallets) | Submit new wallet |
| [`POST /wallets/withholder`](#post_wallets_with_holder) | Submit new wallet with an holder |
| [`GET /wallets/`](#cget_wallets) | Retrieve wallets list |
| [`GET /wallets/-{id}`](#get_wallets) | Retrieve wallet details |
| [`GET /wallets/-{id}/balance/{date}`](#get_wallets_balance) | Retrieve wallet balance for a given date |

## Details ##


#### <a id="post_wallets"></a> Submit a new wallet ####

```
Method: POST 
URL: /wallets/
```

This request allows you to submit a new wallet.  
**Caution :** The holder object in the parameters will only be considered if you suscribed to the `Multi account per currency with holder` wallet option.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | Required | A three-digit code specifying the wallet currency. `USD` |
| tag | string | Optionnal | Custom data `USD wallet #3` |
| holder | [Holder Object](../objects/objects.md#beneficiary_object) | Optionnal | The holder of the wallet. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet you just created. |

**Example:**
```js
POST /wallets/
{
    "currency": "USD",
    "tag": "USD Account #3",
    "holder": {
        "name": "US client #5763",
        "type": "Individual",
        "address": {
            "street": "12 1st Street",
            "postCode": "12500",
            "city": "New York",
            "state": "NY",
            "country": "US"
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
| sort | String | Optional | A code representing the order of rendering wallets by their creation date. `ASC | DESC` | 

[Pagination format](../conventions/formattingConventions.md#pagination) can be applied on this request.

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallets | Array[Object] | An Array of objects representing wallets. |
| Object.id | [ID](../conventions/formattingConventions.md#type_id) | The code identifying the wallet. |
| Object.tag | String | The custom wording of the wallet. |
| Object.currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code identifying the currency of the wallet. |
| Object.bookingAmount | [Amount Object](../objects/objects.md#amount_object) | The total amount booked on the account. |
| Object.valueAmount | [Amount Object](../objects/objects.md#amount_object) | The total amount available in the wallet. |
| Object.dateLastFinancialMovement | [Date](../conventions/formattingConventions.md#type_date) | The date of the last financial move in this wallet. |

**Example:**
```js
GET /wallets/?per_page=20&page=2&sort=DESC
```

<hr />

#### <a id="get_wallets"></a> Retrieve wallet details ####

```
Method: GET 
URL: /wallets/-{id}
```
This request allows you to see the details related to a specific wallet. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the external bank account you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet requested. |

**Example:**
```JS
GET /wallets/-NT4d3a
```

<hr />

#### <a id="get_wallets_balance"></a> Retrieve wallet balance for a given date ####

```
Method: GET 
URL: /wallets/-{id}/balance/{date}
```
This request allows you to see the details of a wallet balance at a given date. 

**Parameters:**  

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The code identifying the external bank account you want. |
| date | [Date](../conventions/formattingConventions.md#type_date) | Required | The date used to retrieve the balance of the wallet. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The code identifying the wallet. |
| balance | [Balance Object](../objects/objects.md#balance_object) | An object representing the balance of the account. |

**Example:**
```js
GET /wallets/-NT4eAD/balance/2015-04-30
```

<hr />

#### <a id="get_wallets_iban"></a> Retreive wallet IBAN ####

```
Method: GET 
URL: /wallets/generateIBAN/{branch}/{accountNumber}
```

This request allows you to get a virtual IBAN number for an IBAN branch and an account number.  
The given account number is a custom number set on your own side, it has no link with a wallet.  
**Caution :** This request is only available for clients which suscribed to the `virtual IBAN` option.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| branch | string | Required | A string representing the branch number of your virtual IBAN. `548` |
| accountNumber | string | Required | A string representing a custom account number. `1075` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| virtualIBAN | [Virtual IBAN Object](../objects/objects.md#virtualIBAN_object) | An object representing the wallet you just created. |

**Example:**
```js
GET /wallets/generateIBAN/548/1075
```
