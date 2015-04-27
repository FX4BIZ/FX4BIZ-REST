# WALLET ACCOUNT SERVICE #

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /accounts`](#get-wallet-list) | Retrieve wallet list |
| [`GET /account/{id}`](#get-wallet-details) | Retrieve wallet details |
| [`GET /account/{id}/balance/{date}`](#get-wallet-balance-from-date) | Retrieve wallet balance for a given date |

## Details ##

#### <a id="get-wallet-list"></a> Retrieve wallet list ####

```
Method: GET 
URL: /wallets
```
With the FX4BIZ API, you can list all the wallet accounts that you hold. 

*Parameters:*

This request is applicable for the [pagination format](../conventions/formatingConventions.md#pagination).

*Example:*
```
/wallets/
```

As a response to this query, you will receive an Array containing the summary for each wallets, including :
* it's own unique ID (`id`) 
* it's wording (`tag`)
* it's currency (`currency`)
* it's actual balance as a [Balance Object](../objects/objects.md#balance_object) (`balance`)

<hr />

#### <a id="get-wallet-details"></a> Retrieve wallet details ####

```
Method: GET 
URL: /externalbankaccount/{id}
```
This request allows you to see the details related to an account, to confirm display the beneficiary information in your application for example.  

*Parameters*  

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required.** The ID of the external bank account you want. |

*Example:*
```
/wallets/2041
```
As a response to this query, you will receive the details of the [Wallet](../objects/objects.md#wallet_object).

<hr />

#### <a id="get-wallet-balance-from-date"></a> Retrieve wallet balance for a given date ####

```
Method: GET 
URL: /wallets/{id}/balance/{date}
```
This request allows you to see the details of a wallet balance at a given date. 

*Parameters*  

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required.** The ID of the external bank account you want. |
| date | Date | **Required.** The date to search the balance of the wallet. <br />This date should be in the format `YYYY-MM-DD` |

*Example:*
```
/wallets/2041/balance/2015-04-30
```
As a response to this query, you will receive the details of the [Balance](../objects/objects.md#balance_object), and the unique ID (`id`) of the wallet.

<hr />
