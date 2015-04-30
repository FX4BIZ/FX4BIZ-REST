# AUTHENTICATION SERVICE #

Every call to the FX4BIZ API must be authenticated, which must be done by adding a custom HTTP header (X-WSSE) with your username and secret.  
This section contains detailed instructions on how to create valid headers for the Suite REST API.

**All API request must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTPS). Calls made over plain [HTTP](http://en.wikipedia.org/wiki/HTTP) will fail. You must authenticate for all requests.**

## X-WSSE Format ##

The header has the following format, usually a single [HTTP](http://en.wikipedia.org/wiki/HTTP) header line which we broke down into multiple lines for easier readability:

```js
X-WSSE: UsernameToken
Username="customer001",
PasswordDigest="ZmI2ZmQ0MDIxYmQwNjcxNDkxY2RjNDNiMWExNjFkZA==",
Nonce="d36e3162829ed4c89851497a717f",
Created="2014-03-20T12:51:45Z"
```

The following sections describe each component in detail:

**X-WSSE :** Name of the [HTTP](http://en.wikipedia.org/wiki/HTTP) header we use for authenticating the request.  
**UsernameToken :** Authentication method. The X-WSSE header must contain a UsernameToken as we only support token-based authentication.  
**Username :** Field containing the username you were provided during onboarding.  
**PasswordDigest :** Field containing the hashed token which will prove the authenticity of your request. It is essential that you recompute this hash for every request as a hash is only valid for a certain period of time, and then it expires.  
**Nonce :** A random value used to make your request unique so it cannot be replicated by any other unknown party.  
**Created :** Field containing the current UTC, GMT, ZULU timestamp (YYYY-MM-DDTHH:MM:SSZ) according to the [ISO8601](http://fr.wikipedia.org/wiki/ISO_8601) format, *e.g. 2014-03-20T12:51:45Z*.

##Computing the Password Digest##

Computing the password digest involves 5 simple steps:

1. Get a randomly generated 16 byte Nonce formatted as 32 hexadecimal characters.
2. Get the current Created timestamp in [ISO-8601](http://fr.wikipedia.org/wiki/ISO_8601) format.
3. Concatenate the following three values in this order: nonce, timestamp, secret.
4. Calculate the [SHA-1](http://fr.wikipedia.org/wiki/SHA-1) hash value of the concatenated string, and make sure this value is in hexadecimal format! Some languages, like PHP, output hexadecimal hash by default. You may need to use special methods to obtain hexadecimal hashes in different languages or even convert byte to hex values by hand (see the sample codes below for more information).
5. Apply a [Base64](http://fr.wikipedia.org/wiki/Base64) encoding to the resulted hash to get the final PasswordDigest value.

##Exemple of creating the X-WSSE header with PHP##
Here is a sample code that you can use to make your X-WSSE header, or just in a way of understanding the process.

```php
<?php

// Login informations
$username = "YourUserName" ;			// The username used to authenticate
$password = "secret" ;					// The password used to authenticate
$nonce = "" ;							// The nonce
$nonce64 = "" ;							// The nonce with a Base64 encoding
$date = "" ;							// The date of the request, in  ISO 8601 format
$digest = "" ;							// The password digest needed to authenticate you
$header = "" ; 							// The final header to put in your request

// Making the nonce and the encoded nonce
$chars = "0123456789abcdef";
for ($i = 0; $i < 32; $i++) {
    $nonce .= $chars[rand(0, 15)];
}
$nonce64 = base64_encode($nonce) ;

// Getting the date at the right format (e.g. YYYY-MM-DDTHH:MM:SSZ)
$date = gmdate('c');
$date = substr($ts,0,19)."Z" ;

// Getting the password digest
$digest = base64_encode(sha1($nonce.$ts.$password, true));

// Getting the X-WSSE header to put in your request
$header = sprintf('X-WSSE: UsernameToken Username="%s", PasswordDigest="%s", Nonce="%s", Created="%s"',$username, $digest, $nonce64, $ts);

```

