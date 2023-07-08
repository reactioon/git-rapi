# Reactioon API (aka, RAPI)

RAPI stands for "Reactioon API" and is focused on developing solutions that use the reactioon network. The API provides a scenario with many possibilities to our users, with resources like bots, snapshots, watchlists and etc. All available with the protocol, see the details below.

### Base URL

```
@base_url = https://api.reactioon.com:1357
```

### Work-Flow 

The communication flow in general terms is simple, an authenticated and signed request will be made, after the request a response will be received after X seconds, this response will be in JSON format.

**GET:**  
```
[U] --> Auth (HMAC) --> [R] --> Response (JSON) --> [U]
```

**POST:**  
```
[U] --> Auth (HMAC) & Data --> [R] --> Response (JSON) --> [U]
```

[U] = User  
[R] = Reactioon

The authorization is an signature with public and private key that needs to be sent with header from the allowed host.

## Authorization

Reactioon Network uses HTTP Headers to identify the user making the request, see the available headers below.

* `X-RTN-KEY`  
The header key must be used to inform the public part of the API key created within the reaction tool.

* `X-RTN-SIGNATURE`  
The header signature must be used with value of an generated HMAC signature of the content being transported by the user. The signature is signed using the private part of the user's API key.

**Note:** If you don't have an public and private key of reactioon network, you can create inside of your account.

## Response
All responses from reactioon network is in JSON format. If the return is not an JSON, the request is with error.

## Security
* Reactioon use an combination of whitelist for hosts and signature keys to validate the request integrity.
* The data is sent over HTTPS, this will encrypt any data sent over network.
* The data sent over reactioon network needs an signature, this will prevent data change by third parties.

## Context & Endpoints

The API allows you to create customized solutions with different contexts, whether focusing on aspects related to bots or aspects related to analysis/snapshots. See below for available contexts.

* Account
* Watchlist
* Snapshots
* Bots  
* Network (coming soon)

All contexts have methods that allow you to freely manage any resource. Reactioon Network support two methods to perform all actions, GET and POST.

**Note:** in most of case, not all, the `GET` is available to get data and `POST` is available to send data. But there are some scenarios that `POST` is used to get data.

**Aspects**

* `Bots`  
The bots run over a distributed network, this mean that all changes needs to be uploaded to network.

* `Snapshots`  
Market snapshots are generated with an interval of 4h, check the last snapshot to see the time of the next snapshot.

### Endpoints
To integrate with reactioon network we have some private endpoints to any machine send/get data inside of reactioon, the whole group of endpoints enable full integration.

**Important:**  
(1): The endpoints use `GET` and `POST` as a method type of the request. In most of case, `GET` don't send fields on request, in URL or in body of the request. `POST` have fields inside of request, and each content in body needs an signature. So, the header of request must cointains a custom header called `X-RTN-SIGNATURE` with the hmac of content, but in case of `GET` is a blank HMAC signature with only the api secret. When the developer is using the official libraries, the handling of the situations reported above is done automatically.

**Notes:**  
(1): All fields sent on `POST` is required to each endpoint.  
(2): To prevent high cost with usage, save the return of an request in your environment and parse it to show  later, in most of case the data won't change in minutes. So, Create your own copy of the data, and if time requested is lower than 3 min load the data from your enviroment.

To full list of endpoints is available inside of file `endpoints.md`.

## Libraries
Reactioon has some libraries to help developers when creating solutions, see the available languages below:

* Go

