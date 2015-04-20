# LOG SERVICE #

As a developper, you might want to go further than sending request and recieve responses.  
The FX4BIZ API allows you to access the logs of its own services, to provide you some details about what you send, and what it's done on the platform.

You can retrieve informations during a certain time.  
This time passed, your log entry will be lost.

| HTTP Method | Time |
|-------------|------|
| GET | 1 hour |
| POST, PUT | 1 week |

## Routes ##

| Route | Description |
|-------|-------------|
| [`GET /logs`](#get-logs) | Retrieve Logs |
| [`GET /logs/{nonce}`](#get-log-by-nonce) | Retrieve a log entry with a nonce |

## Details ##

#### <a id="get-logs"></a> Retrieve Logs ####
```
Method: 	GET
URL: 		/logs
```
The FX4BIZ-REST API provides a log feed about request you made, allowing you to know exactly what your request do on the platform.  
This request uses the login sent in your header to get logs about this user's actions.

*Parameters:*

This request is appliable for the [pagination format](../conventions/formatingConventions.md#pagination).

*Example:*
```
/logs?per_page=10&page=1
```

As a response to this query, you will receive an Array made of [Log objects](../objects/objects.md#log_object) for all the log you're concerned.

<hr />

#### <a id="get-log-by-nonce"></a> Retrieve a log entry with a nonce ####

```
Method: 	GET
URL: 		/logs/{nonce}
```
In case of somewhat happens during the request, the FX4BIZ API allows you to retrive a log entry by its nonce.  

*Parameters:*

| Field | Type | Description |
|-------|------|-------------|
| nonce | String | **Required.** The nonce used to authenticate the request. As the one in the header, this nonce has to be [Base64](http://fr.wikipedia.org/wiki/Base64) encoded. |

*Example:*
```
/logs/MzI0ZWQ0YTA0NmJhM2VmZWZiNzYyMTkzMWQ1ZjY2M2I=
```

As a response to this query, you will receive an Array made of one [Log object](../objects/objects.md#log_object) corresponding with the nonce you sent.

