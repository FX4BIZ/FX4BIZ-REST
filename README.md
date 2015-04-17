# FX4BIZ-REST API #

The FX4BIZ-REST API provides a simplified, easy-to-use interface to the FX4BIZ accounts & operations via a RESTful API. This page explains how to use the API to execute FX trades, send cross-boarder payments and monitoring accounts with FX4BIZ.

We recommend FX4BIZ-REST for financial institutions just getting started with FX4BIZ, since it provides high-level abstractions and convenient simplifications in the data format. 

## Available API Routes ##

Our API is divided into sections based on different concepts in our system. Each section is made up of a series of calls.

#### External Bank Accounts ####

* [Submit new external bank account - `POST /externalbankaccounts`](#post-account-create) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)
* [Retrieve external bank accounts list - `GET /externalbankaccounts`](#get-accounts-list) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)
* [Retrieve external bank account details - `GET /externalbankaccounts/{id}`](#get-account-details) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)
* [Delete external bank account - `DELETE /externalbankaccounts/{id}`](#delete-account) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)

<hr />

#### Wallet Accounts ####

* [Submit new wallet account - `POST /account`](#post-account-create)
* [Retrieve wallet accounts list - `GET /accounts`](#get-accounts-list)
* [Retrieve wallet account details - `GET /account/{account_id}`](#get-account-details)
* [Update wallet account details - `PUT /account/{account_id}`](#put-account-details)
* [Delete wallet account - `DELETE /account/{account_id}`](#delete-account)

#### Payments ####

* [Submit Payment - `POST /payment`](#submit-payment)
* [Confirm Payment - `PUT /payment/{payment_id}/confirm`](#confirm-payment)
* [Retrieve Payments History - `GET /payments`](#get-payment-history)
* [Retrieve Payment Details - `GET /payment/{payment_id}`](#get-payment-history)
* [Update Payment Details - `PUT /payment/{payment_id}`](#put-payment-details)
* [Cancel Payment  - `DELETE /payment/{payment_id}`](#delete-payment)

#### Financial Movements ####

* [Retrieve Financial Movements History - `GET /financialmovement`](#get-transfers-list)
* [Retrieve Financial Movements Details - `GET /financialmovement/{id}`](#get-transfer-details)

#### Trades ####

* [Retrieve Rates - `GET /rates`](#get-rates) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)
* [Request Quote - `POST /quote`](#get-quote)
* [Submit Trade - `POST /trade`](#get-trade)
* [Cancel Trade - `DELETE /trade/{trade_id}`](#cancel-trade)
* [Retrieve Trades Book - `GET /trades`](#get-trade-book)
* [Retrieve Trade Details - `GET /trade/{trade_id}`](#get-trade-details)

#### Logs ####

* [Retrieve Logs - `GET /logs`](#get-logs) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)
* [Retrieve a log entry with a nonce  - `GET /logs/{nonce}`](#get-log-by-nonce) !["alt"](http://png-3.findicons.com/files/icons/1689/splashy/16/check.png)

## API Reference ##

The FX4BIZ API is organized around [REST](http://en.wikipedia.org/wiki/Representational_state_transfer). Our API is designed to have predictable, resource-oriented URLs and use the HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support [cross-origin resource sharing](http://en.wikipedia.org/wiki/Representational_state_transfer) to allow you to interact securely with our API from a client-side web application. [JSON](http://www.json.org/) will be returned in all responses from the API, including errors.

#### Objects List ####

* [External Bank Account Object](#account_object)
* [Address Object](#address_object)
* [Balance Object](#balance_object)
* [Holder Bank Object](#beneficiary_bank_object)
* [Holder Object](#beneficiary_object)
* [Correspondent Bank Object](#correspondent_bank_object)
* [Payment Object](#payment_object)
* [Rate Object](#rate_object)
* [Trade Object](#trade_object)
* [Transfer Object](#transfer_object)
* [Quote Object](#trade_object)
* [Log Object](#log_object)

#### Formatting Conventions ####

* [Cut-Off Times](#cut_off_times)
* [Errors](#errors_conventions)
* [Pagination](#pagination)
* [Quoted numbers](#quoted_numbers)
* [Versioning](#versioning)

## API Services ##

### Authentication Services ###

Every call to the FX4BIZ API must be authenticated, which must be done by adding a custom HTTP header (X-WSSE) with your username and secret.  
This section contains detailed instructions on how to create valid headers for the Suite REST API.

**All API request must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTPS). Calls made over plain [HTTP](http://en.wikipedia.org/wiki/HTTP) will fail.  
You must authenticate for all requests.**

#### X-WSSE Format ####

The header has the following format, usually a single [HTTP](http://en.wikipedia.org/wiki/HTTP) header line which we broke down into multiple lines for easier readability:

```js
X-WSSE: UsernameToken
Username="customer001",
PasswordDigest="ZmI2ZmQ0MDIxYmQwNjcxNDkxY2RjNDNiMWExNjFkZA==",
Nonce="d36e3162829ed4c89851497a717f",
Created="2014-03-20T12:51:45Z"
```

The following sections describe each component in detail:

**X-WSSE :** Name of the [HTTP](http://en.wikipedia.org/wiki/HTTP) header we use for authenticating the request.  
**UsernameToken :** Authentication method. The X-WSSE header must contain a UsernameToken as we only support token-based authentication.  
**Username :** Field containing the username you were provided during onboarding.  
**PasswordDigest :** Field containing the hashed token which will prove the authenticity of your request. It is essential that you recompute this hash for every request as a hash is only valid for a certain period of time, and then it expires.  
**Nonce :** A random value used to make your request unique so it cannot be replicated by any other unknown party.  
**Created :** Field containing the current UTC, GMT, ZULU timestamp (YYYY-MM-DDTHH:MM:SS) according to the [ISO8601](http://fr.wikipedia.org/wiki/ISO_8601) format, *e.g. 2014-03-20T12:51:45+01:00*.

####Computing the Password Digest####

Computing the password digest involves 5 simple steps:

1. Get a randomly generated 16 byte Nonce formatted as 32 hexadecimal characters.
2. Get the current Created timestamp in [ISO-8601](http://fr.wikipedia.org/wiki/ISO_8601) format.
3. Concatenate the following three values in this order: nonce, timestamp, secret.
4. Calculate the [SHA-1](http://fr.wikipedia.org/wiki/SHA-1) hash value of the concatenated string, and make sure this value is in hexadecimal format! Some languages, like PHP, output hexadecimal hash by default. You may need to use special methods to obtain hexadecimal hashes in different languages or even convert byte to hex values by hand (see the sample codes below for more information).
5. Apply a [Base64](http://fr.wikipedia.org/wiki/Base64) encoding to the resulted hash to get the final PasswordDigest value.

####Exemple of creating the X-WSSE header with PHP####
Here is a sample code that you can use to make your X-WSSE header, or just in a way of understanding the process.

```php
<?php

// Login informations
$username = "YourUserName" ;			// The username used to authenticate
$password = "secret" ;					// The password used to authenticate
$nonce = "" ;							// The nonce
$nonce64 = "" ;							// The nonce with a Base64 encoding
$date = "" ;							// The date of the request, in  ISO 8601 format
$digest = "" ;							// The password digest needed to authenticate you
$header = "" ; 							// The final header to put in your request

// Making the nonce and the encoded nonce
$chars = "0123456789abcdef";
for ($i = 0; $i < 32; $i++) {
    $nonce .= $chars[rand(0, 15)];
}
$nonce64 = base64_encode($nonce) ;

// Getting the date at the right format (e.g. YYYY-MM-DDTHH:MM:SSZ)
$date = gmdate('c');
$date = substr($ts,0,19)."Z" ;

// Getting the password digest
$digest = base64_encode(sha1($nonce.$ts.$password, true));

// Getting the X-WSSE header to put in your request
$header = sprintf('X-WSSE: UsernameToken Username="%s", PasswordDigest="%s", Nonce="%s", Created="%s"',$username, $digest, $nonce64, $ts);

```


### <a id="account_services"></a> External Bank Account Services ###

In the FX4BIZ API, what we call `external bank` account, can be either your own account in another bank or a third party recipient account.

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
        "correspondantBank":{
            "bic": "AGRIFRPP",
            "name": "CREDIT AGRICOLE SA",
            "address": {
                "street": "BUILDING PASTEUR, BLOC 1: 91-93, BOULEVARD PASTEUR",
                "postCode": "75015",
                "city": "Paris",
                "country": "FRANCE"
            }
        },
        "holderBank": {
            "bic": "FXBBBEBB",
            "name": "FX4BIZ SA",
            "address": {
                "street": "Avenue Louise, 350",
                "postCode": "1050",
                "city": "Bruxelles",
                "country": "FR"
            }
        },
        "holder": {
            "name": "John Doe",
            "type": "individual",
            "address": {
                "street": "1 My Road",
                "postCode": "ZIP",
                "city": "London",
                "country": "UK",
            }
        }
        
    }
}
```

#### <a id="post-account-create"></a> Submit a new external bank account ####

```
Method: POST 
URL: /externalbankaccounts
```
By submitting a new external bankaccount, supply the relevant details in order to pay a beneficiary.

*Caution.* All your own `wallet` accounts are created automatically when subscribing with FX4BIZ.

The Submit external bank account service allows to reference `external bank` accounts which are your own accounts or a third party account hold in another bank.
This service include verifications on the format of the account created.
The API has been made in order to accept local specification of cross-boarder payments.

The Api accepts the following formats of `external bank` accounts :
-	Austrian Bankleitzahl
-	Australian Bank State Branch
-	German Bankleitzahl
-	Canadian Payments Association Payment Routing Number
-	Spanish Domestic Interbanking Code
-	Fedwire Routing Number
-	HEBIC (Hellenic Bank Identification Code)
-	Bank Code of Hong Kong
-	Irish National Clearing Code (NSC)
-	Indian Financial System Code (IFSC)
-	Italian Domestic Identification Code
-	New Zealand National Clearing Code
-	Polish National Clearing Code (KNR)
-	Portuguese National Clearing Code
-	Russian Central Bank Identification Code
-	UK Domestic Sort Code
-	Swiss Clearing Code
-	South African National Clearing Code

Of course, it is possible to reference third party `wallet` accounts and pay as a beneficiary.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| Number | String | **Required.** The recipient account number or Iban. `xxx4548` |
| Currency | String | **Required.** Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `EUR` |
| Tag | String | Custom Data. `John Doe bank account EUR` |
| CorrespondentBic | String | The intermediary bank BIC code. |
| holderBank | [Holder Bank Object](#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| holder | [Holder Object](#beneficiary_object) | **Required.** The recipient details, owner of the account. |

As a response to this query, you will receive a json response containing details of the [External Bank Account](#account_object) created. The unique AccountId must be stored and used in your future payments.

#### <a id="get-accounts-list"></a> Retrieve external bank accounts list ####

```
Method: GET 
URL: /externalbankaccounts
```
With the FX4BIZ API, you can list all the external bank accounts hold by the person or compagny of a certain user.  
The user is not to be passed as a parameter since it's the one you use to authenticate that will be used.

*Example:*
```
/externalbankaccounts/
```

As a response to this query, you will receive an Array containing the [External Bank Account Object](#account_object) for each external account.

#### <a id="get-account-details"></a> Retrieve external bank account details ####

```
Method: GET 
URL: /externalbankaccount/{id}
```
This request allows you to see the details related to an account, to confirm display the beneficiary information in your application for example.  

*Parameters*  

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required.** The ID of the external bank account you want. <br :>As ID's are listed with the [Account Object](#account_object), You can retrive this by listing all external bank accounts for the current user. |

*Example:*
```
/externalbankaccounts/2041
```
As a response to this query, you will receive the details of the [External Bank Account](#account_object).

#### <a id="delete-account"></a> Delete external bank account ####

```
Method: DELETE 
URL: /externalbankaccount/{id}
```
Delete an account.

As a response to this query, you will receive a HTTP 200 if the deleting was successfull, or an error if it wasn't.

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

#### <a id="submit-payment"></a> Submit a payment ####

```
Method: POST 
URL: /payment
```
Use this path in order to schedule a new payment. 

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| account_id | String | **Required.** Id of the destination account. `xxx` |
| amount | [Amount Object](#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response to this query, you will receive the details of the [Payment](#payment_object) created.

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

As a response to this query, you will receive an Array with the `payment_id`, `creation_date`, `execution_date` and `amount`  of the [Payment] of all payment created.

#### <a id="get-payment-details"></a> Retrieve Payment Details ####

```
Method: GET
URL: /payment
```
Retrieve the details of a specific payment.

As a response to this query, you will receive the details of the [Payment](#payment_object).

#### <a id="put-payment"></a> Update Payment Details####

```
Method: PUT
URL: /payment/{payment_id}
```
Update information on a specific payment.

As a response to this query, you will receive the updated details of the [Payment](#payment_object).

#### <a id="delete-payment"></a> Cancel Payment ####

```
Method: DELETE
URL: /payment/{payment_id}
```

As a response to this query, you will receive a confirmation that the [Payment](#payment_object) is deleted properly.

### Financial Movements Services ###

#### <a id="get-transfers-list"></a> Get financial movements history ####

```
Method: GET 
URL: /financialmovements
```
Request the list of financial movements that has been received or sent on a specific period of time.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| from_date | Date | List all transfers that has been credited or debited on your wallets account since this `booking_date`. `YYYY-MM-DD` |
| to_date | Date | List all transfers that has been credited or debited on your wallets account until this `booking_date`. `YYYY-MM-DD` | 

As a response to this query, you will receive an Array containing the `transfer_id`, `booking_date`, `value_date` and the [Amount](#amount_object) for all transfers that has been credited or debited on `booking date`.

#### <a id="get-transfer-details"></a> Retrieve financial movements details ####

```
Method: GET 
URL: /financial movements/{id}
```
Request information on a particular financial movement that has been credited or debited to a wallet. 

As a response to this query, you will receive the details of the [Financial Movement Object](#transfer_object).

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

#### <a id="get-rates"></a> Retrieve Rates ####

```
Method: 	GET
URL: 		/rates
```
The FX4BIZ-REST API provides a FX Data Feed. You can use the [Rates service](#rate_object) in order to ask for currency rates tables. Spreads showed in this service are minimal spreads, the tradable spread can be higher depending on the volatility of the market.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| instruments | String | **Required.** A string representing a list of crosses. Crosses must be separated with commas. <br />You can chain as many crosses as you want, as long as they're separated with commas. |

*Example:*
```
/rates?instruments=EURGBP,EURUSD
```

As a response to this query, you will receive an Array made of [Rate objects](#rate_object) for all rates asked.

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
| amount | [Amount Object](#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response to this query, you will receive the [Quote](#quote_object) required.

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
| amount | [Amount Object](#amount_object) | **Required.** Amount to be sent. `10000.00+GBP` *Caution.* The currency of the amount sent must be equal to the currency of the beneficiary account. |
| execution_date | Date | Initial execution date of you payment. `YYYY-MM-DD` |

As a response, you will receive the details of the [Trade](#trade_object) executed.

#### <a id="get-trade"></a> Retrieve Trade ####

```
Method: GET
URL: /trade/{trade_id}
```
Retrieve the trade details.

As a response, you will receive the details of the [Trade](#trade_object).

#### <a id="get-trades"></a> Retrieve Trades Book ####

```
Method: GET
URL: /trades
```
Retrieve the list of trades executed. We sort trades by validation date.

As a response, you will receive an array containing the `trade_id`, the `validation_date` and the `amount` for all trades validated.

### Log Services ###

As a developper, you might want to go further than sending request and recieve responses.  
The FX4BIZ API allows you to access the logs of its own services, to provide you some details about what you send, and what it's done on the platform.

You can retrieve informations during a certain time.  
This time passed, your log entry will be lost.

| HTTP Method | Time |
|-------------|------|
| GET | 1 hour |
| POST, PUT | 1 week |

#### <a id="get-logs"></a> Retrieve Logs ####

```
Method: 	GET
URL: 		/logs
```
The FX4BIZ-REST API provides a log feed about request you made, allowing you to know exactly what your request do on the platform.  
This request uses the login sent in your header to get logs about this user's actions.

*Parameters:*

This request is appliable for the [pagination format](#pagination).

*Example:*
```
/logs?per_page=10&page=1
```

As a response to this query, you will receive an Array made of [Log objects](#log_object) for all the log you're concerned.

#### <a id="get-log-by-nonce"></a> Retrieve a log entry with a nonce ####

```
Method: 	GET
URL: 		/logs/{nonce}
```
In case of somewhat happens during the request, the FX4BIZ API allows you to retrive a log entry by its nonce.  

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| nonce | String | **Required.** The nonce used to authenticate the request. As the one in the header, this nonce has to be [Base64](http://fr.wikipedia.org/wiki/Base64) encoded. |

*Example:*
```
/logs/MzI0ZWQ0YTA0NmJhM2VmZWZiNzYyMTkzMWQ1ZjY2M2I=
```

As a response to this query, you will receive an Array made of one [Log object](#log_object) corresponding with the nonce you sent.

### API objects ###

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
        "account_id": {account},
    },
```

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

#### <a id="transfer_object"></a> Financial Movements Object ####

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

#### <a id="quote_object"></a> Quote Object ####

When a `quote` is specified as part of a JSON body, it is encoded as an object with four fields:

| Field | Type | Description |
|-------|------|-------------|

Example Quote Object:

```js
--> TBD
```

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

#### <a id="quoted_numbers"></a> Quoted Numbers ####

In any case where a large number should be specified, FX4Biz-REST uses a string instead of the native JSON number type. This avoids problems with JSON libraries which might automatically convert numbers into native types with differing range and precision.

You should parse these numbers into a numeric data type with adequate precision. If it is not clear how much precision you need, we recommend using an arbitrary-precision data type.

#### <a id="versioning"></a> Versioning ####

When we make backwards-incompatible changes to the API, we realease new dated versions.
