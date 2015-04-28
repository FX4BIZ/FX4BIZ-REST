# FINANCIAL MOVEMENT SERVICE #

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /financialmovement`](#get-transfers-list) | Retrieve Financial Movements History |
| [`GET /financialmovement/{id}`](#get-transfer-details) | Retrieve Financial Movements Details |

## Details ##

#### <a id="get-transfers-list"></a> Get financial movements history ####

```
Method: GET 
URL: /financialmovements
```
Request the list of financial movements that has been received or sent on a specific period of time.

**Parameters:**

| Field | Type | Description |
|-------|------|-------------|
| fromDate | Date | List all transfers that has been credited or debited on your wallets account since this `booking_date`. `YYYY-MM-DD` |
| toDate | Date | List all transfers that has been credited or debited on your wallets account until this `booking_date`. `YYYY-MM-DD` | 

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | The id refering the financial movement. |
| bookingDate | Date | The booking date of the financial movement. |
| valueDate | Date | The value date of the financial movement. |
| amount | [Amount Object](../objects/objects.md#amount_object) | An object reprsenting the amount concerned by the financial movement. |

<hr />

#### <a id="get-transfer-details"></a> Retrieve financial movements details ####

```
Method: GET 
URL: /financial movements/{id}
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

