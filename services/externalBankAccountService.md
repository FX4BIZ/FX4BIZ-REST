# EXTERNAL BANK ACCOUNT SERVICE #

In the FX4BIZ API, what we call `external bank` account, can be either your own account in another bank or a third party recipient account.

As an example, a response for `GET /account/{account_id}/details` looks like this:
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
## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /externalbankaccounts`](#post-account-create) | Submit new external bank account |
| [`GET /externalbankaccounts`](#get-accounts-list) | Retrieve external bank accounts list |
| [`GET /externalbankaccounts/{id}`](#get-account-details) | Retrieve external bank account details |
| [`DELETE /externalbankaccounts/{id}`](#delete-account) | Delete external bank account |

## Details ##

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
| holderBank | [Holder Bank Object](../objects/objects.md#beneficiary_bank_object) | **Required.** The recipient bank details, holding the account. |
| holder | [Holder Object](../objects/objects.md#beneficiary_object) | **Required.** The recipient details, owner of the account. |

As a response to this query, you will receive a json response containing details of the [External Bank Account](../objects/objects.md#account_object) created. The unique AccountId must be stored and used in your future payments.

<hr />

#### <a id="get-accounts-list"></a> Retrieve external bank accounts list ####

```
Method: GET 
URL: /externalbankaccounts
```
With the FX4BIZ API, you can list all the external bank accounts hold by the person or compagny of a certain user.  
The user is not to be passed as a parameter since it's the one you use to authenticate that will be used.

*Parameters:*

This request is appliable for the [pagination format](../conventions/formatingConventions.md#pagination).

*Example:*
```
/externalbankaccounts/
```

As a response to this query, you will receive an Array containing the [External Bank Account Object](../objects/objects.md#account_object) for each external account.

<hr />

#### <a id="get-account-details"></a> Retrieve external bank account details ####

```
Method: GET 
URL: /externalbankaccount/{id}
```
This request allows you to see the details related to an account, to confirm display the beneficiary information in your application for example.  

*Parameters*  

| Field | Type | Description |
|-------|------|-------------|
| id | Integer | **Required.** The ID of the external bank account you want. <br :>As ID's are listed with the [Account Object](../objects/objects.md#account_object), You can retrive this by listing all external bank accounts for the current user. |

*Example:*
```
/externalbankaccounts/2041
```
As a response to this query, you will receive the details of the [External Bank Account](../objects/objects.md#account_object).

<hr />

#### <a id="delete-account"></a> Delete external bank account ####

```
Method: DELETE 
URL: /externalbankaccount/{id}
```
Delete an account.

As a response to this query, you will receive a HTTP 200 if the deleting was successfull, or an error if it wasn't.
