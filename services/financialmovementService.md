# FINANCIAL MOVEMENT SERVICE # 

The FX4BIZ Rest API allows you to get all financial movements from your [Wallets](./walletAccountService.md).

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /financialmovements/`](#cget_financialmovements) | Retrieve Financial Movements History |
| [`GET /financialmovements/-{id}`](#get_financialmovements) | Retrieve Financial Movements Details |

## Details ##

#### <a id="cget_financialmovements"></a> Retrieve financial movements history ####

```
Method: GET 
URL: /financialmovements/
```
Request the list of financial movements that has been received or sent on a specific period of time.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| walletId | [ID](../conventions/formattingConventions.md#type_id) | Optional | A wallet ID to specify to retreive only financial movements concerning this wallet. | 
| fromDate | [Date](../conventions/formattingConventions.md#type_date) | Optional | The starting date to search financial movements on your wallets. |
| toDate |  [Date](../conventions/formattingConventions.md#type_date) | Optional | The ending date to search financial movements on your wallets. | 

[Pagination format](../conventions/formattingConventions.md#pagination) can be applied on this request.

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| financialMovements | Array[Object] | An Array of objects representing financial movements. |
| Object.id | [ID](../conventions/formattingConventions.md#type_id) | The [ID](../conventions/formattingConventions.md#type_id) refering the financial movement. |
| Object.bookingDate | [DateTime](../conventions/formattingConventions.md#type_datetime) | The booking [DateTime](../conventions/formattingConventions.md#type_datetime) of the financial movement. |
| Object.walletId | [ID](../conventions/formattingConventions.md#type_id) | The wallet ID on which the financial movement is. |
| Object.valueDate | [DateTime](../conventions/formattingConventions.md#type_datetime) | The value [DateTime](../conventions/formattingConventions.md#type_datetime) of the financial movement. |
| Object.amount | [Amount Object](../objects/objects.md#amount_object) | An object reprsenting the amount concerned by the financial movement. |

**Example:**
```js
GET /financialmovements/?walletId=NT3bEa&fromDate=2010-01-01&toDate=2015-04-30&per_page=10&page=1
```

<hr />

#### <a id="get_financialmovements"></a> Retrieve financial movements details ####

```
Method: GET 
URL: /financialmovements/-{id}
```
Request information on a particular financial movement that has been credited or debited to a wallet. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The [ID](../conventions/formattingConventions.md#type_id) referring the financial movement. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| financialMovement | [Financial Movement Object](../objects/objects.md#financial_movement_object) | An object describing the requested financial movement. |

**Example:**
```js
GET /financialmovements/-TD4eIf
```