The Go library was created by [@josewilker](https://github.com/josewilker), see the usage details below.

```go
package main

import (

	"fmt"
	"github.com/reactioon/rgo-rapi-lib-go/rapi"

)

func main() {

	// setup keys
	apiKey := []byte("{reactioon-api-key}")
	apiSecret := []byte("{reactioon-api-secret}")

	// load library
	r := rapi.Load(apiKey, apiSecret)

	// execute context with GET
	arrDataGet := make(map[string]string)
	requestReturnGet, requestErrGet := r.Request("GET", "api/v2/bots/spot/all", arrDataGet)

	fmt.Println("return", requestReturnGet)
	fmt.Println("error", requestErrGet)

	// execute context with POST
	arrDataPost := make(map[string]string)
	arrDataPost["exchange"] = "binance"
	arrDataPost["symbol"] = "BTCUSDT"
	arrDataPost["currency"] = "USDT"
	requestReturnPost, requestErrPost := r.Request("POST", "api/v2/watchlist/market/info", arrDataPost)

	fmt.Println("return", requestReturnPost)
	fmt.Println("error", requestErrPost)

}
```

You can get more details about the library for Go inside of their repository, [here](https://github.com/reactioon/rgo-rapi-lib-go).

* NodeJS

The NodeJS library was created by [@josewilker](https://github.com/josewilker), see the usage details below.

```js
async function Init() {

    var rapi = require('./rapi');
    r = rapi.Load("{reactioon-api-key}","{reactioon-api-secret}")

    // unique context
    r.Request("GET", "api/v2/bots/spot/all", {}).then(function(data){
        console.log("unique context:", data);
    });

    // reusable context
    d = await r.Request("GET", "api/v2/bots/spot/all", {});
    console.log("reusable context:", d);

}

Init()
```

You can get more details about the library for NodeJS inside of their repository, [here](https://github.com/reactioon/rgo-rapi-lib-nodejs).

* PHP

The PHP library was created by [@josewilker](https://github.com/josewilker), see the usage details below.

```php
<?php

require_once("rapi/rapi.php");

$rapi = new RAPI();
$rapi->Load("{reactioon-api-key}", "{reactioon-api-secret}");
$requestReturn = $rapi->Request("GET", "api/v2/bots/spot/all", array());
var_dump($requestReturn);

?>
```

You can get more details about the library for PHP inside of their repository, [here](https://github.com/reactioon/rgo-rapi-lib-php).

* Python

The Python library was created by [@lpereira199312](https://github.com/lpereira199312), see the usage details below.

```python
import rapi

apikey={your-api-key}
secretkey={your-secret-key}

# metodo post get price for get snapshot in exchange binance for currency usdt 

rapi.keys(apikey,secretkey)
r = rapi.request("POST","/api/v2/snapshots/cost",{'exchange':'binance','currency':'USDT'})
print(r.text)

# metodo get get assets in your watchlist

rapi.keys(apikey,secretkey)
r =rapi.request("GET","/api/v2/watchlist/assets","")
print(r.text)
```

You can get more details about the library for Python inside of their repository, [here](https://github.com/reactioon/rgo-rapi-lib-python).

## Postman

To speed up the process of learning about the RAPI, you can download the postman collection and install it on your machine. Put your key & secret in variables, and it's ready.

* [Download Postman](https://postman.com)
* [Download RAPI Collection](https://raw.githubusercontent.com/reactioon/rgo-rapi/master/postman_collection.json)

With more than 40 endpoints on collection about Reactioon Network, you can explore anything. See some examples of applications below.

## Use cases

The examples described below are to illustrate the possibilities, feel free to think about how to do other things.

* **Investment funds**  
Investment funds can create solutions integrated with reactioon, allowing personalized management and control of the data, without worrying about the core technology.

* **DeFi Protocols**  
Projects can integrate their systems to enable actions based on what is happening in their business/market.

* **Websites**  
Any developer can provide personalized information about the assets on their websites using reactioon network for personalized interaction with their users.

* **Chatbot**  
A chat bot to inform which are the best assets to invest, this can save time when looking for an asset to trade.

* **Backtest**  
With the reactioon API it is possible to collect targets and store them to carry out different activities in many contexts.

### Contributors
Reactioon is distributed tool created by people for people, and many users help the project to maintain a constant evolution. See below the list of people who contribute to the development of the API.

* [@josewilker](https://github.com/josewilker)
* [@lpereira199312](https://github.com/lpereira199312)

### Versions

Reactioon is under development, which means that over time the responses from procedures executed may have some different details. The integrity of old fields (name and meaning) and the structure of the documents will be preserved as much as possible.

## FAQ

* **What type of project can be developed ?**  
Feel free to build what you want.

* **How much requests an account can do ?**  
There are no limits. The access to the service is with an amount of requests that user buy inside of platform, when it's over, the user can buy more.

* **How much it cost ?**  
At now the cost is $0.1 for each request.

* **Can be used inside of an commercial solution ?**  
Yes, anyone that want create their business based on reactioon tools can do it.

* **When beta of reactioon is done ?**  
At now, we don't know. We see a lot of work to do and we will continue working on enhancements for reactioon.

* **How increase results with the tool ?**  
Read the articles inside of our forum, talk in public with users inside of telegram group. There a lot of possibilities, so the best way is talking with someone to help you.

* **How much cost support from reactioon developers ?**  
We don't have plans to have an direct support right now, we are an small team. We will work to offer the all ways to an user learn, develop and test.

### Conclusion
There are many possibilities to do with reactioon.network, feel free to create your own solution and let's build the market!

<br>

That's all folks!

José Wilker (CEO/Developer)