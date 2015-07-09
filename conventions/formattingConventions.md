# Formatting Conventions #  

The FX4BIZ Rest API conforms to the following general behavior for [RESTful API](http://en.wikipedia.org/wiki/Representational_state_transfer):

* You make [HTTPS](http://en.wikipedia.org/wiki/HTTPS) requests to the API endpoint, indicating the desired resources within the URL itself. (The public server, for the sake of security, only accepts [HTTPS](http://en.wikipedia.org/wiki/HTTPS) requests.)
* The [HTTP method](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) identifies what you are trying to do.  Generally, HTTP `GET` requests are used to retrieve information, while HTTP `POST` requests are used to make changes or submit information.
* If more complicated information needs to be sent, it will be included as [JSON-formatted](http://en.wikipedia.org/wiki/JSON) data within the body of the HTTP POST request.
  * This means that you must set `Content-Type: application/json` in the headers when sending POST requests with a body.
* Upon successful completion, the server returns an [HTTP status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) of 200 OK, and a `Content-Type` value of `application/json`.  The body of the response will be a JSON-formatted object containing the information returned by the endpoint.

As an additional convention, all responses from FX4Biz-REST contain a `"success"` field with a boolean value indicating whether or not the success


* [Cut-Off Times](#cut_off_times)
* [Errors](#errors_conventions)
* [Pagination](#pagination)
* [normalized Types](#anonymous_object)
	* [ID type](#type_id)
	* [Date type](#type_date)
	* [DateTime type](#type_datetime)
	* [Currency type](#type_currency)
	* [CurrencyPair type](#type_currencypair)
	* [QuotedDecimal type](#type_quoteddecimal)
* [anonymous Objects](#anonymous_object)
* [Versioning](#versioning)


## <a id="cut_off_times"></a> Cut-Off Times ##

| Same day value Currencies | Cut Off Time |
|------|------|
| CHF | 8:30 |
| GBP | 10:30 |
| EUR | 16:00 |
| USD | 16:30 |

| Next day value Currencies (Times stated are for the day prior to the Value Date) | Cut Off Time |
|-------|------|
| AOA, ARS, BIF, BRL, CDF, CLP, COP, CRC, DJF, DOP, GHS, HNL, KES, MAD, NPR, PEN, PHP, RUB, TND, TRY, TZS, UGX, XOF/XAF | 10:00 |
| AED, AUD, CAD, CZK, DKK, HKD, HUF, JPY, NOK, NZD, PLN, SEK, SGD, ZAR | 10:30 |

## <a id="errors_conventions"></a> Errors ##

FX4BIZ uses conventional HTTP response codes to indicate success or failure of an API request. The body of the response contains more detailed information on the cause of the problem.

In general, the HTTP status code is indicative of where the problem occurred:

* Codes in the 200-299 range indicate success. 
    * Unless otherwise specified, methods are expected to return `200 OK` on success.
* Codes in the 400-499 range indicate that the request was invalid or incorrect somehow (e.g. a required parameter was missing, a trade failed, etc.). For example:
    * `400 Bad Request` occurs if the JSON body is malformed. This includes syntax errors as well as when invalid or mutually-exclusive options are selected.
    * `403  Forbidden` occurs if there is a problem with your authentication on the server.
    * `404 Not Found` occurs if the path specified does not exist, or does not support that method (for example, trying to POST to a URL that only serves GET requests)
* Codes in the 500-599 range indicate that the server experienced a problem. This could be due to a network outage or a bug in the software somewhere. For example:
    * `500 Internal Server Error` occurs when the server does not catch an error. This is always a bug. If you can reproduce the error, file it at [our bug tracker in your FX4Biz platform](https://fx4bizplatform.com/login).
    * `502 Bad Gateway` occurs if FX4Biz-REST could not contact its `FX4Biz` server at all.
    * `504 Gateway Timeout` occurs if the `FX4Biz` server took too long to respond to the FX4Biz-REST server.

When possible, the server provides a JSON response body with more information about the error. These responses contain the following fields:

| Field | Type | Description |
|-------|------|-------------|
| errorCode | Integer | The code referring the error. |
| errorType | String | A short description identifying a general category for the error that occurred. |
| link | String | An hyperlink to access the page that describes more accurately the error. |

**Example:**

```js
"error": {
	"errorCode": 9,
	"errorType": "Entry already exists",
	"link": "http://fx4bix.com/RestError/OTFlNGY5ZWVlOTNjODZkZDVhZjg3YTlkNzBmMzgxZmI=/9"
}
```

Our API libraries can raise exceptions for many reasons, such as failed trade, invalid parameters, authentications errors, and network unavailability. We recommend always trying to gracefully handle exceptions from our API.

## <a id="pagination"></a> Pagination ##

All top-level FX4BIZ API resources have support for bulk fetches - "list" API methods. For instance you can [list accounts`](#get-account-list), [list transfers`](#get-transfers-list), etc... These list API methods share a common structure.

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| page | String | index of the page (start to 1) Default value: `1` |
| per_page | String | number of items returned. Default value: `50` |


## <a id="normalized_types"></a> Normalized Types ##

The FX4BIZ API uses a set of types that describe a particular parameters and values.<br />
It allows developpers to know the format and the content to send for a particular field.

### <a id="type_id"></a> ID Type ###

The ID type describe an identifier for a certain object.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| ID | String | `^[A-Za-z0-9]{*}$` | A String representing the id of an object. This string contains alpha-numeric characters, including the capital ones. | `ND9dUV` |

### <a id="type_date"></a> Date Type ###

The Date type represents a date with its year, month and day.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Date | String | `^[0-9]{4}\-[0-9]{2}\-[0-9]{2}$` - `YYYY-MM-DD` | A String representing a date by its year, month and day in month. | `2015-07-14` |

### <a id="type_datetime"></a> DateTime Type ###

The Date type represents a date with its year, month and day, and also with hours, minutes and seconds.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| DateTime | String | `^((19[0,99]|2[0-9]{3})\-(0[1-9]|1[012])\-([012][0-9]|3[01])\ ([01][0-9]|2[0-3])\:([0-5][0-9])\:([0-5][0-9]))$` - `YYYY-MM-DD HH:mm:SS` | A String representing a date by its year, month, day in month, hour, minute and second. | `2015-07-14 10:55:37` |

### <a id="type_currency"></a> Currency Type ###

The Currency type represents a currency trigram.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| Currency | String | `^[A-Z]{3}$` | A String representing the Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) of a currency. This String only contains capitalized letters. | `USD` |

### <a id="type_currencypair"></a> CurrencyPair Type ###

The CurrencyPair type represents a currency pair, used for currency change operation.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| CurrencyPair | String | `^[A-Z]{6}$` | A String representing two concatenated Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) of a currency. This String only contains capitalized letters. | `EURGBP` |

### <a id="type_quoteddecimal"></a> QuotedDecimal Type ###

The QuotedDecimal type describe an formatted floating number.

| Type | Real type | format | description | example |
|------|-----------|--------|-------------|---------|
| QuotedDecimal | String | `^((\-){?})[0-9]{12}((\.[0-9]{1-7})?)$` | A String representing a formatted floating number. | `"1.11723"` |

## <a id="anonymous_object"></a> Anonymous objects ##

Some request of the FX4BIZ REST API send, as a response, anomymous objects, or arrays of anonymous objects.  
Here is an example to understand the notation of these objects :

| Field | Type | Description |
|-------|------|-------------|
| foo | Array[Object] | `foo` is an Array containing anonymous objects |
| Object.bar | String | `bar` is a String typed property of the anonymous object `Object`. <br /> In a [DOM](http://fr.wikipedia.org/wiki/Document_Object_Model) representation, you can access `bar` via `foo[].bar`

Here is the JSON corresponding the description above : 

```js
{
	...
	"foo" [
		{
			"bar":"something"
		},
		{
			"bar":"somethingElse"
		}
	]
	...
}
```

## <a id="versioning"></a> Versioning ##

When we make backwards-incompatible changes to the API, we realease new dated versions.
