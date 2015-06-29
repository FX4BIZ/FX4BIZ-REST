# WALLET ACCOUNT SERVICE #

In the FX4BIZ API, what we call a `wallet` account, is a payment account in FX4BIZ, allowing you to send and receive funds.

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /wallets`](#cget_wallets) | Retrieve wallet list |
| [`GET /wallets/{id}`](#get_wallets) | Retrieve wallet details |
| [`GET /wallets/{id}/balance/{date}`](#get_wallets_balance) | Retrieve wallet balance for a given date |
| [`POST /wallets/`](#post_wallets) | Submit new wallet |
| [`POST /wallets/withholder`](#post_wallets_with_holder) | Submit new wallet with an holder |

## Details ##

#### <a id="cget_wallets"></a> Retrieve wallet list ####

```
Method: GET 
URL: /wallets/
```
With the FX4BIZ API, you can list all the wallet accounts that you hold. 

**Parameters:**

This request is applicable for the [pagination format](../conventions/formatingConventions.md#pagination).

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallets | Array[Object] | An Array of objects representing wallets. |
| Object.id | String | The ID of the wallet. |
| Object.tag | String | The wording of the wallet. |
| Object.currency | String | The currency of the wallet. |
| Object.bookingAmount | [Amount Object](../objects/objects.md#amount_object) | The booking amount (`e.g. the total money in the wallet, minus the immobilizations`) in the wallet. |
| Object.valueAmount | [Amount Object](../objects/objects.md#amount_object) | The total amount avalable in the wallet. |
| Object.dateLastFinancialMovement | String | The date of the last financial move with this wallet. |

**Example:**
```
/wallets/
```

<hr />

#### <a id="get_wallets"></a> Retrieve wallet details ####

```
Method: GET 
URL: /wallets/{id}
```
This request allows you to see the details related to an wallets, to confirm display the beneficiary information in your application for example.  

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | String | Required | The ID of the external bank account you want. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet requested. |

**Example:**
```
/wallets/2041
```

<hr />

#### <a id="get_wallets_balance"></a> Retrieve wallet balance for a given date ####

```
Method: GET 
URL: /wallets/{id}/balance/{date}
```
This request allows you to see the details of a wallet balance at a given date. 

**Parameters:**  

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | String | Required | The ID of the external bank account you want. |
| date | String | Required | The date to search the balance of the wallet. <br />This date should be in the format `YYYY-MM-DD` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | String | The ID of the wallet. |
| balance | [Balance Object](../objects/objects.md#balance_object) | An object representing the balance of the account. |

**Example:**
```
/wallets/2041/balance/2015-04-30
```

<hr />

#### <a id="post_wallets"></a> Submit a new wallet ####

```
Method: POST 
URL: /wallets/
```

This request allows you to submit a new wallet for a given currency.
**Caution :** This service is only avalable for FX4BIZ users that have suscribed to the wallet option program.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currency | String | Required | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the wallet currency. `USD` |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet you just created. |

**Example:**
```
/wallets/
```

<hr />

#### <a id="post_wallets_with_holder"></a> Submit a new wallet with an holder ####

```
Method: POST 
URL: /wallets/
```

This request allows you to submit a new wallet for a given currency and a given holder.  
**Caution :** This service is only avalable for FX4BIZ users that have suscribed to the wallet option program.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| currency | String | Required | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the wallet currency. `USD` |
| holder | [Holder Object](../objects/objects.md#beneficiary_object) | Required | The recipient details, owner of the wallet. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| wallet | [Wallet Object](../objects/objects.md#wallet_object) | An object representing the wallet you just created. |

**Example:**
```
/wallets/withholder/
```

<hr />