# TRADE SERVICE #

FX trades are made between two wallet accounts. FX4BIZ will automatically debit the source wallet account and credit the destination wallet account at the date specified in the FX trade instructions. A FX trades also involves an amount, which includes both the numeric amount and the currency in order to define is this amount is to be buy or sell, for example: '100000.00+GBP'.

FX4BIZ provides a deliverable FX facility and deliverable FX liquidity via the FX4BIZ-REST API. You will become counterparty to FX4BIZ and can market and sell deliverable FX services to corporate and private clients as well as using such services on their behalf.

The FX4BIZ-rest API supports online trading for the following contracts: TOD (Same-day settled for those currencies than can be), TOM (next-day settled), SPOT (T+2) and forward contracts up to one year. 

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /rates`](#get_rates) | Retrieve Rates |
| [`POST /quotes`](#post_quotes) | Request Quote |
| [`POST /trades`](#post_trades) | Submit Trade |
| [`POST /trades/onquote/{id}`](#post_trades_on_quote) | Submit Trade with an existing quote |
| [`GET /trades/`](#cget_trades) | Retrieve Trades Book |
| [`GET /trades/{id}`](#get_trades) | Retrieve Trade Details |

## Details ##

#### <a id="get_rates"></a> Retrieve Rates ####

```
Method: 	GET
URL: 		/rates/{instruments}
```
The FX4BIZ-REST API provides a FX Data Feed. You can use the [Rates service](../objects/objects.md#rate_object) in order to ask for currency rates tables. Spreads showed in this service are minimal spreads, the tradable spread can be higher depending on the volatility of the market.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| instruments | String | Required | A string representing a list of crosses. Crosses must be separated with commas. <br />You can chain as many crosses as you want, as long as they're separated with commas. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| rates | Array[[Rate objects](../objects/objects.md#rate_object)] | An array containing crosses requested. |

**Example:**
```
/rates?instruments=EURGBP,EURUSD
```

<hr />

#### <a id="post_quotes"></a> Retrieve Quote ####

```
Method: POST
URL: /quotes/
```
The Retrieve Quote service is a read-only service permitting to ask for the real-time rate before to execute a trade. 
*Caution:* It is not possible to trade with the Retrieve Quote service, you have to use the [Trade Service](#submit-trade) in order to placing new trades. 

*Parameters:*

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currencyPair | String | Required | The currency pair representing the quote requessted. `EURUSD` |
| side | String | Required | The side repressenting the quote. `S` to sell and `B` to buy. |
| amount | [Amount Object](../objects/objects.md#amount_object) | Required | Amount to trade. |
| deliveryDate | String | Required | Initial delivery date of the quote. `YYYY-MM-DD` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| quote | [Quote object](../objects/objects.md#quote_object) | A [Quote object](../objects/objects.md#quote_object) representing the quote requested. |

**Example:**
```
/quotes/
```
<hr />

#### <a id="post_trades"></a> Execute Trade ####

```
Method: POST
URL: /trades/
```
This services permits to execute trade.

*Parameters:*

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| settlementWalletId | String | Required | The ID of the source account. `xxx` |
| deliveryWalletId | String | Required | The ID of the destination account. `xxx` |
| currencyPair | String | Required | The currency pair representing the quote requessted. `EURUSD` |
| side | String | Required | The side repressenting the quote. `S` to sell and `B` to buy. |
| amount | [Amount Object](../objects/objects.md#amount_object) | Required | Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| deliveryDate | String | Required | Initial execution date of you payment. `YYYY-MM-DD` |


**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| trade | [Trade object](../objects/objects.md#trade_object) | A [Trade object](../objects/objects.md#trade_object) representing the trade submitted. |

**Example:**
```
/trades/
```
<hr />

#### <a id="post_trades_on_quote"></a> Execute trade with an existing quote ####

```
Method: POST
URL: /trades/onquote/{id}
```
This services permits to execute trade.

*Parameters:*

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | String | Required | The ID of the quote you get from [Retrieve quote](#post_quote). |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| quote | [Trade object](../objects/objects.md#trade_object) | A [Trade object](../objects/objects.md#trade_object) representing the trade submitted. |

**Example:**
```
/trades/onquote/{id}
```
<hr />

#### <a id="cget_trades"></a> Retrieve Trades Book ####

```
Method: GET
URL: /trades/
```
Retrieve the list of trades executed. We sort trades by validation date.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| fromDate | String | Optionnal | A date representing the starting date to search payments. `YYYY-MM-DD` |
| toDate | String | Optionnal | A date representing the ending date to search payments. `YYYY-MM-DD` | 
| sort | String | Optionnal | A String representing the order of rendering objects. `YYYY-MM-DD` | 

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| trades | Array[[Trade Object](../objects/objects.md#trade_object)] | An array of [Trade Object](../objects/objects.md#trade_object) describing trades you requested. |

**Example:**
```
/trades/
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
| id | String | Required | The ID of the trade you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| trade | [Trade Object](../objects/objects.md#trade_object) | A [Trade Object](../objects/objects.md#trade_object) describing trades you requested. |

**Example:**
```
/trades/89456
```
