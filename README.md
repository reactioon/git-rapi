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

<br>

## Authorization

Reactioon Network uses HTTP Headers to identify the user making the request, see the available headers below.

* `X-RTN-KEY`  
The header key must be used to inform the public part of the API key created within the reaction tool.

* `X-RTN-SIGNATURE`  
The header signature must be used with value of an generated HMAC signature of the content being transported by the user. The signature is signed using the private part of the user's API key.

**Note:** If you don't have an public and private key of reactioon network, you can create inside of your account.

<br>

## Response
All responses from reactioon network is in JSON format. If the return is not an JSON, the request is with error.

<br>

## Security
* Reactioon use an combination of whitelist for hosts and signature keys to validate the request integrity.
* The data is sent over HTTPS, this will encrypt any data sent over network.
* The data sent over reactioon network needs an signature, this will prevent data change by third parties.

<br>

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

<br>

### Endpoints
To integrate with reactioon network we have some private endpoints to any machine send/get data inside of reactioon, the whole group of endpoints enable full integration.

**Important:**  
(1): The endpoints use `GET` and `POST` as a method type of the request. In most of case, `GET` don't send fields on request, in URL or in body of the request. `POST` have fields inside of request, and each content in body needs an signature. So, the header of request must cointains a custom header called `X-RTN-SIGNATURE` with the hmac of content, but in case of `GET` is a blank HMAC signature with only the api secret. When the developer is using the official libraries, the handling of the situations reported above is done automatically.

**Notes:**  
(1): All fields sent on `POST` is required to each endpoint.  
(2): To prevent high cost with usage, save the return of an request in your environment and parse it to show  later, in most of case the data won't change in minutes. So, Create your own copy of the data, and if time requested is lower than 3 min load the data from your enviroment.

#### Accounts  
All endpoints for accounts is the way to interact with user account. Below have an list of endpoints with their fields.

`(POST)` `{@base_url}`/account/keys/add  

| field    | Type    | Details |
| -------- | ------- | ------- |
| name     | string  | Any text to help identify the key.
| exchange | string  | An exchange available
| key      | string  | Your API KEY from exchange
| secret   | string  | Your API SECRET from exchange

`(POST)` `{@base_url}`/account/keys/remove  
`(POST)` `{@base_url}`/account/keys/check  
`(POST)` `{@base_url}`/account/keys/details  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| key_id   | numeric | Id of key generated from reactioon.

#### Snapshots  
All endpoints for snapshots provide an way to interact with snapshots of user. Below have an list of endpoints with their fields.

`(POST)` `{@base_url}`/snapshots/cost  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchanges available (list availables)
| currency | string  | Currency available (list availables).

`(POST)` `{@base_url}`/snapshots/new  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| time       |         | Available (last, 24h, 7d, 15d, 30d).
| _liquidity | numeric | 1 = use your liquidity balance, 0 = don't use your liquidity.

`(POST)` `{@base_url}`/snapshots/details  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| snapshot | numeric | ID of snapshot, use snapshots_id as reference.

`(POST)` `{@base_url}`/snapshots/list/period  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| start    | date    | Date to start. (Ex: 20230626)
| end      | date    | Date to end. (Ex: 20230626)

`(GET)` `{@base_url}`/snapshots/last  
`(POST)` `{@base_url}`/snapshots/assets 

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchanges available (list availables)
| currency | string  | Currency available (list availables).

#### Watchlist  
All endpoints for watchlist provied an way to interact with assets added on user account to watch. Below have an list of endpoints with their fields.

`(GET)` `{@base_url}`/watchlist/assets  
`(POST)` `{@base_url}`/watchlist/assets/toggle  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| symbol     | string  | Asset symbol of an snapshot
| snapshot   | numeric | ID of snapshot, use snapshots_id as reference.

`(POST)` `{@base_url}`/watchlist/assets/details  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchange of your watchlist
| symbol   | string  | Asset symbol of your watchlist

`(POST)` ? `{@base_url}`/watchlist/assets/remove/all  
`(POST)` `{@base_url}`/watchlist/assets/renew  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| symbol     | string  | Asset symbol of an snapshot

