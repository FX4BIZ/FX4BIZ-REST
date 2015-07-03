# EXTERNAL BANK ACCOUNT SERVICE #

In the FX4BIZ API, what we call `external bank` account, can be either your own account in another bank or a third party recipient account.

## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /externalBankAccounts/`](#post_externalbankaccounts) | Submit new external bank account |
| [`GET /externalBankAccounts/`](#cget_externalbankaccounts) | Retrieve external bank accounts list |
| [`GET /externalBankAccounts/{id}`](#get_externalbankaccounts) | Retrieve external bank account details |
| [`DELETE /externalBankAccounts/{id}`](#delete_externalbankaccounts) | Delete an external bank account |

## Details ##

#### <a id="post_externalbankaccounts"></a> Submit a new external bank account ####

```
Method: POST 
URL: /externalBankAccounts/
```
By submitting a new `external bank` account, you must supply the relevant details in order to pay a beneficiary.<br />
**Caution.** All your `physical` iban accounts hold with FX4BIZ will be automatically created when subscribing with us.

The Submit External Bank Account service allows to reference `external bank` accounts which can be either your own accounts in another bank or a third party account.

Adding an external bank has some rules :

* If you have the BIC/SWIFT of the bank, just submit it, and we will recover informations of the bank on our own.
* If you do not have the BIC/SWIFT of the bank, you have to refer at least its clearing code type, its clearing code, its name and its city.

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

Of course, it is possible to reference third party `wallet` accounts and execute an instant and free `wallet2wallet` payment at this destination. 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| accountNumber | String | Required | The recipient account number or Iban. `xxx4548` |
| currency | [Currency](../conventions/formatingConventions.md#type_currency) | Required | A [three digit code](../conventions/formatingConventions.md#type_currency) specifying the account currency. `EUR` |
| holderBank | [Holder Bank Object](../objects/objects.md#beneficiary_bank_object) | Required | The recipient bank details, holding the account. |
| holder | [Holder Object](../objects/objects.md#beneficiary_object) | Required | The recipient details, owner of the account. |
| tag | String(50) | Optionnal | Custom Data. `John Doe bank account EUR` |
| correspondentBic | String | Optionnal | The intermediary bank identifier code. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| account | [External Bank Account Object](../objects/objects.md#account_object) | An object representing the external bank account you have just created |

**Example:**
```js
POST /externalBankAccounts/
{
    "currency": "EUR",
    "tag": "My EUR account",
    "accountNumber": "5495115813572165205435",
    "correspondentBankBic": "AGRIFRPP",
    "holderBank": {
        "bic": "CHASUS33",
        "clearingCodeType": "FW",
        "clearingCode": "021000021",
        "name": "JPMORGAN CHASE BANK, N.A.",
        "address": {
            "street": "4 NEW YORK PLAZA FLOOR 15",
            "postCode": "100004",
            "city": "New York",
            "state":"New York",
            "country": "US"
        }
    },
    "holder": {
        "name": "Client 453",
        "type": "Corporate",
        "address": {
            "street": "5  1st street",
            "postCode": "10004",
            "city": "New York",
            "state":"New York',
            "country": "US"
        }
    }
}
```

<hr />

#### <a id="cget_externalbankaccounts"></a> Retrieve External Bank Accounts List ####

```
Method: GET 
URL: /externalBankAccounts/
```
With the FX4BIZ API, you can list all the external bank accounts hold by the person or company of a certain user.  
The user is not to be passed as a parameter since it's the one you use to authenticate that will be used.

**Parameters:**

This request is appliable for the [pagination format](../conventions/formatingConventions.md#pagination).

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| accounts | Array[[External Bank Account Object](../objects/objects.md#account_object)] | An array containing [External Bank Account Object](../objects/objects.md#account_object) describing requested external bank accounts. |

**Example:**
```js
GET /externalBankAccounts/
```

<hr />

#### <a id="get_externalbankaccounts"></a> Retrieve External Bank Account Details ####

```
Method: GET 
URL: /externalBankAccounts/{id}
```
This request allows you to see the details related to an account. In order to confirm, you can display the beneficiary information in your application for example.  

**Parameters:**  

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id)  | Required | The [ID](../conventions/formatingConventions.md#type_id) of the external bank account you want. <br :>As ID's are listed with the [External Bank Account Objects](../objects/objects.md#account_object), You can retrieve this by listing all external bank accounts for the current user. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| account | [External Bank Account Object](../objects/objects.md#account_object) | An [External Bank Account Object](../objects/objects.md#account_object) containing details about external bank account requested. |

**Example:**
```js
GET /externalBankAccounts/NT4aB1
```

<hr />

#### <a id="delete_externalbankaccounts"></a> Delete an external bank account ####

```
Method: DELETE 
URL: /externalBankAccounts/{id}
```
Delete an external bank account.

**Parameters:**  

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formatingConventions.md#type_id) | Required | The [ID](../conventions/formatingConventions.md#type_id) of the external bank account you want to delete. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| process | [Process Result Object](../objects/objects.md#processresult_object) | A [Process Result Object](../objects/objects.md#processresult_object) containing the result of the process. |

**Example:**
```js
DELETE /externalBankAccounts/ND4EbA
```

