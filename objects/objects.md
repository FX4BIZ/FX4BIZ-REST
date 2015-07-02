# API Objects #

* [External Bank Account Object](#account_object)
* [Wallet Object](#wallet_object)
* [Address Object](#address_object)
* [Amount Object](#amount_object)
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

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the account. `xxx` |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation [DateTime](../conventions/formatingConventions.md#type_datetime) of the object: `YYYY-MM-DD HH:MM:SS` |
| createdBy |  String | The creation user of the object: `api` |
| currency | String | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `USD` |
| tag |  String | Custom data. `reference` |
| status |  String | Status of the account `active` |
| type |  String | type of account `wallet` |
| number | String | Iban or account number. `xxx384` |
| correspondentBank | [Correspondent Bank Object](#correspondent_bank_object) | **Required for local format.** The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [Holder Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| holder | [Holder Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |

**Example Account Object:**

```js
"account": {
    "id": "xxx"
    "status": "active",
    "type": "wallet",
    "createdDate": "2014-01-12 00:00:00",
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
| id |  [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the account. `xxx` |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation [DateTime](../conventions/formatingConventions.md#type_datetime) of the object: `YYYY-MM-DD HH:MM:SS` |
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
| value  | Float | The quantity of the currency. `25000000.00` |
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | The [Currency](../conventions/formatingConventions.md#type_currency) specifying the amount currency. `USD` |

*Example Amount Object:*

```js
"amount": {
	"value": 10000.00,
	"currency": "GBP"
}
```

<hr />

#### <a id="balance_object"></a> Balance Object ####

When the balance is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| closingDate | [Date](../conventions/formatingConventions.md#type_date) | The closing [Date](../conventions/formatingConventions.md#type_date) of the balance details given. `YYYY-MM-DD` |
| bookingAmount | [Amount Object](#amount_object) | The closing balance of the account. |
| valueAmount | [Amount Object](#amount_object) | The closing value of the account. |

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
| id | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the payment. `xxx` |
| status | String | Payment status. `Awaiting Confirmation` |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | Creation [DateTime](../conventions/formatingConventions.md#type_datetime) of the payment. `YYYY-MM-DD HH:MM:SS` |
| desiredExecutionDate | [Date](../conventions/formatingConventions.md#type_date) | The initial [Date](../conventions/formatingConventions.md#type_date) of execution when the payment is created. `YYYY-MM-DD` |
| executionDate | [Date](../conventions/formatingConventions.md#type_date) | The  [Date](../conventions/formatingConventions.md#type_date) of execution of the payment. `YYYY-MM-DD` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount to be transfered. `10,000.00 GBP` |
| tag | String | Custom reference on the payment. `Invoice xxx` |
| beneficiaryAccountId | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the beneficiary account. |
| sourceWalletId | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the wallet the payment will be processed. |
| communication | String | The wording of the payment. |
| priorityPaymentOption | String | A string representing wether this payment as a normal priority, or it as to be done quick. |
| feePaymentOption | String | A string representing the fee payment option for this payment. |

*Example Payment Object:*

```js
"payment": {
    "id": "XXXXXX",
    "status": "WaitingConfirmation",
    "createdDate": "2015-06-29 16:50:30",
    "desiredExecutionDate": "2015-06-30",
    "executionDate": "2015-06-30",
    "amount": {
        "value": "5500.00",
        "currency": "USD"
    },
    "tag": "XXXXXX",
    "beneficiaryAccountId": "XXXXXX",
    "sourceWalletId": "XXXXXX",
    "communication": "XXXXXX",
    "priorityPaymentOption": "normal",
    "feePaymentOption": "BEN"
}
```

<hr />

#### <a id="trade_object"></a> Trade Object ####

When a `trade` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the quote. |
| side | String | The side of the quote. 'B' for buy, and 'S' for sell. |
| settlementAmount | [Amount Object](#amount_object)  | The [Amount Object](#amount_object) representing the amount to settle. |
| deliveredAmount | [Amount Object](#amount_object)  | The [Amount Object](#amount_object) representing the amount to be delivered. |
| settlementWalletId | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the source account. `xxx` |
| deliveryWalletId | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the destination account. `xxx` |
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | The currency pair representing the quote.  |
| rate | [Rate Object](#rate_object) | The [Rate Object](#rate_object) describing the rate of the quote. |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation [DateTime](../conventions/formatingConventions.md#type_datetime) of the quote. |
| deliveryDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The delivery [DateTime](../conventions/formatingConventions.md#type_datetime) of the trade. |

*Example Trade Object:*

```js
"trade": {
    "id": "NTUzMzA",
    "side": "B",
    "settlementAmount": {
        "value": 9013.89,
        "curreny": "EUR"
    },
    "deliveredAmount": {
        "value": 10000,
        "currency": "USD"
    },
    "settlementWalletId": "XXXXX",
    "deliveryWalletId": "XXXXX",
    "currencyPair": "EURUSD",
    "rate": {
        "currencyPair": "EURUSD",
        "midMarket": 1.1094,
        "coreAsk": 1.1094,
        "coreBid": 1.1174,
        "date": "2015-06-29 11:46:36"
    },
    "createdDate": "2015-06-29 11:46:36",
    "deliveryDate": "2015-06-30 00:00:00"
}
```

<hr />

#### <a id="financial_movement_object"></a> Financial Movements Object ####

When a financial movement is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the financial movement. |
| bookingDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The booking [DateTime](../conventions/formatingConventions.md#type_datetime) of the financial movement. |
| valueDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The value [DateTime](../conventions/formatingConventions.md#type_datetime) of the financial movement. |
| orderingAccountNumber | String | The number refering the ordering account. |
| beneficiaryAccountNumber | String | The number refering the beneficiary account. |
| orderingCustomer | String | A free formatted String representing the ordering customer with it's name and it's address. |
| orderingInstitution | String | A free formatted String representing the ordering institution with it's name and it's address. |
| beneficiaryCustomer | String | A free formatted String representing the beneficiary customer with it's name and it's address. |
| amount | [Amount Object](#amount_object)  | The [Amount Object](#amount_object) of the financial movement. |

*Example Financial Movement Object:*

```js
"financialMovement": {
        "id": "XXXXXXX",
        "bookingDate": "2015-04-30 00:00:00",
        "valueDate": "2015-04-30 00:00:00",
        "orderingAccountNumber": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "beneficiaryAccountNumber": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "orderingCustomer": "Diamants Plus Avenue de l'opéra Paris FR",
        "orderingInstitution": "BNP-PARIBAS SA (FORMERLY BANQUE NATIONALE DE PARIS S.A.) 16 BOULEVARD DES ITALIENS   75450 PARIS CEDEX 09 FR",
        "BeneficiaryCustomer": "Diamants Plus Avenue de l'opéra Paris FR",
        "amount": {
            "value": "4573.00",
            "currency": "EUR"
        }
}
```

<hr />

#### <a id="rate_object"></a> Rate Object ####

As a rate is specified in a JSON body, it's encoded as an object with five fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | The [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) used for the rates provided |
| midMarket | Float | The average rate of the market between the bid and the ask rate. |
| date | String | The date of the last update on this currency. `YYYY-MM-DD HH:MM:SS` |
| coreAsk | Float | The interbank BID rate provided by the FX partner of FX4BIZ . |
| coreBid | Float | The interbank ASK rate provided by the FX partner of FX4BIZ. |

Example Rate Object:

```js
"rate":{
	"currencyPair":"EURGBP",
	"midMarket":0.726500,
	"date":"2015-03-31 13:15:04",
	"coreAsk":0.726500,
	"coreBid":0.726500
}
```

<hr />

#### <a id="quote_object"></a> Quote Object ####

When a `quote` is specified as part of a JSON body, it is encoded as an object with four fields:

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the quote. |
| side | String | The side of the quote. 'B' for buy, and 'S' for sell. |
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | The [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) representing the quote.  |
| rate | [Rate Object](#rate_object) | The [Rate Object](#rate_object) describing the rate of the quote. |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation [DateTime](../conventions/formatingConventions.md#type_datetime) of the quote. |
| deliveryDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The delivery [DateTime](../conventions/formatingConventions.md#type_datetime) of the quote. |

Example Quote Object:

```js
"quote": {
    "id": "NTUzMzA",
    "side": "B",
    "currencyPair": "EURUSD",
    "rate": {
        "currencyPair": "EURUSD",
        "midMarket": 1.1094,
        "coreAsk": 1.1094,
        "coreBid": 1.1174,
        "date": "2015-06-29 11:46:36"
    },
    "createdDate": "2015-06-29 11:46:36",
    "deliveryDate": "2015-06-30 00:00:00"
}
```

<hr />

#### <a id="log_object"></a> Log Object ####

When a `log` is specified as part of a JSON body, it is encoded as an object with the following fields:

| Field | Type | Description |
|-------|------|-------------|
| CreatedAt |  [DateTime](../conventions/formatingConventions.md#type_datetime) | The  [DateTime](../conventions/formatingConventions.md#type_datetime) when the log entry was created. |
| ClosedAt |  [DateTime](../conventions/formatingConventions.md#type_datetime) | The  [DateTime](../conventions/formatingConventions.md#type_datetime) when the log entry was closed. |
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
