# FINANCIAL MOVEMENT SERVICE #

FX trades are made between two wallet accounts. FX4BIZ will automatically debit the source wallet account and credit the destination wallet account at the date specified in the FX trade instructions. If no date is specified, we will execute the operation at the closest tradable date available. A FX trades also involves an amount, which includes both the numeric amount and the currency in order to define is this amount is to be buy or sell, for example: '100000.00+GBP'.

FX4BIZ provides a deliverable FX facility and deliverable FX liquidity via the FX4BIZ-REST API. You will become counterparty to FX4BIZ and can market and sell deliverable FX services to corporate and private clients as well as using such services on their behalf.

The FX4BIZ-rest API supports online trading for the following contracts: TOD (Same-day settled for those currencies than can be), TOM (next-day settled), SPOT (T+2) and forward contracts up to one year. 

As an example, a response for `GET /trade/{:id}` object looks like this:
```js
{
    "trade": {
        "id": "xxx",
        "status": "Awaiting confirmation",
        "created_date": "2014-01-12T00:00:00+00:00",
        "created_by": "Api",
        "initial_operation_date"= "2014-01-12T00:00:00+00:00",
        "type": "Standard",
        "amount": {
            "value": "125000.00",
            "currency": "USD"
        }
    }
}
```
## Route ##

| Route | Description |
|-------|-------------|
| [`GET /rates`](#get-rates) | Retrieve Rates |
| [`POST /quote`](#get-quote) | Request Quote |
| [`POST /trade`](#get-trade) | Submit Trade |
| [`DELETE /trade/{trade_id}`](#cancel-trade) | Cancel Trade |
| [`GET /trades`](#get-trade-book) | Retrieve Trades Book |
| [`GET /trade/{trade_id}`](#get-trade-details) | Retrieve Trade Details |

## Details ##

#### <a id="get-rates"></a> Retrieve Rates ####

```
Method: 	GET
URL: 		/rates/{instruments}
```
The FX4BIZ-REST API provides a FX Data Feed. You can use the [Rates service](../objects/objects.md#rate_object) in order to ask for currency rates tables. Spreads showed in this service are minimal spreads, the tradable spread can be higher depending on the volatility of the market.

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| instruments | String | **Required.** A string representing a list of crosses. Crosses must be separated with commas. <br />You can chain as many crosses as you want, as long as they're separated with commas. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| rates | Array[[Rate objects](../objects/objects.md#rate_object)] | An array containing crosses requested. |

**Example:**
```
/rates?instruments=EURGBP,EURUSD
```

<hr />

#### <a id="get-quote"></a> Retrieve Quote ####

```
Method: GET
URL: /quote
```
The Retrieve Quote service is a read-only service permitting to ask for the real-time rate before to execute a trade. 
*Caution:* It is not possible to trade with the Retrieve Quote service, you have to use the [Trade Service](#submit-trade) in order to placing new trades. 

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| currency_source | String | **Required.** Id of the destination account. `xxx` |
| currency_target | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](../objects/objects.md#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response to this query, you will receive the [Quote](../objects/objects.md#quote_object) required.

<hr />

#### <a id="submit-trade"></a> Execute Trade ####

```
Method: POST
URL: /trade
```
This services permits to execute trade.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| currency_source | String | **Required.** Id of the destination account. `xxx` |
| currency_target | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](../objects/objects.md#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response, you will receive the details of the [Trade](../objects/objects.md#trade_object) executed.

<hr />

#### <a id="get-trade"></a> Retrieve Trade ####

```
Method: GET
URL: /trade/{trade_id}
```
Retrieve the trade details.

As a response, you will receive the details of the [Trade](../objects/objects.md#trade_object).

<hr />

#### <a id="get-trades"></a> Retrieve Trades Book ####

```
Method: GET
URL: /trades
```
Retrieve the list of trades executed. We sort trades by validation date.

As a response, you will receive an array containing the `trade_id`, the `validation_date` and the `amount` for all trades validated.
