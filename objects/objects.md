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
"account": {
    "id": "xxx"
    "status": "active",
    "type": "wallet",
    "createdDate": "2014-01-12T00:00:00+00:00",
    "createdBy": "api",
    "tag": "My wallet account EUR",
    "number": "xxx4548",
    "currency": "EUR",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
	"holder":{beneficiary}
}
```

<hr />

#### <a id="wallet_object"></a>Wallet Object ####

When a wallet is specified as part of a JSON body, it is encoded as an object with the following fields:

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
"wallet": {
    "id": "xxx"
    "status": "active",
    "type": "wallet",
    "createdDate": "2014-01-12T00:00:00+00:00",
    "createdBy": "api",
    "tag": "My wallet account EUR",
    "number": "xxx4548",
    "currency": "EUR",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
	"holder":{beneficiary}
}
```

<hr />

#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with four fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| street | String | The street for the address described. |
| postCode | String | The ZIP/Post code for the address described. |
| city | String | The city for the address described. |
| country | String | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

*Example Address Object:*

```js
"address": {
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"country": "US"
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
"amount": {
	"value": "10000.00",
	"currency": "GBP"
}
```

<hr />

#### <a id="balance_object"></a> Balance Object ####

When the balance is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| closingDate | Date | The closing date of the balance details given. `2014-01-12` |
| bookingAmount | [Amount Object](#amount_object) | The closing balance of the account. `10,000.00 GBP`|
| valueAmount | [Amount Object](#amount_object) | The closing value of the account. `10,000.00 GBP`|

*Example balance Object:*

```js
"balance": {
    "closingDate": "2014-01-12",
    "bookingAmount": {amount},
    "valueAmount": {amount}
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
"beneficiaryBank" {
	"bic": "CHASUS33",
	"clearingType": "FW",
	"clearingCode": "021000021",
	"name": "JPMORGAN CHASE BANK, N.A.",
	"address": {address}
}
```

<hr />

#### <a id="beneficiary_object"></a> Holder Object ####

Definition: Holder - The Individual or Organisation that hold an account.
May also be referred to as: Beneficiary/Supplier/Vendor/Payee/Recipient
The Holder Object must store the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| name | String | **Required.** The name of the account owner. `John Doe`|
| type | String | **Required.** The type of account owner. `Individual` |
| address | [Address Object](#address_object) | The account owner address. |

*Example Holder Object:*

```js
"holder": {
    "name": "John Doe",
    "type": "Individual",
    "address": {address},
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
}
```

<hr />

#### <a id="payment_object"></a> Payment Object ####

When a `payment` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| payment_id | String | **Required.** id of the payment. `xxx` |
| status | String | Payment status. `Awaiting Confirmation` |
| createdDate | Date Time | Creation date of the payment. `2014-01-12T00:00:00+00:00` |
| wantedExecutionDate | Date | The initial date of execution when the payment is created. `YYYY-MM-DD` |
| executionDate | Date | We consider the payment executed when the status turns finalized. `YYYY-MM-DD` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount to be transfered. `10,000.00 GBP` |
| type | String | Defines the type of payment affected. `Standard` |
| tag | String | Custom reference on the payment. `Invoice xxx` |
| beneficiaryName | String | The name of the beneficiary. |
| beneficiaryAccountNumber | String | The account number of the beneficiary. |


*Example Payment Object:*

```js

```

<hr />

#### <a id="trade_object"></a> Trade Object ####

When a `trade` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| tradeId | String | **Required.** id of trade `xxx` |
| nominalAmount | [Amount Object](#amount_object) | **Required.** The nominal amount of the trade. `10,000.00 GBP` |
| settlementAmount | [Amount Object](#amount_object) | **Required.** The amount that will be debited on the related source `wallet` account at delivery date. `10,000.00 GBP` |
| deliveredAmount | [Amount Object](#amount_object) | **Required.** The amount that will be credited on the related target `wallet` account at delivery date. `10,000.00 GBP` |
| rateApplied | String | **Required.** The nominal amount of the trade. `1.10000` |
| currencyPair | String | **Required.** The instrument related to the rate applied. `EURUSD` |
| rate | [Rate Object](#rate_object) | **Required.** The Core and mid-market real time rates related to this trade. |
| deliveryDate | Date | `YYYY-MM-DD` |
| createdDate | Date | `YYYY-MM-DD` |

*Example Trade Object:*

```js
"trade": {
    "createdDate": "2014-01-12T00:00:00+00:00",
    "deliveryDate": "2014-01-12T00:00:00+00:00",
    "nominalAmount": {amount},
    "rateApplied": "1.1002",
    "currencyPair": "EURUSD",
    "rate": {rate}
    "settlementAmount": {amount},
    "deliveredAmount": {amount},
}
```

<hr />

#### <a id="financial_movement_object"></a> Financial Movements Object ####

When a financial movement is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | The id of the financial movement. |
| bookingDate | Date | The booking date of the financial movement. |
| valueDate | Date | The value date of the financial movement. |
| orderingAccountNumber | String | The number refering the ordering account. |
| BeneficiaryAccountNumber | String | The number refering the beneficiary account. |
| orderingCustomer | String | A free formatted String representing the ordering customer with it's name and it's address. |
| orderingInstitution | String | A free formatted String representing the ordering institution with it's name and it's address. |
| beneficiaryCustomer | String | A free formatted String representing the beneficiary customer with it's name and it's address. |
| account | [Account Object](#account_object) | Details of the settlement account. |


*Example Financial Movement Object:*

```js
"financialMovement": {
    "id": "XXX",
    "bookingDate": "2015-04-24 08:55:18",
    "valueDate": "2015-04-24 00:00:00",
    "orderingAccountNumber": "XXXXXXXXXXXXXXXXXXX",
    "BeneficiaryAccountNumber": "XXXXXXXXXXXXXXXXXXX",
    "orderingCustomer": "John Doe 31 1st New York US",
    "orderingInstitution": "JPMORGAN CHASE BANK, N.A. 4 NEW YORK PLAZA FLOOR 15 NEW YORK,NY 10004 US",
    "BeneficiaryCustomer": "Jane Doe 32 1st New York US",
    "amount": {amount}
}
```

<hr />

#### <a id="rate_object"></a> Rate Object ####

As a rate is specified in a JSON body, it's encoded as an object with five fields:

*Object resources:*

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
	"midMarket":"0.726500",
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

When a `log` is specified as part of a JSON body, it is encoded as an object with the following fields:

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
	"createdAt": "2015-04-14 10:12:40",
	"closedAt": "2015-04-14 10:12:40",
	"tokenNonce": "MjVmOGZmYjI2MjU3NzlkMw==",
	"remoteAddress": "::1",
	"requestMethod": "GET",
	"uriRequested": "/app_dev.php/api/rates/EURUSD",
	"parametersGiven": "a:0:{}",
	"requestBody": null,
	"httpResponseCode": "200",
	"responseBody": "{"rates":[{"currencyPair":"EURUSD","midMarket":"1.074300","date":"2015-03-31 13:15:04","coreAsk":"1.074300","coreBid":"1.074300"}]}",
	"restErrorTypeId": null,
	"login": "p00005m",
}
```