`(POST)` `{@base_url}`/watchlist/assets/renew/all  

Note: no arguments, on this request, there are an POST request without any argument.

`(POST)` `{@base_url}`/watchlist/assets/best/symbol  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchange of your watchlist
| symbol   | string  | Asset symbol of your watchlist

`(POST)` `{@base_url}`/watchlist/assets/best/snapshot  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchange of your watchlist
| symbol   | string  | Asset symbol of your watchlist
| snapshot | string  | ID of snapshot, use snapshots_id as reference.

`(POST)` `{@base_url}`/watchlist/market/info  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| symbol     | string  | Asset symbol of an snapshot

`(POST)` `{@base_url}`/watchlist/market/targets

| Param      | Type     | Details |
| --------   | -------- | ------- |
| exchange   | string   | Exchanges available (list availables)
| currency   | string   | Currency available (list availables).
| symbol     | string   | Asset symbol of an snapshot
| days       | int      | Number of days. (max: 30)
| dateStart  | datetime | Date and time. (ex: 2023-05-26 22:30:00)

#### Bots  
All endpoints here provide an way to interact with bots. Below have an list of endpoints with their fields.

`(GET)` `{@base_url}`/bots/spot/all  
`(POST)` `{@base_url}`/bots/spot/cost  

| Param      | Type     | Details |
| --------   | -------- | ------- |
| name       | string   | name of your bot
| days       | int      | amount of days. (ex: 30)
| apiKey     | numeric  | ApiKeyID generated from reactioon when add new key.
| amount     | integer / decimal      | Total amount available for the bot.
| symbol     | string   | Asset symbol

`(POST)` `{@base_url}`/bots/spot/create  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| name                | text               | name of your bot
| mode                | int                | amount of days. (ex: 30)
| profit              | decimal            | ApiKeyID generated from reactioon when add new key.
| risk_margin         | integer            | Total amount available for the bot.
| risk_days           | integer            | Total risk days
| spend_buy           | integer            | Integer amount that represent an buy order. (ex: 1)
| spend_sell          | integer            | Integer amount that represent an sell order. (ex: 1)
| limits_buy          | integer            | Total orders open in parallel to buy. (ex: 2)
| limits_sell         | integer            | Total orders open in parallel to sell. (ex: 1)
| cancel_buy          | integer            | Amount of time in minutes to cancel an order open of buy. (ex: 5)
| cancel_sell         | integer            | Amount of time in minutes to cancel an order open of sell. (ex: 10)
| wait                | integer            | Amount of time in seconds to wait after an action.
| symbol              | string             | Asset symbol
| api_key             | string             | ApiKeyID 
| balance             | string             | Total amount available to the bot.
| volatility          | decimal            | Volatility of the price, which means the difference between price of buy and sell, in points, each 0.1 = 1%, 1 = 10%. (ex: 0.1)
| days                | string             | Total days that bot will be running.
| book_position_buy   | integer            | Position that the order will be placed on book.
| book_position_sell  | integer            | Position that the order will be placed on book.
| borrow_open         | integer            | Flag to open an borrow to run a bot.
| _confirmed          | numeric            | Flag to confirm the whole data.

`(POST)` `{@base_url}`/bots/spot/edit  


| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.
| name                | text               | name of your bot
| mode                | int                | amount of days. (ex: 30)
| profit              | decimal            | ApiKeyID generated from reactioon when add new key.
| risk_margin         | integer            | Total amount available for the bot.
| risk_days           | integer            | Total risk days
| spend_buy           | integer            | Integer amount that represent an buy order. (ex: 1)
| spend_sell          | integer            | Integer amount that represent an sell order. (ex: 1)
| limits_buy          | integer            | Total orders open in parallel to buy. (ex: 2)
| limits_sell         | integer            | Total orders open in parallel to sell. (ex: 1)
| cancel_buy          | integer            | Amount of time in minutes to cancel an order open of buy. (ex: 5)
| cancel_sell         | integer            | Amount of time in minutes to cancel an order open of sell. (ex: 10)
| wait                | integer            | Amount of time in seconds to wait after an action.
| symbol              | string             | Asset symbol
| api_key             | string             | ApiKeyID 
| balance             | string             | Total amount available to the bot.
| volatility          | decimal            | Volatility of the price, which means the difference between price of buy and sell, in points, each 0.1 = 1%, 1 = 10%. (ex: 0.1)
| days                | string             | Total days that bot will be running.
| book_position_buy   | integer            | Position that the order will be placed on book.
| book_position_sell  | integer            | Position that the order will be placed on book.
| edit                | numeric            | Flag to confirm the whole data.

