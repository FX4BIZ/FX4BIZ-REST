# FX4BIZ-REST API #

The FX4BIZ-REST API provides a simplified, easy-to-use interface to the FX4BIZ accounts & operations via a RESTful API. This page explains how to use the API to execute FX trades, send cross-boarder payments and monitoring accounts with FX4BIZ.

We recommend FX4BIZ-REST for financial institutions just getting started with FX4BIZ, since it provides high-level abstractions and convenient simplifications in the data format. 

## Available API Routes ##

Our API is divided into sections based on different concepts in our system. Each section is made up of a series of calls.

#### Authenticate ####

* [Get User Login - `GET /login-user`](#get-login-user)
* [Get End Session - `GET /end-session`](#get-end-session)

#### Accounts ####

* [Submit New Account - `POST /account`](#post-account-create)
* [Retrieve Accounts list - `GET /accounts`](#get-accounts-list)
* [Retrieve Account Details - `GET /account/{account_id}`](#get-account-details)
* [Update Account Details - `PUT /account/{account_id}`](#put-account-details)
* [Retrieve Transfer History - `GET /transfers`](#get-transfers-list)
* [Retrieve Transfer Details - `GET /transfer/{transfer_id}`](#get-transfer-details)
* [Delete Account - `DELETE /account/{account_id}`](#delete-account)

#### Payments ####

* [Submit Payment - `POST /payment`](#submit-payment)
* [Confirm Payment - `PUT /payment/{payment_id}/confirm`](#confirm-payment)
* [Retrieve Payments History - `GET /payments`](#get-payment-history)
* [Retrieve Payment Details - `GET /payment/{payment_id}`](#get-payment-history)
* [Update Payment Details - `PUT /payment/{payment_id}`](#put-payment-details)
* [Cancel Payment  - `DELETE /payment/{payment_id}`](#delete-payment)

#### Trades ####

* [Retrieve Rates - `GET /rates`](#get-rates)
* [Request Quote - `POST /quote`](#get-quote)
* [Submit Trade - `POST /trade`](#get-trade)
* [Cancel Trade - `DELETE /trade/{trade_id}`](#cancel-trade)
* [Retrieve Trades Book - `GET /trades`](#get-trade-book)
* [Retrieve Trade Details - `GET /trade/{trade_id}`](#get-trade-details)

## API Reference ##

The FX4BIZ API is organized around [REST](http://en.wikipedia.org/wiki/Representational_state_transfer). Our API is designed to have predictable, resource-oriented URLs and use the HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support [cross-origin resource sharing](http://en.wikipedia.org/wiki/Representational_state_transfer) to allow you to interact securely with our API from a client-side web application. [JSON](http://www.json.org/) will be returned in all responses from the API, including errors.

#### Objects List ####

* [Account Object](#account_object)
* [Address Object](#address_object)
* [Balance Object](#balance_object)
* [Beneficiary Bank Object](#beneficiary_bank_object)
* [Beneficiary Object](#beneficiary_object)
* [Correspondent Bank Object](#correspondent_bank_object)
* [Payment Object](#payment_object)
* [Payments Object](#payments_object)
* [Rate Object](#rate_object)
* [Trade Object](#trade_object)
* [Transfer Object](#transfer_object)
* [Quote Object](#trade_object)

#### Formatting Conventions ####

* [Cut-Off Times](#cut_off_times)
* [Errors](#errors_conventions)
* [Pagination](#pagination)
* [Quoted numbers](#quoted_numbers)
* [Versioning](#versioning)

## API Services ##

### Authentication Services ###

You authenticate to the FX4BIZ API by providing one of your API keys in the request. You can have multiple API keys active at one time. Your API keys carry many privileges, so be sure to keep them secret!

Authentication to the API occurs via [HTTP Auth.](http://en.wikipedia.org/wiki/Representational_state_transfer) Provide your API key as the basic auth username. You do not need to provide a password.

All API request must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTPS). Calls made over plain HTTP will fail. You must authenticate for all requests.

#### <a id="get-login-user"></a> Login ####
-> TBD

#### <a id="get-end-session"></a> End session ####
-> TBD

### <a id="account_services"></a> Account Services 

There are two kinds of accounts with FX4BIZ. What we call `wallet` account, which is an account hold in the FX4BIZ books and `external bank` account, which can be either your own account in another bank or a third party recipient account.

**As an example, a response for `GET /account/{account_id}/details` looks like this:**
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
        "correspondant_bank":{
            "bic": "AGRIFRPP",
            "name": "CREDIT AGRICOLE SA",
            "address": {
                "street": "BUILDING PASTEUR, BLOC 1: 91-93, BOULEVARD PASTEUR",
                "post_code": "75015",
                "city_name": "Paris",
                "state_or_province": "",
                "country": "FRANCE"
            }
        },
        "beneficiary_bank": {
            "bic": "FXBBBEBB",
            "name": "FX4BIZ SA",
            "address": {
                "street": "Avenue Louise, 350",
                "post_code": "1050",
                "city_name": "Bruxelles",
                "state_or_province": "Bruxelles-Capitale",
                "country": "FR"
            }
        },
        "beneficiary": {
            "name": "John Doe",
            "type": "individual",
            "address": {
                "street": "1 My Road",
                "post_code": "ZIP",
                "city_name": "London",
                "state_or_province": "",
                "country": "UK",
            }
        }
        
    }
}
```

#### <a id="post-account-create"></a> Submit account ####

```
Method: POST 
URL: /account
```
This service permits to reference a new `external bank` account. All `wallet` accounts are created automatically when subscribing with FX4BIZ.
This service include verifications on the format of the account created.
The API has been made in order to accept local specification of cross-boarder payments.

The Api accepts the following formats of `external bank` accounts :
- Bic & Iban
- Local UK format
- Local US format
- Local CA format
- Other local formats (unspecified but different from the previous).

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| Correspondent Bank | [Correspondent Bank Object](#correspondent_bank_object) | **Required for local format.** The intermediary bank details, used to reach the beneficiary bank. |
| Beneficiary Bank | [Beneficiary Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| Beneficiary | [Beneficiary Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |
| number | String | **Required.** The recipient account number or Iban. `xxx4548` |
| currency | String | **Required.** Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `EUR` |
| tag | String | Custom Data. `John Doe bank account EUR` |

As a response to this query, you will receive a json response containing details of the [Account](#account_object) created.

#### <a id="get-accounts-list"></a> Retrieve accounts list ####

```
Method: GET 
URL: /accounts
```
Retrieve the list of accounts referenced. 
This service also provides the balance of `wallet` type accounts.

As a response to this query, you will receive an Array containing the `account_id` and the [Balance](#balance_object) for each `wallet` account.

#### <a id="get-account-details"></a> Retrieve account details ####

```
Method: GET 
URL: /account/{account_id}
```
Retrieve bank details on a specific account. 

As a response to this query, you will receive the details of the [Account](#account_object).

#### <a id="put-account-details"></a> Update account details ####

```
Method: PUT 
URL: /account/{account_id}
```
Update information on an account or modify beneficiary bank or correspondent bank related to this one. 

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| Correspondent Bank | [Correspondent Bank Object](#correspondent_bank_object) | **Required for local format.** The intermediary bank details, used to reach the beneficiary bank. |
| Beneficiary Bank | [Beneficiary Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| Beneficiary | [Beneficiary Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |
| number | String | **Required.** The recipient account number or Iban. `xxx4548` |
| currency | String | **Required.** Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `EUR` |
| tag | String | Custom Data. `External bank account EUR` |

As a response to this query, you will receive the details of the [Account](#account_object) with updated information.

#### <a id="get-transfers-list"></a> Get transfers history ####

```
Method: GET 
URL: /transfers
```
Request the list of transfers that has been received or sent on a specific period of time.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| from_date | Date | List all transfers that has been credited or debited on your wallets account since this `booking_date`. `YYYY-MM-DD` |
| to_date | Date | List all transfers that has been credited or debited on your wallets account until this `booking_date`. `YYYY-MM-DD` | 

As a response to this query, you will receive an Array containing the `transfer_id`, `booking_date`, `value_date` and the [Amount](#amount_object) for all transfers that has been credited or debited on `booking date`.

#### <a id="get-transfer-details"></a> Retrieve transfer details ####

```
Method: GET 
URL: /transfer/{transfer_id}
```
Request information on a particular transfer that has been credited or debited to a wallet. 

As a response to this query, you will receive the details of the [Transfer](#transfer_object).

#### <a id="delete-account"></a> Delete account ####

```
Method: DELETE 
URL: /account/{account_id}
```
Delete an account.

As a response to this query, you will receive a JSON confirmation that the account has been deleted properly.

*Caution:* A `wallet` account cannot be deleted.

### Payment Service ###

Sending funds from your FX4BIZ wallet account to your own bank account or a third-party recipient involves two steps:

1. Generate the payment object with the [Create Payment method](#submit-payment). 
When you submit a payment to be scheduled, you assign a unique id to that payment. 

*Caution:* The payment created will be automatically rolled to the next closest working days if not confirmed in the scheduled date of operation.

2. Confirm the payment to the API for processing, using the [Confirm Payment method](#confirm-payment). 
When you confirm a payment for processing, make sure you have sufficient funds in your wallet account balance. The funds transfer will be automatically locked-in if the wallet account balance is not sufficient. Make sure you always have enough funds on your wallet.

*Caution:* If the balance of your wallet account is not sufficient to cover the payment amount, funds may be locked-in by FX4BIZ.

**As an example, a response for `GET /payment/{:id}` looks like this:**
```js
{
    "payment": {
        "id": "xxx",
        "status": "Awaiting Confirmation",
        "type": "Standard",
        "tag": "Invoice xxx",
        "created_date": "2014-01-12T00:00:00+00:00",
        "initial_execution_date": "2014-01-12T00:00:00+00:00",
        "amount": {
            "value": "125000.00",
            "currency": "USD"
        "account": {
            "account_id": "xxx"
            "status": "active",
            "type": "wallet",
            "created_date": "2014-01-12T00:00:00+00:00",
            "created_by": "api",
            "tag": "John Doe wallet account with FX4BIZ",
            "number": "xxx4548",
            "currency": "EUR",
            "correspondant_bank":{
                "bic": "AGRIFRPP",
                "name": "CREDIT AGRICOLE SA",
                "address": {
                    "street": "BUILDING PASTEUR, BLOC 1: 91-93, BOULEVARD PASTEUR",
                    "post_code": "75015",
                    "city_name": "Paris",
                    "state_or_province": "",
                    "country": "FRANCE"
                }
            },
            "beneficiary_bank": {
                "bic": "FXBBBEBB",
                "name": "FX4BIZ SA",
                "address": {
                    "street": "Avenue Louise, 350",
                    "post_code": "1050",
                    "city_name": "Bruxelles",
                    "state_or_province": "Bruxelles-Capitale",
                    "country": "FR"
                }
            },
            "beneficiary": {
                "name": "John Doe",
                "type": "individual",
                "address": {
                    "street": "1 My Road",
                    "post_code": "ZIP",
                    "city_name": "London",
                    "state_or_province": "",
                    "country": "UK"
                }
            }
        }
    }
}
```

#### <a id="submit-payment"></a> Submitting a payment ####

```
Method: POST 
URL: /payments
```
Use this path in order to schedule a new payment. 

As a response to this query, you will receive the details of the [Payment](#payment_object) created.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| account_id | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

#### <a id="confirm-payment"></a> Confirm a payment ####

```
Method: PUT 
URL: /payment/{payment_id}/confirm
```
Payments that has been scheduled must be confirmed in order to be release. 
If the payment is not confirmed on scheduled date of operation, it will be postponed to the next operation date available.

As a response to this query, you will receive the updated details of the [Payment](#payment_object) confirmed.

#### <a id="get-payments-history"></a> Retrieve payments history ####

```
Method: GET
URL: /payments
```
Request the list of payments that has been created on a specific period of time.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| from_date | Date | `YYYY-MM-DD` |
| to_date | Date | `YYYY-MM-DD` | 

#### <a id="get-payment-details"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payment
```

#### <a id="put-payment"></a> Update Payment Details####

```
Method: PUT
URL: /payment/{payment_id}
```

#### <a id="delete-payment"></a> Cancel Payment ####

```
Method: DELETE
URL: /payment/{payment_id}
```

### Trade Services ###

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


#### <a id="submit-rates"></a> Retrieve Rates ####

```
Method: GET
URL: /rates
```
The FX4BIZ-REST API provides a FX Data Feed. You can use the [Rates service](#rates_object) in order to ask for daily, hourly, minute, or real-time currency rates tables. Real-time rates are specified at mid-market.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| instruments | Array | **Required.** List of crosses.`EURGBP;EURUSD` |
| type | String | **Required.** List the crosses for all the rates wished. `real-time` |
| date | Date | For historical rates. `YYYY-MM-DD` |

#### <a id="submit-quote"></a> Retrieve Quote ####

```
Method: GET
URL: /quote
```
The Retrieve Quote service is a read-only service permitting to ask for the real-time rate before to execute a trade. 
*Caution:* It is not possible to trade with the Retrieve Quote service, you have to utilize the [Trade Service](#submit-trade) in order to placing new trades.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| currency_source | String | **Required.** Id of the destination account. `xxx` |
| currency_target | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

#### <a id="submit-trade"></a> Execute Trade ####

```
Method: POST
URL: /trade
```
This services permits to execute trade.
As a response, you will receive the details of the [Trade](#trade_object) executed.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| currency_source | String | **Required.** Id of the destination account. `xxx` |
| currency_target | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

#### <a id="get-trade"></a> Retrieve Trade ####

```
Method: GET
URL: /trade/{trade_id}
```

#### <a id="get-trades"></a> Retrieve Trades Book ####

```
Method: GET
URL: /trades
```

### API objects ###

#### <a id="account_object"></a> Account Object ####

When an account is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| account_id |  String | The id of the account. `xxx` |
| created_date |  Date Time | The creation date of the object: `2014-01-12T00:00:00+00:00` |
| created_by |  String | The creation date of the object: `api` |
| currency | String | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `USD` |
| tag |  String | Custom data. `reference` |
| status |  String | Status of the account `active` |
| type |  String | type of account `wallet` |
| number | String | Iban or account number. `xxx384` |
| Correspondent Bank | [Correspondent Bank Object](#correspondent_bank_object) | **Required for local format.** The intermediary bank details, used to reach the beneficiary bank. |
| Beneficiary Bank | [Beneficiary Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| Beneficiary | [Beneficiary Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |

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
        "beneficiary_bank":{beneficiary_bank}
        "beneficiary":{beneficiary}
    }
}
```

#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with four fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| street | String | `350 Avenue Louise` |
| post_code | String | `1050` |
| city | String | `Bruxelles` |
| state_or_province | String | `Bruxelles-Capitale` |
| country | String | `BE` |

*Example Address Object:*

```js
{
    "address": {
      "street": "4 NEW YORK PLAZA, FLOOR 15",
      "post_code": "10004",
      "city": "NEW YORK",
      "state_or_province": "NY",
      "country": "US"
    }
}
```

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

#### <a id="balance_object"></a> Balance Object ####

When the balance is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| closing_date | Date | The losing date of the balance details given. `2014-01-12` |
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

#### <a id="beneficiary_bank_object"></a> Beneficiary Bank Object ####

When a beneficiary bank is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| bic | String | **Required if swift format.** Eight or eleve-digit [ISO 9362 Business Identifier Code](http://en.wikipedia.org/wiki/ISO_9362) specifying the Recipient Bank. `CHASUS33XXX` |
| clearing_type | String | **Required if local format.** Two-digit code specifying the local clearing network. `FW` |
| clearing_number | String | **Required if local format.** The branch number on the local clearing network `021000021` |
| name | String | **Required if local format.** The beneficiary bank name. `JPMORGAN CHASE BANK, N.A.` |
| address | [Address Object](#address_object) | **Required if local format.** The beneficiary bank address. |

*Example Beneficiary Bank Object:*

```js
{
  "bic": "CHASUS33",
  "clearing_type": "FW",
  "clearing_code": "021000021",
  "name": "JPMORGAN CHASE BANK, N.A.",
  "address": {address}
}
```

#### <a id="beneficiary_object"></a> Beneficiary Object ####

When the beneficiary of an account is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| name | String | **Required.** The name of the account owner. `John Doe`|
| type | String | **Required.** The type of account owner. `individual` |
| address | [Address Object](#address_object) | The account owner address. |

*Example Beneficiary Object:*

```js
{
  "name": "John Doe",
  "type": "individual",
  "address": {address}
}
```

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
        "account_id": {amount},
    },
```
#### <a id="trade_object"></a> Trade Object ####

When a `trade` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| trade_id | String | **Required.** id of trade `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount of the trade. `10,000.00 GBP` |
| execution_date | Date | `YYYY-MM-DD` |
| created_date | Date | `YYYY-MM-DD` |

*Example Trade Object:*

```js
},
```

#### <a id="trades_object"></a> Trades Object ####

When a `trades` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| trade_id | String | **Required.** id of trade `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount of the trade. `10,000.00 GBP` |
| execution_date | Date | `YYYY-MM-DD` |
| created_date | Date | `YYYY-MM-DD` |

*Example Trades Object:*

```js
},
```

#### <a id="transfer_object"></a> Transfer Object ####

When a transfer is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| payment_id | String | **Required.** id of the beneficiary account. `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount to be transfered. `10,000.00 GBP` |
| account_source | [Account Object](#account_object) | Details of the initiating account. |
| account_target | [Account Object](#account_object) | Details of the destination account. |
| booking_date | Date | `YYYY-MM-DD` |
| value_date | Date | `YYYY-MM-DD` |
| communication | String | Communication in the free format field of the transfer. `Invoice XXX` |

#### <a id="transfers_object"></a> Transfers Object ####

When a `transfers` is specified as part of a JSON body, it is encoded as an object with the following fields:

*Object resources:*

| Field | Type | Description |
|-------|------|-------------|
| transfers_id | String | **Required.** id of transfers `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** The nominal amount of the trade. `10,000.00 GBP` |
| execution_date | Date | `YYYY-MM-DD` |
| created_date | Date | `YYYY-MM-DD` |

*Example Transfers Object:*

```js
},
```

#### <a id="rate_object"></a> Rate Object ####

When a rate is specified as part of a JSON body, it is encoded as an object with four fields:

| Field | Type | Description |
|-------|------|-------------|
| mid_market | String (Quoted decimal) | The average rate of the market between the bid and the ask rate |
| core | String (Quoted decimal) | The interbank rate provided by the FX partner of FX4BIZ |
| applied | String (Quoted decimal) | The rate applied by FX4Biz for this transaction |
| currency_pair | String | The cross of currency used for the rates provided |

Example Rate Object:

```js
"rate":{
  "mid_market": "1.1005",
  "core": "1.1004",
  "applied": "1.1002",
  "currency_pair": "EURUSD",
}
```

#### <a id="rates_object"></a> Rates Object ####

When a rates is specified as part of a JSON body, it is encoded as an array of rates objects with he following fields:

| Field | Type | Description |
|-------|------|-------------|
| rate | [Rate Object](#rate_object) | |
| instruments | Array | **Required.** List of crosses.`EURGBP;EURUSD` |
| type | String | **Required.** List the crosses for all the rates wished. `real-time` |
| date | Date | For historical rates. `YYYY-MM-DD` |

Example Rates Object:

```js
    "rates":[{rate_object}]
```
#### <a id="quote_object"></a> Quote Object ####

When a `quote` is specified as part of a JSON body, it is encoded as an object with four fields:

| Field | Type | Description |
|-------|------|-------------|

Example Quote Object:

```js
{
}
```

### Formatting Conventions ###

The `FX4BIZ-rest` API conforms to the following general behavior for [RESTful API](http://en.wikipedia.org/wiki/Representational_state_transfer):

* You make HTTP (or HTTPS) requests to the API endpoint, indicating the desired resources within the URL itself. (The public server, for the sake of security, only accepts HTTPS requests.)
* The HTTP method identifies what you are trying to do.  Generally, HTTP `GET` requests are used to retrieve information, while HTTP `POST` requests are used to make changes or submit information.
* If more complicated information needs to be sent, it will be included as JSON-formatted data within the body of the HTTP POST request.
  * This means that you must set `Content-Type: application/json` in the headers when sending POST requests with a body.
* Upon successful completion, the server returns an [HTTP status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) of 200 OK, and a `Content-Type` value of `application/json`.  The body of the response will be a JSON-formatted object containing the information returned by the endpoint.

As an additional convention, all responses from FX4Biz-REST contain a `"success"` field with a boolean value indicating whether or not the success

#### <a id="cut_off_times"></a> Cut-Off Times ####

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

#### <a id="errors_conventions"></a> Errors ####

FX4BIZ uses conventional HTTP response codes to indicate success or failure of an PAI resuest. The body of the response contains more detailed information on the cause of the problem.

In general, the HTTP status code is indicative of where the problem occurred:

* Codes in the 200-299 range indicate success. 
    * Unless otherwise specified, methods are expected to return `200 OK` on success.
* Codes in the 400-499 range indicate that the request was invalid or incorrect somehow (e.g. a required parameter was missing, a trade failed, etc.). For example:
    * `400 Bad Request` occurs if the JSON body is malformed. This includes syntax errors as well as when invalid or mutually-exclusive options are selected.
    * `404 Not Found` occurs if the path specified does not exist, or does not support that method (for example, trying to POST to a URL that only serves GET requests)
* Codes in the 500-599 range indicate that the server experienced a problem. This could be due to a network outage or a bug in the software somewhere. For example:
    * `500 Internal Server Error` occurs when the server does not catch an error. This is always a bug. If you can reproduce the error, file it at [our bug tracker in your FX4Biz platform](https://fx4bizplatform.com/login).
    * `502 Bad Gateway` occurs if FX4Biz-REST could not contact its `FX4Biz` server at all.
    * `504 Gateway Timeout` occurs if the `FX4Biz` server took too long to respond to the FX4Biz-REST server.

When possible, the server provides a JSON response body with more information about the error. These responses contain the following fields:

| Field | Type | Description |
|-------|------|-------------|
| success | Boolean | `false` indicates that an error occurred. |
| error_type | String | A short code identifying a general category for the error that occurred. |
| error | String | A human-readable summary of the error that occurred. |
| message | String | (May be omitted) A longer human-readable explanation for the error. |

*Example error:*

```js
{
    "success": false,
    "error_type": "invalid_id",
    "error": "Invalid parameter: account_id",
    "message": "Your payment must have a counterparty"
}
```

**Handling errors**

Our API libraries can raise exceptions for many reasons, such as failed trade, invalid parameters, authentications errors, and network unavailability. We recommend always trying to gracefully handle exceptions from our API.

## <a id="pagination"></a> Pagination ##

All top-level FX4BIZ API resources have support for bulk fetches - "list" API methods. For instance you can [list accounts`](#get-account-list), [list transfers`](#get-transfers-list), etc... These list API methods share a common structure.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| page | String | index of the page (start to 1) Default value: `1` |
| per_page | String | number of items returned. Default value: `10` Max: `100` |

#### <a id="quoted_numbers"></a> Quoted Numbers ####

In any case where a large number should be specified, FX4Biz-REST uses a string instead of the native JSON number type. This avoids problems with JSON libraries which might automatically convert numbers into native types with differing range and precision.

You should parse these numbers into a numeric data type with adequate precision. If it is not clear how much precision you need, we recommend using an arbitrary-precision data type.

#### <a id="versioning"></a> Versioning ####

When we make backwards-incompatible changes to the API, we realease new dated versions.
