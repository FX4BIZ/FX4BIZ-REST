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
By submitting a new `external bank` account, you must supply the relevant details in order to pay a beneficiary.

*Caution.* All your own `wallet` accounts have been created automatically when subscribing with FX4BIZ.

The Submit External Bank Account service allows to reference `external bank` accounts which are your own accounts or a third party account hold in another bank.
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
| number | String | Required | The recipient account number or Iban. `xxx4548` |
| currency | String | Required | Three-digit [ISO 4217 Currency Code](http://www.xe.com/iso4217.php) specifying the account currency. `EUR` |
| holderBank | [Holder Bank Object](../objects/objects.md#beneficiary_bank_object) | Required | The recipient bank details, holding the account. |
| holder | [Holder Object](../objects/objects.md#beneficiary_object) | Required | The recipient details, owner of the account. |
| tag | String | Optionnal | Custom Data. `John Doe bank account EUR` |
| correspondentBic | String | Optionnal | The intermediary bank BIC code. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| account | [External Bank Account Object](../objects/objects.md#account_object) | An object representing the external bank account you just created |

**Example:**
```
/externalBankAccounts/
```

<hr />

#### <a id="cget_externalbankaccounts"></a> Retrieve External Bank Accounts List ####

```
Method: GET 
URL: /externalBankAccounts/
```
With the FX4BIZ API, you can list all the external bank accounts hold by the person or compagny of a certain user.  
The user is not to be passed as a parameter since it's the one you use to authenticate that will be used.

**Parameters:**

This request is appliable for the [pagination format](../conventions/formatingConventions.md#pagination).

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| accounts | Array[[External Bank Account Object](../objects/objects.md#account_object)] | An array containing [External Bank Account Object](../objects/objects.md#account_object) describing requested external bank accounts. |

**Example:**
```
/externalBankAccounts/
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
| id | String | Required | The ID of the external bank account you want. <br :>As ID's are listed with the [External Bank Account Objects](../objects/objects.md#account_object), You can retrive this by listing all external bank accounts for the current user. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| account | [External Bank Account Object](../objects/objects.md#account_object) | An [External Bank Account Object](../objects/objects.md#account_object) containing details about external bank account requested. |

**Example:**
```
/externalBankAccounts/2041
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
| id | String | Required | The ID of the external bank account you want to delete. |

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| process | Object | An object containing the result of the process. |
| Object.result | Boolean | A boolean value describing if the process succeeded or not. |


**Example:**
```
/externalBankAccounts/2041
```

