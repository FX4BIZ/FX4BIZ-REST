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
* [Process Result Object](#processresult_object)

## Details ##

#### <a id="account_object"></a> External Bank Account Object ####

When an account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formatingConventions.md#type_id) | The code identifying the account. |
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| status |  String | The code specifying the status of the account. `authorized | locked | not authorized` |
| accountNumber | String(40) | The code specifying the account (can be either an Iban or an account number). |
| correspondentBank | [Correspondent Bank Object](#correspondent_bank_object) | The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [Holder Bank Object](#beneficiary_bank_object) | The recipient bank details, holding the account. |
| holder | [Holder Object](#beneficiary_object) | The recipient details, owner of the account. |

**Example:**

```js
"account": {
    "id": "NT4edA",
    "currency": "EUR",
    "tag": "My wallet account EUR",
    "status": "Authorized",
    "accountNumber": "516981638516313513",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
	"holder":{beneficiary}
}
```

<hr />

#### <a id="wallet_object"></a>Wallet Object ####

When a wallet is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id |  [ID](../conventions/formatingConventions.md#type_id) | The code identifying of the account. |
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference associated to this wallet. (For internal use only, not communicated to any beneficiary). |
| status |  String | The code identifying the status of the account. `authorized | locked | not authorized` |
| accountNumber | String | Iban or account number. |
| correspondentBank | [Correspondent Bank Object](#correspondent_bank_object) | The intermediary bank details, used to reach the beneficiary bank. |
| holderBank | [Holder Bank Object](#beneficiary_bank_object) | The recipient bank details, holding the account. |
| holder | [Holder Object](#beneficiary_object) | The recipient details, owner of the account. |

**Example:**

```js
"wallet": {
    "id": "ND2da3"
    "status": "authorized",
    "tag": "My wallet account EUR",
    "accountNumber": "654165816813556358",
    "currency": "EUR",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
	"holder":{beneficiary}
}
```

<hr />

#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| street | String(255) | The street for the address described. |
| postCode | String(15) | The ZIP/Post code for the address described. |
| city | String(35) | The city for the address described. |
| state | String(35) | The state for the address described. |
| country | String(2) | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```js
"address": {
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"state": "New York",
	"country": "US"
}
```

<hr />

#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | Float | The quantity of the currency. |
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```js
"amount": {
	"value": 10000.00,
	"currency": "GBP"
}
```

<hr />

#### <a id="balance_object"></a> Balance Object ####

When the balance is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| closingDate | [Date](../conventions/formatingConventions.md#type_date) | The closing date of the balance details given. |
| bookingAmount | [Amount Object](#amount_object) | The closing balance of the account. |
| valueAmount | [Amount Object](#amount_object) | The closing value of the account. |

**Example:**

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

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| bic | String(11) | Eight or eleve-digit [ISO 9362 Business Identifier Code](http://en.wikipedia.org/wiki/ISO_9362) specifying the Recipient Bank. |
| clearingType | String(2) | The two-digit code specifying the local clearing network. |
| clearingCode | String(15) | The code identifying the branch number on the local clearing network. |
| name | String(120) | The beneficiary bank name. |
| address | [Address Object](#address_object) | The beneficiary bank address. |

**Example:**

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

What we call a Holder can be either an Individual or an Organisation that own the account.
May also be referred to as: Beneficiary/Supplier/Vendor/Payee/Recipient.

The Holder Object must store the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| name | String(100) | The name of the account owner. |
| type | String(10) | The code identifying the type of account owner. `Individual | Corporate` |
| address | [Address Object](#address_object) | The account owner address. |

**Example:**

```js
"holder": {
    "name": "John Doe",
    "type": "Individual",
    "address": {address},
}
```

<hr />

#### <a id="correspondent_bank_object"></a> Correspondent Bank Object ####

When a correspondent bank related to an account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| bic | String(11) | A code representing the correspondant bank identifyer code. |
| name | String(120) | The correspondant bank name. |
| address | [Address Object](#address_object) | The correspondant bank address. |

**Example:**

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

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The [ID](../conventions/formatingConventions.md#type_id) of the payment. |
| status | String | The code identifying the payment status. ` Planified | Rejected | Finalized | Canceled | Refused | blocked | WaitingConfirmation ` |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation date of the payment. |
| desiredExecutionDate | [Date](../conventions/formatingConventions.md#type_date) | The initial date of execution when the payment is created. |
| executionDate | [Date](../conventions/formatingConventions.md#type_date) | The effective date of execution of the payment. |
| amount | [Amount Object](#amount_object) | The nominal amount to be transfered. |
| tag | String(50) | The custom reference related to the payment. (For internal use only, not communicated to the beneficiary) |
| externalBankAccountId | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the beneficiary account. |
| sourceWalletId | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the wallet the payment will be processed. |
| communication | String(76) | The wording of the payment. |
| priorityPaymentOption | String(6) | The code representing whether this payment as a normal priority or if it as to be treated with a priority status by all the routing banks. `normal | urgent` |
| feePaymentOption | String(5) | The code identifying the charges option for this payment. `BEN | OUR | SHARE` |

**Example:**

```js
"payment": {
    "id": "ND4ue2",
    "status": "WaitingConfirmation",
    "createdDate": "2015-06-29 16:50:30",
    "desiredExecutionDate": "2015-06-30",
    "executionDate": "2015-06-30",
    "amount": {amount},
    "tag": "Invoice #11854854",
    "externalBankAccountId": "NTd4ME",
    "sourceWalletId": "ND4uME",
    "communication": "Payment for invoce",
    "priorityPaymentOption": "normal",
    "feePaymentOption": "BEN"
}
```

<hr />

#### <a id="trade_object"></a> Trade Object ####

When a `trade` is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the trade. |
| status | String | The code identifying the payment status. `Plannified | Canceled | Finalized` |
| side | String(1) | The side of the trade. `B | S` |
| sourceAmount | [Amount Object](#amount_object)  | The amount to be debited from the source Wallet. |
| deliveredAmount | [Amount Object](#amount_object)  | The amount to be credited to the delivery wallet |
| sourceWalletId | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the account to be debited of the amount of currency sold. |
| deliveryWalletId | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the destination account of the amount of currencies bought. |
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | A six-digit code representing the cross of currency or isntrument requested. |
| rate | [Rate Object](#rate_object) | The [Rate Object](#rate_object) describing FX market information asoociated to the trade. |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation date of the trade |
| deliveryDate | [Date](../conventions/formatingConventions.md#type_date) | The delivery date of the trade. |

**Example:**

```js
"trade": {
    "id": "NTUzMzA",
    "side": "B",
    "sourceAmount": {amount},
    "deliveredAmount": {amount},
    "sourceWalletId": "TD4eAB",
    "deliveryWalletId": "ND3eaB",
    "currencyPair": "EURUSD",
    "rate": {rate},
    "createdDate": "2015-06-29 11:46:36",
    "deliveryDate": "2015-06-30"
}
```

<hr />

#### <a id="financial_movement_object"></a> Financial Movements Object ####

When a financial movement is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the transfer. |
| bookingDate | [Date](../conventions/formatingConventions.md#type_date) | The booking date of the transfer. |
| valueDate | [Date](../conventions/formatingConventions.md#type_date) | The value date of the transfer. |
| orderingAccountNumber | String(40) | The number referring the ordering account of the transfer. |
| beneficiaryAccountNumber | String(40) | The number referring the beneficiary account. |
| orderingCustomer | String | A free formatted String representing the ordering customer with it's name and it's address. |
| orderingInstitution | String | A free formatted String representing the ordering institution with it's name and it's address. |
| beneficiaryCustomer | String | A free formatted String representing the beneficiary customer with it's name and it's address. |
| orderingAmount | [Amount Object](#amount_object) | The amount instructed by the ordering customer of the transfer. |
| beneficiaryAmount | [Amount Object](#amount_object) | The amount delivered credited in the beneficiary account. |
| RemittanceInformation  | The communication field. |
| chargesDetails  | String | The charges details related to the transfer. |
| exchangeRate | Float | The exchange rate applied on the transfer. |

**Example:**

```js
"financialMovement": {
        "id": "XXXXXXX",
        "bookingDate": "2015-04-30",
        "valueDate": "2015-04-30",
        "orderingAccountNumber": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "beneficiaryAccountNumber": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        "orderingCustomer": "Diamants Plus Avenue de l'opéra Paris FR",
        "orderingInstitution": "BNP-PARIBAS SA (FORMERLY BANQUE NATIONALE DE PARIS S.A.) 16 BOULEVARD DES ITALIENS   75450 PARIS CEDEX 09 FR",
        "BeneficiaryCustomer": "Diamants Plus Avenue de l'opéra Paris FR",
        "orderingAmount": {amount},
        "beneficiaryAmount": {amount},
        "RemittanceInformation": "Billing #1385438",
        "chargesDetails": "-4200 USD BEN",
        "exchangeRate": 1.2400
}
```

<hr />

#### <a id="rate_object"></a> Rate Object ####

As a rate is specified in a JSON body, it's encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | The three-digit code used for the rates provided |
| midMarket | String | The average rate of the market between the bid and the ask rate. |
| date | [DateTime](../conventions/formatingConventions.md#type_datetime) | The date representing the last update on the update. |
| coreAsk | String | The interbank BID rate provided by the FX partner of FX4BIZ. |
| coreBid | String | The interbank ASK rate provided by the FX partner of FX4BIZ. |
| appliedAsk | String | The interbank BID rate applied for the transaction. |
| appliedBid | String | The interbank ASK rate applied for the transaction. |

**Example:**

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

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | The code identifying the quote. |
| side | String(1) | The code identifying the side of the quote. `B | S` |
| currencyPair | [CurrencyPair](../conventions/formatingConventions.md#type_currencypair) | The three-digit code representing the quote.  |
| rate | [Rate Object](#rate_object) | The Object describing the rate of the quote. |
| createdDate | [DateTime](../conventions/formatingConventions.md#type_datetime) | The creation date of the quote. |
| deliveryDate | [Date](../conventions/formatingConventions.md#type_date) | The delivery date of the quote. |
| sourceAmount | [Amount Object](../objects/objects.md#amount_object) |  Amount to be sent for this quote. |
| deliveredAmount | [Amount Object](../objects/objects.md#amount_object) | Amount to be recieved by the beneficiary for this quote. |

**Example:**

```js
"quote": {
    "id": "NTUzMzA",
    "side": "B",
    "currencyPair": "EURUSD",
    "rate": {rate},
    "createdDate": "2015-06-29 11:46:36",
    "deliveryDate": "2015-06-30",
    "sourceAmount": {amount},
    "deliveredAmount": {amount}
}
```

<hr />

#### <a id="log_object"></a> Log Object ####

When a `log` is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| CreatedAt |  [DateTime](../conventions/formatingConventions.md#type_datetime) | The  [DateTime](../conventions/formatingConventions.md#type_datetime) when the log entry was created. |
| ClosedAt |  [DateTime](../conventions/formatingConventions.md#type_datetime) | The  [DateTime](../conventions/formatingConventions.md#type_datetime) when the log entry was closed. |
| TokenNonce | String | The nonce used in the HTTP header to authenticate the request. |
| RemoteAddress | String(15) | The address of the request's emiter. |
| RequestMethod | String(6) | The [HTTP method](http://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol#M.C3.A9thodes) of the request. |
| UriRequested | String | The [Universal Resource Identifier](http://fr.wikipedia.org/wiki/Uniform_Resource_Identifier) given for this request. |
| ParametersGiven | String | The optional parameters *(e.g. after the ?)* given for this request. |
| RequestBody | String | The [HTTP](http://fr.wikipedia.org/wiki/Hypertext_Transfer_Protocol) request body. |
| HttpResponseCode | Integer | The [HTTP response code](http://fr.wikipedia.org/wiki/Liste_des_codes_HTTP). |
| ResponseBody | String | The text sent by the server as a result for the request. |
| RestErrorTypeId | Integer | If there is an error during the processing the request, this id could be used to find this error. |
| Login | String(7) | The login used for the request. |

**Example:**

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

<hr />

#### <a id="processresult_object"></a> Process Result Object ####

As some of our process just need to send you back the confirmation that this process is successful, the API will send you a Process Result Object.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| result | Boolean | The result of the operation. `true` if the operation is successful, else `false` |

**Example:**

```js
"process":{
    "result": true,
}
```