`(POST)` `{@base_url}`/bots/spot/delete  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

`(POST)` `{@base_url}`/bots/spot/renew  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.
| days                | integer            | Total days to renew

`(POST)` `{@base_url}`/bots/spot/status/toggle  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.


`(POST)` `{@base_url}`/bots/spot/buy/disable  
`(POST)` `{@base_url}`/bots/spot/sell/disable  
`(POST)` `{@base_url}`/bots/spot/cancel/disable  
`(POST)` `{@base_url}`/bots/spot/stoploss/disable  
`(POST)` `{@base_url}`/bots/spot/workprice/disable  
`(POST)` `{@base_url}`/bots/spot/fastactions/disable  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.


`(POST)` `{@base_url}`/bots/spot/buy/enable  
`(POST)` `{@base_url}`/bots/spot/sell/enable  
`(POST)` `{@base_url}`/bots/spot/cancel/enable  
`(POST)` `{@base_url}`/bots/spot/fastactions/enable  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

`(POST)` `{@base_url}`/bots/spot/buy/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| limit               | integer               | Total of orders open in parallel to buy.
| position            | integer               | Position in book that will open an order.


`(POST)` `{@base_url}`/bots/spot/sell/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| limit               | integer               | Total of orders open in parallel to buy.
| position            | integer               | Position in book that will open an order.
| profit              | decimal               | The target aimed to sell an order.

`(POST)` `{@base_url}`/bots/spot/cancel/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| buy                 | integer               | Amount of time in minutes to cancel an order open on buy side.
| sell                | integer               | Amount of time in minutes toc ancel an order open om sell side.

`(POST)` `{@base_url}`/bots/spot/stoploss/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| stoploss            | integer               | The target aimed to stop an order. This will enable sell an order if the entry reach more than the target in %. (ex: 10)

`(POST)` `{@base_url}`/bots/spot/workprice/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| buy_start           | decimal               | The price to enable the buy.
| buy_end             | decimal               | The price to disable the buy.
| sell_start          | decimal               | The price to enable the sell.
| sell_end            | decimal               | The price to disable the sell.

`(POST)` `{@base_url}`/bots/spot/trades/list  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

`(POST)` `{@base_url}`/bots/spot/trades/latest  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

`(POST)` `{@base_url}`/bots/spot/orders/open  

@Note: There are no arguments to the endpoint. So, this endpoint will return orders open of all bots.

`(POST)` `{@base_url}`/bots/spot/stats/trade/last  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.
| symbol              | string             | Asset symbol

`(POST)` `{@base_url}`/bots/spot/upload/update  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

<br>

## Libraries
Reactioon has some libraries to help developers when creating solutions, see the available languages below:

* Go

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

You can get more details about the library for Go inside of their repository, here.

* NodeJS

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

You can get more details about the library for NodeJS inside of their repository, here.

* PHP

```php
<?php

require_once("rapi/rapi.php");

$rapi = new RAPI();
$rapi->Load("{reactioon-api-key}", "{reactioon-api-secret}");
$requestReturn = $rapi->Request("GET", "api/v2/bots/spot/all", array());
var_dump($requestReturn);

?>
```

You can get more details about the library for PHP inside of their repository, here.

<br>

## Postman

To speed up the process of learning about the RAPI, you can download the postman collection and install it on your machine. Put your key & secret in variables, and it's ready.

* Download Postman
* Download RAPI Collection

With more than 40 endpoints on collection about Reactioon Network, you can explore anything. See some examples of applications below.

<br>

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

@author José Wilker <josewilker@reactioon.com>