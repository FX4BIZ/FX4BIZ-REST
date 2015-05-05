# FX4BIZ-REST API #

The FX4BIZ-REST API provides a simplified, easy-to-use interface to the FX4BIZ accounts & operations via a RESTful API. This page explains how to use the API to execute FX trades, send cross-boarder payments and monitoring accounts with FX4BIZ.

We recommend FX4BIZ-REST for financial institutions just getting started with FX4BIZ, since it provides high-level abstractions and convenient simplifications in the data format. 

## GETTING STARTED WITH FX4BIZ-REST API ##

#### 1. Create your account with FX4BIZ ####

Please contact our institutional team in order to open an API account.
* Maxime Champoux @ mch@fx4biz.com

*Caution: We need to get all your required legal docs in order to create your new account.*

#### 2. Get your test environment ####

Please contact our IT Team in order to have access to our Test environnement.
* Laurent Maluski @ lma@fx4biz.com

#### 3. Go live ! ####

Once the setup has been validated from both sides, you will get your production environnement credentials.

## API Reference ##

The FX4BIZ API is organized around [REST](http://en.wikipedia.org/wiki/Representational_state_transfer). Our API is designed to have predictable, resource-oriented URLs and use the HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support [cross-origin resource sharing](http://en.wikipedia.org/wiki/Representational_state_transfer) to allow you to interact securely with our API from a client-side web application. [JSON](http://www.json.org/) will be returned in all responses from the API, including errors.

## Quick notes before starting ##

#### Authentication ####

As our API is designed to be secured, we highly recommend you to consult our [Authentication service](./services/authenticationService.md) in order to get your application working properly with our services.

#### Time ####

As a part of the authentication process is based on time, your systems should be synchronised through [Network Time Protocol](http://en.wikipedia.org/wiki/Network_Time_Protocol) to ensure the capability of reaching our services. Our API engine has a tolerance of 2 seconds gap to ensure that all systems are capable to reach the API, whever the [Time Server](http://en.wikipedia.org/wiki/Time_server) you uses. Using our API with non-synchronized system will result in various errors in response to your query.

## Available API Routes ##

Our API is divided into sections based on different concepts in our system. Each section is made up of a series of calls.

#### [External Bank Accounts](./services/externalbankaccountService.md) ####

* [Submit new external bank account - `POST /externalbankaccounts`](./services/externalbankaccountService.md#post_externalbankaccounts) 
* [Retrieve external bank accounts list - `GET /externalbankaccounts`](./services/externalbankaccountService.md#cget_externalbankaccounts) 
* [Retrieve external bank account details - `GET /externalbankaccounts/{id}`](./services/externalbankaccountService.md#get_externalbankaccounts) 
* [Delete external bank account - `DELETE /externalbankaccounts/{id}`](./services/externalbankaccountService.md#delete_externalbankaccounts) 

#### [Wallet Accounts](./services/walletService.md) ####

* [Retrieve wallet list - `GET /wallets`](./services/walletService.md#cget_wallets)
* [Retrieve wallet details - `GET /wallets/{id}`](./services/walletService.md#get-wallets)
* [Retrieve wallet balance for a given date - `GET /wallets/{id}/balance/{date}`](./services/walletService.md#get_wallets_balance)

#### [Payments](./services/paymentService.md) ####

* [Submit Payment - `POST /payment`](./services/paymentService.md#submit-payment)
* [Confirm Payment - `PUT /payment/{payment_id}/confirm`](./services/paymentService.md#confirm-payment)
* [Retrieve Payments History - `GET /payments`](./services/paymentService.md#cget_payments)
* [Retrieve Payment Details - `GET /payment/{payment_id}`](./services/paymentService.md#get_payments)
* [Update Payment Details - `PUT /payment/{payment_id}`](./services/paymentService.md#put-payment-details)
* [Cancel Payment  - `DELETE /payment/{payment_id}`](./services/paymentService.md#delete-payment)

#### [Financial Movements](./services/financialmovementService.md) ####

* [Retrieve Financial Movements History - `GET /financialmovements`](./services/financialmovementService.md#cget_financialmovement)
* [Retrieve Financial Movements Details - `GET /financialmovements/{id}`](./services/financialmovementService.md#get_financialmovement)

#### [Trades](./services/tradeService.md) ####

* [Retrieve Rates - `GET /rates`](./services/tradesService.md#get_rates)
* [Request Quote - `POST /quote`](./services/tradesService.md#get-quote)
* [Submit Trade - `POST /trade`](./services/tradesService.md#get-trade)
* [Cancel Trade - `DELETE /trade/{trade_id}`](./services/tradesService.md#cancel-trade)
* [Retrieve Trades Book - `GET /trades`](./services/tradesService.md#get-trade-book)
* [Retrieve Trade Details - `GET /trade/{trade_id}`](./services/tradesService.md#get-trade-details)

#### [Logs](./services/logService.md) ####

* [Retrieve Logs - `GET /logs`](./services/logService.md#get_logs) 
* [Retrieve a log entry with a nonce  - `GET /logs/{nonce}`](./services/logService.md#get_log) 

## [API Objects](./objects/objects.md) ##

* [External Bank Account Object](./objects/objects.md#account_object)
* [Address Object](./objects/objects.md#address_object)
* [Balance Object](./objects/objects.md#balance_object)
* [Holder Bank Object](./objects/objects.md#beneficiary_bank_object)
* [Holder Object](./objects/objects.md#beneficiary_object)
* [Correspondent Bank Object](./objects/objects.md#correspondent_bank_object)
* [Payment Object](./objects/objects.md#payment_object)
* [Rate Object](./objects/objects.md#rate_object)
* [Trade Object](./objects/objects.md#trade_object)
* [Transfer Object](./objects/objects.md#transfer_object)
* [Quote Object](./objects/objects.md#trade_object)
* [Log Object](./objects/objects.md#log_object)

## [Formatting Conventions](./conventions/formatingConventions.md) ##

* [Cut-Off Times](./conventions/formatingConventions.md#cut_off_times)
* [Errors](./conventions/formatingConventions.md#errors_conventions)
* [Pagination](./conventions/formatingConventions.md#pagination)
* [Anonymous objects](./conventions/formatingConventions.md#anonymous_object)
* [Versioning](./conventions/formatingConventions.md#versioning)

## LICENCE ##

TBD