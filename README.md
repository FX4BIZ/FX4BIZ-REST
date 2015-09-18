# FX4BIZ-REST API  

The FX4BIZ-REST API provides a simplified, easy-to-use interface to the FX4BIZ accounts & operations via a RESTful API. This page explains how to use the API to execute FX trades, send cross-boarder payments and monitoring accounts with FX4BIZ.

We recommend FX4BIZ-REST for financial institutions just getting started with FX4BIZ, since it provides high-level abstractions and convenient simplifications in the data format. 

## GETTING STARTED WITH FX4BIZ-REST API ##

#### 1. Create your account with FX4BIZ ####

Please contact our institutional team in order to open an API account.
* Maxime Champoux @ mch@fxforbiz.com

*Caution: We need to get all your required legal docs in order to create your new account.*

#### 2. Get your test environment ####

Please contact our IT Team in order to have access to our Test environnement.
* Laurent Maluski @ lma@fxforbiz.com
* Adrien Gras @ agr@fxforbiz.com

#### 3. Go live ! ####

Once the setup has been validated from both sides, you will get your production environnement credentials.

## API Reference ##

The FX4BIZ API is organized around [REST](http://en.wikipedia.org/wiki/Representational_state_transfer). Our API is designed to have predictable, resource-oriented URLs and use the HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients, and we support [cross-origin resource sharing](http://en.wikipedia.org/wiki/Representational_state_transfer) to allow you to interact securely with our API from a client-side web application. [JSON](http://www.json.org/) will be returned in all responses from the API, including errors.

## Quick notes before starting ##

#### Authentication ####

As our API is designed to be secured, we highly recommend you to consult our [Authentication service](./services/authenticationService.md) in order to get your application working properly with our services.

#### Time ####

As a part of the authentication process is based on time, your systems should be synchronised through [Network Time Protocol](http://en.wikipedia.org/wiki/Network_Time_Protocol) to ensure the capability of reaching our services. Our API engine has a tolerance of 2 seconds gap to ensure that all systems are capable to reach the API, whever the [Time Server](http://en.wikipedia.org/wiki/Time_server) you uses. Using our API with non-synchronized system will result in various errors in response to your query.

## Testing and troubleshooting ##

For testing requests and have a clear representattion of what's returned by our API, you may want to use our [API testing script](./conventions/testing.md). Easy to use, it will help you integrate the API more efficiently.  
Also, if you have some issues, please check our [Troubleshooting section](./conventions/troubleshooting.md) to see if your issue is referenced. If it's not, please contact our IT team.  

## Available API Routes ##

Our API is divided into sections based on different concepts in our system. Each section is made up of a series of calls.

#### [External Bank Accounts](./services/externalbankaccountService.md) ####

* [Submit new external bank account `POST /externalBankAccounts/`](./services/externalbankaccountService.md#post_externalbankaccounts)
* [Retrieve external bank accounts list `GET /externalBankAccounts/`](./services/externalbankaccountService.md#cget_externalbankaccounts)
* [Retrieve external bank account details `GET /externalBankAccounts/-{id}`](./services/externalbankaccountService.md#get_externalbankaccounts)
* [Delete an external bank account `DELETE /externalBankAccounts/-{id}`](./services/externalbankaccountService.md#delete_externalbankaccounts)

#### [Wallet Accounts](./services/walletService.md) ####

* [Submit new wallet `POST /wallets/`](./services/walletService.md#post_wallets)
* [Submit new wallet with an holder `POST /wallets/withholder`](./services/walletService.md#post_wallets_with_holder)
* [Retrieve wallets list `GET /wallets/`](./services/walletService.md#cget_wallets)
* [Retrieve wallet details `GET /wallets/-{id}`](./services/walletService.md#get_wallets)
* [Retrieve wallet balance for a given date `GET /wallets/-{id}/balance/{date}`](./services/walletService.md#get_wallets_balance)

#### [Payments](./services/paymentService.md) ####

* [Submit a Payment `POST /payments/`](./services/paymentService.md#post_payments)
* [Confirm Payment `PUT /payments/-{id}/confirm`](./services/paymentService.md#put_payments_confirm)
* [Retrieve Payments History `GET /payments/`](./services/paymentService.md#cget_payments)
* [Retrieve Payment Details `GET /payments/-{id}`](./services/paymentService.md#get_payments)
* [Cancel Payment `DELETE /payments/-{id}`](./services/paymentService.md#delete_payments)

#### [Financial Movements](./services/financialmovementService.md) ####

* [Retrieve Financial Movements History - `GET /financialmovements/`](./services/financialmovementService.md#cget_financialmovements)
* [Retrieve Financial Movements Details - `GET /financialmovements/-{id}`](./services/financialmovementService.md#get_financialmovements)

#### [Trades](./services/tradeService.md) ####

* [Retrieve Rates `GET /rates/{instruments}`](./services/tradeService.md#get_rates)
* [Request Quote `POST /quotes/`](./services/tradeService.md#post_quotes)
* [Submit Trade `POST /trades/`](./services/tradeService.md#post_trades)
* [Retrieve Trades Book `GET /trades/`](./services/tradeService.md#cget_trades)
* [Retrieve Trade Details `GET /trades/-{id}`](./services/tradeService.md#get_trades)

#### [Logs](./services/logService.md) ####

* [Retrieve Logs `GET /logs/`](./services/logService.md#get_logs) 
* [Retrieve a log entry with a nonce `GET /logs/-{nonce}`](./services/logService.md#get_log) 

## [API Objects](./objects/objects.md) ##

* [External Bank Account Object](./objects/objects.md#account_object)
* [Wallet Object](./objects/objects.md#wallet_object)
* [Address Object](./objects/objects.md#address_object)
* [Amount Object](./objects/objects.md#amount_object)
* [Balance Object](./objects/objects.md#balance_object)
* [Holder Bank Object](./objects/objects.md#beneficiary_bank_object)
* [Holder Object](./objects/objects.md#beneficiary_object)
* [Correspondent Bank Object](./objects/objects.md#correspondent_bank_object)
* [Payment Object](./objects/objects.md#payment_object)
* [Rate Object](./objects/objects.md#rate_object)
* [Trade Object](./objects/objects.md#trade_object)
* [Financial Movement Object](./objects/objects.md#financial_movement_object)
* [Quote Object](./objects/objects.md#trade_object)
* [Log Object](./objects/objects.md#log_object)
* [Process Result Object](./objects/objects.md#processresult_object)

## [Formatting Conventions](./conventions/formattingConventions.md) ##

* [Cut-Off Times](./conventions/formattingConventions.md#cut_off_times)
* [Errors](./conventions/formattingConventions.md#errors_conventions)
* [Pagination](./conventions/formattingConventions.md#pagination)
* [normalized Types](./conventions/formattingConventions.md#anonymous_object)
* [anonymous Objects](./conventions/formattingConventions.md#anonymous_object)
* [Versioning](./conventions/formattingConventions.md#versioning)


