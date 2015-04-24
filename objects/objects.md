# API Objects #

* [External Bank Account Object](#account_object)
* [Wallet Object](#wallet_object)
* [Address Object](#address_object)
* [Balance Object](#balance_object)
* [Holder Bank Object](#beneficiary_bank_object)
* [Holder Object](#beneficiary_object)
* [Correspondent Bank Object](#correspondent_bank_object)
* [Payment Object](#payment_object)
* [Rate Object](#rate_object)
* [Trade Object](#trade_object)
* [Financial Movement Object](#financial_movement_object)
* [Quote Object](#trade_object)
* [Log Object](#log_object)

## Details ##

#### <a id="account_object"></a> External Bank Account Object ####

When an account is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| id |  String | The id of the account. `xxx` |
| createdDate |  Date Time | The creation date of the object: `2014-01-12T00:00:00+00:00` |
| createdBy |  String | The creation date of the object: `api` |
| currency | String | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `USD` |
| tag |  String | Custom data. `reference` |
| status |  String | Status of the account `active` |
| type |  String | type of account `wallet` |
| number | String | Iban or account number. `xxx384` |
| correspondentBank | [Correspondent Bank Object](#correspondent_bank_object) | **Required for local format.** The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [Holder Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| holder | [Holder Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |

*Example Account Object:*

```js
{
    "account": {
        "id": "xxx"
        "status": "active",
        "type": "wallet",
        "created_date": "2014-01-12T00:00:00+00:00",
        "created_by": "api",
        "tag": "My wallet account EUR",
        "number": "xxx4548",
        "currency": "EUR",
        "correspondant_bank":{correspondent_bank}
        "holderBank":{beneficiary_bank}
        "holder":{beneficiary}
    }
}
```

<hr />

#### <a id="wallet_object"></a>Wallet Object ####

When an wallet is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| id |  String | The id of the account. `xxx` |
| createdDate |  Date Time | The creation date of the object: `2014-01-12T00:00:00+00:00` |
| createdBy |  String | The creation date of the object: `api` |
| currency | String | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `USD` |
| tag |  String | Custom data. `reference` |
| status |  String | Status of the account `active` |
| type |  String | type of account `wallet` |
| number | String | Iban or account number. `xxx384` |
| correspondentBank | [Correspondent Bank Object](#correspondent_bank_object) | **Required for local format.** The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [Holder Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| holder | [Holder Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |

*Example Account Object:*

```js
{
    "account": {
        "id": "xxx"
        "status": "active",
        "type": "wallet",
        "created_date": "2014-01-12T00:00:00+00:00",
        "created_by": "api",
        "tag": "My wallet account EUR",
        "number": "xxx4548",
        "currency": "EUR",
        "correspondant_bank":{correspondent_bank}
        "holderBank":{beneficiary_bank}
        "holder":{beneficiary}
    }
}
```

<hr />

#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with four fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| street | String | `350 Avenue Louise` |
| postCode | String | `1050` |
| city | String | `Bruxelles` |
| country | String | `BE` |

*Example Address Object:*

```js
{
    "address": {
      "street": "4 NEW YORK PLAZA, FLOOR 15",
      "post_code": "10004",
      "city": "NEW YORK",
      "country": "US"
    }
}
```

<hr />

#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with two fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| value  | String (Quoted decimal) | The quantity of the currency. `25000000.00` |
| currency | String | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the amount currency. `USD` |

*Example Amount Object:*

```js
{
    "amount": {
      "value": "10000.00",
      "currency": "GBP"
    }
}
```

<hr />

#### <a id="balance_object"></a> Balance Object ####

When the balance is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| closing_date | Date | The closing date of the balance details given. `2014-01-12` |
| booking_amount | [Amount Object](#amount_object) | The closing balance of the account. `10,000.00 GBP`|
| value_amount | [Amount Object](#amount_object) | The closing value of the account. `10,000.00 GBP`|

*Example balance Object:*

```js
{
    "balance": {
        "date": "2014-01-12",
        "booking_amount": {amount},
        "value_amount": {amount}
    }
}
```

<hr />

#### <a id="beneficiary_bank_object"></a> Holder Bank Object ####

When a beneficiary bank is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| bic | String | **Required if swift format.** Eight or eleve-digit [ISO 9362 Business Identifier Code](http://en.wikipedia.org/wiki/ISO_9362) specifying the Recipient Bank. `CHASUS33XXX` |
| clearingType | String | **Required if local format.** Two-digit code specifying the local clearing network. `FW` |
| clearingCode | String | **Required if local format.** The branch number on the local clearing network `021000021` |
| name | String | **Required if local format.** The beneficiary bank name. `JPMORGAN CHASE BANK, N.A.` |
| address | [Address Object](#address_object) | **Required if local format.** The beneficiary bank address. |

*Example Holder Bank Object:*

```js
{
  "bic": "CHASUS33",
  "clearingType": "FW",
  "clearingCode": "021000021",
  "name": "JPMORGAN CHASE BANK, N.A.",
  "address": {address}
}
```

<hr />

#### <a id="beneficiary_object"></a> Holder Object ####

Definition: Beneficiary - The Individual or Organisation to receive payment.
May also be referred to as: Supplier/Vendor/Payee/Recipient
The Holder Object must store the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| name | String | **Required.** The name of the account owner. `John Doe`|
| type | String | **Required.** The type of account owner. `Individual` |
| address | [Address Object](#address_object) | The account owner address. |

*Example Holder Object:*

```js
{
    "holder": {
        "name": "John Doe",
        "type": "Individual",
        "address": {address},
    }
}
```

<hr />

#### <a id="correspondent_bank_object"></a> Correspondent Bank Object ####

When a correspondent bank of an account is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| bic | String | **Required if local format.** `CHASUS33` |
| name | String | The bank name. `CREDIT AGRICOLE SA` |
| address | [Address Object](#address_object) | The bank address. |

*Example Correpondent Bank Object:*

```js
"correspondant_bank":{
    "bic": "AGRIFRPP",
    "name": "CREDIT AGRICOLE SA",
    "address": {address}
},
```

<hr />

#### <a id="payment_object"></a> Payment Object ####

When a `payment` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| payment_id | String | **Required.** id of the payment. `xxx` |
| status | String | Payment status. `Awaiting Confirmation` |
| created_date | Date Time | Creation date of the payment. `2014-01-12T00:00:00+00:00` |
| updated_date | Date Time | Last update on the payment. `2014-01-12T00:00:00+00:00` |
| initial_execution_date | Date | The initial date of execution when the payment is created. `YYYY-MM-DD` |
| confirmation_date | Date | We consider the payment confirmed when the status turns scheduled. `YYYY-MM-DD` |
| execution_date | Date | We consider the payment executed when the status turns finalized. `YYYY-MM-DD` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount to be transfered. `10,000.00 GBP` |
| type | String | Defines the type of payment affected. `Standard` |
| tag | String | Custom reference on the payment. `Invoice xxx` |
| account_id | [Account Object](#account_object) | **Required.** Details of the destination account. |

*Example Payment Object:*

```js
    "payment":{
        "payment_id": "xxx",
        "status": "Awaiting Confirmation",
        "type": "Standard",
        "tag": "Invoice xxx",
        "created_date": "2014-01-12T00:00:00+00:00",
        "initial_execution_date": "2014-01-12T00:00:00+00:00",
        "amount": {amount},
        "account_id": {account},
    },
```

<hr />

#### <a id="trade_object"></a> Trade Object ####

When a `trade` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| trade_id | String | **Required.** id of trade `xxx` |
| nominal_amount | [Amount Object](#amount_object) | **Required.** The nominal amount of the trade. `10,000.00 GBP` |
| settlement_amount | [Amount Object](#amount_object) | **Required.** The amount that will be debited on the related source `wallet` account at delivery date. `10,000.00 GBP` |
| delivered_amount | [Amount Object](#amount_object) | **Required.** The amount that will be credited on the related target `wallet` account at delivery date. `10,000.00 GBP` |
| rate_applied | String | **Required.** The nominal amount of the trade. `1.10000` |
| currency_pair | String | **Required.** The instrument related to the rate applied. `EURUSD` |
| rate | [Rate Object](#rate_object) | **Required.** The Core and mid-market real time rates related to this trade. |
| delivery_date | Date | `YYYY-MM-DD` |
| created_date | Date | `YYYY-MM-DD` |

*Example Trade Object:*

```js
    "trade": {
        "created_date": "2014-01-12T00:00:00+00:00",
        "delivery_date": "2014-01-12T00:00:00+00:00",
        "nominal_amount": {amount},
        "rate_applied": "1.1002",
        "currency_pair": "EURUSD",
        "rate": {rate}
        "settlement_amount": {amount},
        "delivered_amount": {amount},
    }
```

<hr />

#### <a id="financial_movment_object"></a> Financial Movements Object ####

When a financial movement is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount to be transfered. `10,000.00 GBP` |
| accountSource | [Account Object](#account_object) | Details of the settlement account. |
| accountTarget | [Account Object](#account_object) | Details of the delivery account. |
| bookingDate | Date | `YYYY-MM-DD` |
| valueDate | Date | `YYYY-MM-DD` |
| communication | String | Communication in the free format field of the transfer. `Invoice XXX` |

*Example Transfer Object:*

```js
-> TBD
```

<hr />

#### <a id="rate_object"></a> Rate Object ####

As a rate is specified in a JSON body, it's encoded as an object with five fields:

| Field | Type | Description |
|-------|------|-------------|
| currencyPair | String | The cross of currency used for the rates provided |
| midMarket | String (Quoted decimal) | The average rate of the market between the bid and the ask rate. |
| date | Date | The date of the last update on this currency. |
| coreAsk | String (Quoted decimal) | The interbank BID rate provided by the FX partner of FX4BIZ . |
| coreBid | String (Quoted decimal) | The interbank ASK rate provided by the FX partner of FX4BIZ. |

Example Rate Object:

```js
"rate":{
	"currencyPair":"EURGBP",
	"minMarket":"0.726500",
	"date":"2015-03-31 13:15:04",
	"coreAsk":"0.726500",
	"coreBid":"0.726500"
}
```

<hr />

#### <a id="quote_object"></a> Quote Object ####

When a `quote` is specified as part of a JSON body, it is encoded as an object with four fields:

| Field | Type | Description |
|-------|------|-------------|

Example Quote Object:

```js
--> TBD
```

<hr />

#### <a id="log_object"></a> Log Object ####

When a `log` is specified as part of a JSON body, it is encoded as an object with four fields:

| Field | Type | Description |
|-------|------|-------------|
| CreatedAt | Date | The date when the log entry was created. |
| ClosedAt | Date | The date when the log entry was closed. |
| TokenNonce | String | The nonce used in the HTTP header to authenticate the request. |
| RemoteAddress | String | The address of the request's emiter. |
| RequestMethod | String | The [HTTP method](http://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol#M.C3.A9thodes) of the request. |
| UriRequested | String | The [Universal Resource Identifier](http://fr.wikipedia.org/wiki/Uniform_Resource_Identifier) given for this request. |
| ParametersGiven | String | The optional parameters *(e.g. after the ?)* given for this request. |
| RequestBody | String | The [HTTP](http://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol) request body. |
| HttpResponseCode | Integer | The [HTTP response code](http://fr.wikipedia.org/wiki/Liste_des_codes_HTTP). |
| ResponseBody | String | The text sent by the server as a result for the request. |
| RestErrorTypeId | Integer | If there is an error during the processing the request, this id could be used to find this error. |
| Login | String | The login used for the request. |

Example Log Object:

```js
"log": {
	"CreatedAt": "2015-04-14 10:12:40",
	"ClosedAt": "2015-04-14 10:12:40",
	"TokenNonce": "MjVmOGZmYjI2MjU3NzlkMw==",
	"RemoteAddress": "::1",
	"RequestMethod": "GET",
	"UriRequested": "/app_dev.php/api/rates/EURUSD",
	"ParametersGiven": "a:0:{}",
	"RequestBody": null,
	"HttpResponseCode": "200",
	"ResponseBody": "{"rates":[{"currency_pair":"EURUSD","min_market":"1.074300","date":"2015-03-31 13:15:04","core_ask":"1.074300","core_bid":"1.074300"}]}",
	"RestErrorTypeId": null,
	"Login": "p00005m",
}
```

