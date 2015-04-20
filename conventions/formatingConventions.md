# Formatting Conventions #


The `FX4BIZ-rest` API conforms to the following general behavior for [RESTful API](http://en.wikipedia.org/wiki/Representational_state_transfer):

* You make HTTP (or HTTPS) requests to the API endpoint, indicating the desired resources within the URL itself. (The public server, for the sake of security, only accepts HTTPS requests.)
* The HTTP method identifies what you are trying to do.  Generally, HTTP `GET` requests are used to retrieve information, while HTTP `POST` requests are used to make changes or submit information.
* If more complicated information needs to be sent, it will be included as JSON-formatted data within the body of the HTTP POST request.
  * This means that you must set `Content-Type: application/json` in the headers when sending POST requests with a body.
* Upon successful completion, the server returns an [HTTP status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) of 200 OK, and a `Content-Type` value of `application/json`.  The body of the response will be a JSON-formatted object containing the information returned by the endpoint.

As an additional convention, all responses from FX4Biz-REST contain a `"success"` field with a boolean value indicating whether or not the success


* [Cut-Off Times](#cut_off_times)
* [Errors](#errors_conventions)
* [Pagination](#pagination)
* [Quoted numbers](#quoted_numbers)
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

FX4BIZ uses conventional HTTP response codes to indicate success or failure of an PAI request. The body of the response contains more detailed information on the cause of the problem.

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
| ErrorCode | Integer | The code referring the error. |
| ErrorType | String | A short description identifying a general category for the error that occurred. |
| Link | String | An hyperlink to access the page that describes more accurately the error. |

*Example error:*

```js
"Error": {
	"ErrorCode": 9,
	"ErrorType": "Entry already exists",
	"Link": "http://fx4bix.com/RestError/OTFlNGY5ZWVlOTNjODZkZDVhZjg3YTlkNzBmMzgxZmI=/9"
}
```

Our API libraries can raise exceptions for many reasons, such as failed trade, invalid parameters, authentications errors, and network unavailability. We recommend always trying to gracefully handle exceptions from our API.

## <a id="pagination"></a> Pagination ##

All top-level FX4BIZ API resources have support for bulk fetches - "list" API methods. For instance you can [list accounts`](#get-account-list), [list transfers`](#get-transfers-list), etc... These list API methods share a common structure.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| page | String | index of the page (start to 1) Default value: `1` |
| per_page | String | number of items returned. Default value: `10` Max: `100` |

## <a id="quoted_numbers"></a> Quoted Numbers ##

In any case where a large number should be specified, FX4Biz-REST uses a string instead of the native JSON number type. This avoids problems with JSON libraries which might automatically convert numbers into native types with differing range and precision.

You should parse these numbers into a numeric data type with adequate precision. If it is not clear how much precision you need, we recommend using an arbitrary-precision data type.

## <a id="versioning"></a> Versioning ##

When we make backwards-incompatible changes to the API, we realease new dated versions.
