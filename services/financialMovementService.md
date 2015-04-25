# FINANCIAL MOVEMENT SERVICE #

## Route ##

| Route | Description |
|-------|-------------|
| [`GET /financialmovement`](#get-transfers-list) | Retrieve Financial Movements History |
| [`GET /financialmovement/{id}`](#get-transfer-details) | Retrieve Financial Movements Details |

## Details ##

#### <a id="get-transfers-list"></a> Get Financial Movements History ####

```
Method: GET 
URL: /financialmovements
```
Request the list of financial movements that has been received or sent on a specific period of time.

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| fromDate | Date | List all financial movements that has been credited or debited on your wallet accounts since this `bookingDate`. `YYYY-MM-DD` |
| toDate | Date | List all financial movements that has been credited or debited on your wallet accounts until this `bookingDate`. `YYYY-MM-DD` | 

As a response to this query, you will receive an Array containing the `transferId`, `bookingDate`, `valueDate` and the [Amount](../objects/objects.md#amount_object) for all transfers that has been credited or debited on `bookingDate`.

<hr />

#### <a id="get-transfer-details"></a> Retrieve financial movements details ####

```
Method: GET 
URL: /financialmovements/{id}
```
Request information on a particular financial movement that has been credited or debited to a wallet. 

As a response to this query, you will receive the details of the [Financial Movement Object](../objects/objects.md#financial_movement_object).
