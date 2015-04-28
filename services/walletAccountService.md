# WALLET ACCOUNT SERVICE #

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /wallets`](#get-wallet-list) | Retrieve wallet list |
| [`GET /wallets/{id}`](#get-wallet-details) | Retrieve wallet details |
| [`GET /wallets/{id}/balance/{date}`](#get-wallet-balance-from-date) | Retrieve wallet balance for a given date |

## Details ##

#### <a id="get-wallet-list"></a> Retrieve wallet list ####

```
Method: GET 
URL: /wallets
```
With the FX4BIZ API, you can list all the wallet accounts that you hold. 

**Parameters:**

This request is applicable for the [pagination format](../conventions/formatingConventions.md#pagination).

**Returns:** `Array["wallet_]`

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | The id of the wallet. |
| tag | String | The wording of the wallet. |
| currency | String | The currency of the wallet. |
| bookingAmount | Float | The booking amount (`e.g. the total money in the wallet, minus the immobilizations`) in the wallet. |
| valueAmount | Float | The total amount avalable in the wallet. |
| DateLastFinancialMovement | Date | The date of the last financial move with this wallet. |

**Example:**
```
/wallets/
```

<hr />

#### <a id="get-wallet-details"></a> Retrieve wallet details ####

```
Method: GET 
URL: /wallets/{id}
```
This request allows you to see the details related to an wallets, to confirm display the beneficiary information in your application for example.  

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required.** The ID of the external bank account you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet requested. |

**Example:**
```
/wallets/2041
```

<hr />

#### <a id="get-wallet-balance-from-date"></a> Retrieve wallet balance for a given date ####

```
Method: GET 
URL: /wallets/{id}/balance/{date}
```
This request allows you to see the details of a wallet balance at a given date. 

**Parameters:**  

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required.** The ID of the external bank account you want. |
| date | Date | **Required.** The date to search the balance of the wallet. <br />This date should be in the format `YYYY-MM-DD` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | The id of the wallet. |
| balance | [Balance Object](../objects/objects.md#balance_object) | An object representing the balance of the account. |

**Example:**
```
/wallets/2041/balance/2015-04-30
```

<hr />
