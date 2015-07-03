# TRADE SERVICE #

FX4BIZ provides a deliverable FX facility and deliverable FX liquidity via the FX4BIZ-REST API. You will become counterparty to FX4BIZ and can market and sell deliverable FX services to corporate and private clients as well as using such services on their behalf.  

The FX4BIZ-rest API supports online trading for the following contracts: TOD (Same-day settled for those currencies than can be), TOM (next-day settled), SPOT (T+2) and forward contracts up to one year.<br />

FX trades are always made between two wallet accounts. FX4BIZ will automatically debit the source wallet account and credit the delivery wallet account at the date specified in the FX trade instructions. If the delivery date has been scheduled, the delivery is automatically processed in the morning before 00:30 am Paris time. If the delivery date is today (TOD), the funds is available on your wallet account by the next 20mn.

A FX trades also involves an amount, which includes both the numeric amount and the currency in order to define is this amount is to be buy or sell, for example: '100000.00+GBP'.

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /rates`](#get_rates) | Retrieve Rates |
| [`POST /quotes`](#post_quotes) | Request Quote |
| [`POST /trades`](#post_trades) | Submit Trade |
| [`POST /trades/onquote/`](#post_trades_on_quote) | Submit Trade with an existing quote |
| [`GET /trades/`](#cget_trades) | Retrieve Trades Book |
| [`GET /trades/{id}`](#get_trades) | Retrieve Trade Details |

## Details ##

#### <a id="get_rates"></a> Retrieve Rates ####

```
Method: 	GET
URL: 		/rates/{instruments}
```
The FX4BIZ-REST API provides a FX Data Feed. The Rates service is a read-only service, it provides "gross" rates: this is mid-market rates but usually with 5mn delays.  
You can use these rates for you internal needs (charts, consolidation etc..).  

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| instruments | String | Required | A string representing a list of [CurrencyPair](../conventions/formatingConventions.md#type_currencypair). Crosses must be separated with commas. <br />You can chain as many crosses as you want, as long as they're separated with commas. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| rates | Array[[Rate objects](../objects/objects.md#rate_object)] | An array containing crosses requested. |

**Example:**
```js
GET /rates/EURGBP,EURUSD
```

<hr />

#### <a id="post_quotes"></a> Retrieve Quote ####

```
Method: POST
URL: /quotes/
```
The Retrieve Quote service is a read-only service permitting to ask for a real-time rate. With this service you will have the same information than in the Trade service but you cannot trade with the Quote service.  
**Caution:** It is not possible to trade with the Retrieve Quote service, you have to use the [Trade Service](#submit-trade) in order to placing new trades. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | Required | The currency pair representing the quote requessted. `EURUSD` |
| side | String | Required | The side repressenting the quote. `S` to sell and `B` to buy. |
| amount | [Amount Object](../objects/objects.md#amount_object) | Required | Amount to trade. |
| deliveryDate | [Date](../conventions/formatingConventions.md#type_date) | Required | Initial delivery date of the quote. `YYYY-MM-DD` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| quote | [Quote object](../objects/objects.md#quote_object) | A [Quote object](../objects/objects.md#quote_object) representing the quote requested. |

**Example:**
```js
POST /quotes/
{
    "currencyPair": "EURUSD",
    "side": "S",
    "amount": {
        "value": "15000",
        "currency": "EUR"
    },
    "deliveryDate": "2015-07-10"
}
```
<hr />

#### <a id="post_trades"></a> Execute Trade ####

```
Method: POST
URL: /trades/
```
This services permits to execute trade.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| sourceWalletId | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) of the account to be debited of the targeted amount. |
| deliveryWalletId | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) of the destination account. `xxx` |
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | Required | The currency pair representing the quote requessted. `EURUSD` |
| side | String | Required | Action related to the nominal amount. To be bought or sold on the market, `S` to sell and `B` to buy. |
| amount | [Amount Object](../objects/objects.md#amount_object) | Required | Nominal amount associated to the trade. It can be the source amount or the targeted amount depending on the side of your trade. Basically, this is the amount that you want to be defined.<br />  **Caution:** The currency of the amount sent must be equal to the currency of the beneficiary account. |
| deliveryDate | [Date](../conventions/formatingConventions.md#type_date) | Required | Execution [Date](../conventions/formatingConventions.md#type_date) of you trade. |


**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| trade | [Trade object](../objects/objects.md#trade_object) | A [Trade object](../objects/objects.md#trade_object) representing the trade submitted. |

**Example:**
```js
POST /trades/
{
    "settlementWalletId": "ND4kTO",
    "deliveryWalletId": "NT5aBS",
    "currencyPair": "EURUSD",
    "side": "B",
    "amount": {
        "value": "10000",
        "currency": "USD"
    },
    "deliveryDate": "2015-08-24"
}
```
<hr />

#### <a id="post_trades_on_quote"></a> Execute trade on an existing quote ####

```
Method: POST
URL: /trades/onquote/
```
This services permits to execute trade.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| quoteId | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) of the quote you get from [Retrieve quote](#post_quote). |
| sourceWalletId | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) ID of the account to be debited of the amount source. |
| deliveryWalletId | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) of the account to be credited of the targeted amount. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| quote | [Trade object](../objects/objects.md#trade_object) | A [Trade object](../objects/objects.md#trade_object) representing the trade submitted. |

**Example:**
```js
POST /trades/onquote/
{
    "quoteId": "NT4kND",
    "settlementWalletId": "NT6sbE",
    "deliveryWalletId": "NT8aBS"
}
```
<hr />

#### <a id="cget_trades"></a> Retrieve Trades Book ####

```
Method: GET
URL: /trades/{status}/
```
Retrieve the list of trades executed.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| status | String |  Required | A String representing the status of the payments you want to get. `all | Planified | Rejected | Finalized | Canceled | Refused | blocked | WaitingConfirmation` | 
| fromDate | [Date](../conventions/formatingConventions.md#type_date) | Optionnal | A [Date](../conventions/formatingConventions.md#type_date) representing the starting date to search payments. `YYYY-MM-DD` |
| toDate | [Date](../conventions/formatingConventions.md#type_date) | Optionnal | A [Date](../conventions/formatingConventions.md#type_date) representing the ending date to search payments. `YYYY-MM-DD` | 
| sort | String | Optionnal | A String representing the order of rendering trades by its creation date. `ASC | DESC` | 

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| trades | Array[[Trade Object](../objects/objects.md#trade_object)] | An array of [Trade Object](../objects/objects.md#trade_object) describing trades you requested. |

**Example:**
```js
GET /trades/all/?fromDate=2015-01-01&toDate=2015-06-30&sort=DESC
```
<hr />

#### <a id="get_trades"></a> Retrieve Trade ####

```
Method: GET
URL: /trades/{id}
```
Retrieve the trade details.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) of the trade you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| trade | [Trade Object](../objects/objects.md#trade_object) | A [Trade Object](../objects/objects.md#trade_object) describing trades you requested. |

**Example:**
```js
GET /trades/TD4ebA
```

