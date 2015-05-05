# FINANCIAL MOVEMENT SERVICE #

The FX4BIZ Rest API allows you to get all financial movements from your [Wallets](./walletAccountService.md).

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /financialmovements/`](#cget_financialmovement) | Retrieve Financial Movements History |
| [`GET /financialmovements/{id}`](#get_financialmovement) | Retrieve Financial Movements Details |

## Details ##

#### <a id="cget_financialmovement"></a> Get financial movements history ####

```
Method: GET 
URL: /financialmovements/
```
Request the list of financial movements that has been received or sent on a specific period of time.

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| number | String | **Optionnal** An account number to specify on which wallet you would retreive the financial movements. | 
| fromDate | Date | **Optionnal** A date representing the starting date to search financial movements on your wallets. |
| toDate | Date | **Optionnal** A date representing the ending date to search financial movements on your wallets. | 

This request is appliable for the [pagination format](../conventions/formatingConventions.md#pagination).

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| financialMovements | Array[Object] | An Array of objects representing financial movements. |
| Object.id | Integer | The id refering the financial movement. |
| Object.bookingDate | Date | The booking date of the financial movement. |
| Object.accountNumber | String | The account refering financial movement. |
| Object.valueDate | Date | The value date of the financial movement. |
| Object.amount | [Amount Object](../objects/objects.md#amount_object) | An object reprsenting the amount concerned by the financial movement. |

**Example:**
```
/financialmovements/?number=XXXXXXXXXXX&fromDate=2010-01-01&toDate?2015-04-30&per_page=10&page=1
```

<hr />

#### <a id="get_financialmovement"></a> Retrieve financial movements details ####

```
Method: GET 
URL: /financialmovements/{id}
```
Request information on a particular financial movement that has been credited or debited to a wallet. 

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | The id refering the financial movement. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| financialMovement | [Financial Movement Object](../objects/objects.md#financial_movement_object) | An object describing the requested financial movement. |

**Example:**
```
/financialmovements/1552
```