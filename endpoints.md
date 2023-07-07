# Endpoints
There are the full list of endpoints availabe to work with reactioon network.

### Accounts  
All endpoints for accounts is the way to interact with user account. Below have an list of endpoints with their fields.

#### - Add API to account
`(POST)` `{@base_url}`/api/v2/account/keys/add  

| field    | Type    | Details |
| -------- | ------- | ------- |
| name     | string  | Any text to help identify the key.
| exchange | string  | An exchange available
| key      | string  | Your API KEY from exchange
| secret   | string  | Your API SECRET from exchange

#### - Remove API Key from an account
`(POST)` `{@base_url}`/api/v2/account/keys/remove  

#### - Check balance of an key
`(POST)` `{@base_url}`/api/v2/account/keys/check  

#### - Get details of an key
`(POST)` `{@base_url}`/api/v2/account/keys/details  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| key_id   | numeric | Id of key generated from reactioon.

### Snapshots  
All endpoints for snapshots provide an way to interact with snapshots of user. Below have an list of endpoints with their fields.

#### - Calculate the cost of an snapshot
`(POST)` `{@base_url}`/api/v2/snapshots/cost  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchanges available (list availables)
| currency | string  | Currency available (list availables).

#### - Get new snapshot
`(POST)` `{@base_url}`/api/v2/snapshots/new  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| time       |         | Available (last, 24h, 7d, 15d, 30d).
| _liquidity | numeric | 1 = use your liquidity balance, 0 = don't use your liquidity.

#### - Get snapshot details
`(POST)` `{@base_url}`/api/v2/snapshots/details  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| snapshot | numeric | ID of snapshot, use snapshots_id as reference.

#### - Get list of snapshots by period
`(POST)` `{@base_url}`/api/v2/snapshots/list/period  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| start    | date    | Date to start. (Ex: 20230626)
| end      | date    | Date to end. (Ex: 20230626)

#### - Get last snapshot added in account
`(GET)` `{@base_url}`/api/v2/snapshots/last  

#### - Get all assets inside of an snapshot
`(POST)` `{@base_url}`/api/v2/snapshots/assets 

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchanges available (list availables)
| currency | string  | Currency available (list availables).

### Watchlist  
All endpoints for watchlist provied an way to interact with assets added on user account to watch. Below have an list of endpoints with their fields.

#### - Get all assets added on watchlist
`(GET)` `{@base_url}`/api/v2/watchlist/assets  

#### - Toggle assets on watchlist
`(POST)` `{@base_url}`/api/v2/watchlist/assets/toggle  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| symbol     | string  | Asset symbol of an snapshot
| snapshot   | numeric | ID of snapshot, use snapshots_id as reference.

#### - Get details of an asset
`(POST)` `{@base_url}`/api/v2/watchlist/assets/details  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchange of your watchlist
| symbol   | string  | Asset symbol of your watchlist

#### - Remove all assets inside of watchlist
`(POST)` ? `{@base_url}`/api/v2/watchlist/assets/remove/all  

#### - Renew an asset to get last snapshot
`(POST)` `{@base_url}`/api/v2/watchlist/assets/renew  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| symbol     | string  | Asset symbol of an snapshot

#### - Renew all assets of watchlist
`(POST)` `{@base_url}`/api/v2/watchlist/assets/renew/all  

Note: no arguments, on this request, there are an POST request without any argument.

#### - Get all assets listed on best using an symbol as reference
`(POST)` `{@base_url}`/api/v2/watchlist/assets/best/symbol  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchange of your watchlist
| symbol   | string  | Asset symbol of your watchlist

#### - Get all assets listed on best using an snapshot as reference
`(POST)` `{@base_url}`/api/v2/watchlist/assets/best/snapshot  

| Param    | Type    | Details |
| -------- | ------- | ------- |
| exchange | string  | Exchange of your watchlist
| symbol   | string  | Asset symbol of your watchlist
| snapshot | string  | ID of snapshot, use snapshots_id as reference.

#### - Get market info about an asset inside of watchlist
`(POST)` `{@base_url}`/api/v2/watchlist/market/info  

| Param      | Type    | Details |
| --------   | ------- | ------- |
| exchange   | string  | Exchanges available (list availables)
| currency   | string  | Currency available (list availables).
| symbol     | string  | Asset symbol of an snapshot

#### - Get market targets to an asset
`(POST)` `{@base_url}`/api/v2/watchlist/market/targets

| Param      | Type     | Details |
| --------   | -------- | ------- |
| exchange   | string   | Exchanges available (list availables)
| currency   | string   | Currency available (list availables).
| symbol     | string   | Asset symbol of an snapshot
| days       | int      | Number of days. (max: 30)
| dateStart  | datetime | Date and time. (ex: 2023-05-26 22:30:00)

### Bots  
All endpoints here provide an way to interact with bots. Below have an list of endpoints with their fields.

**Important:**  
(1): After create/edit/remove an bot, the changes needs to be uploaded to network. 

#### - Get all bots
`(GET)` `{@base_url}`/api/v2/bots/spot/all  

#### - Calculate the cost an bot
`(POST)` `{@base_url}`/api/v2/bots/spot/cost  

| Param      | Type     | Details |
| --------   | -------- | ------- |
| name       | string   | name of your bot
| days       | int      | amount of days. (ex: 30)
| apiKey     | numeric  | ApiKeyID generated from reactioon when add new key.
| amount     | integer / decimal      | Total amount available for the bot.
| symbol     | string   | Asset symbol

#### - Create new bot
`(POST)` `{@base_url}`/api/v2/bots/spot/create  

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

#### - Edit bot
`(POST)` `{@base_url}`/api/v2/bots/spot/edit  


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

#### - Delete an bot
`(POST)` `{@base_url}`/api/v2/bots/spot/delete  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

#### - Renew bot period
`(POST)` `{@base_url}`/api/v2/bots/spot/renew  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.
| days                | integer            | Total days to renew

#### - Toggle status 
`(POST)` `{@base_url}`/api/v2/bots/spot/status/toggle  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

#### - Disable action
`(POST)` `{@base_url}`/api/v2/bots/spot/buy/disable  
`(POST)` `{@base_url}`/api/v2/bots/spot/sell/disable  
`(POST)` `{@base_url}`/api/v2/bots/spot/cancel/disable  
`(POST)` `{@base_url}`/api/v2/bots/spot/stoploss/disable  
`(POST)` `{@base_url}`/api/v2/bots/spot/workprice/disable  
`(POST)` `{@base_url}`/api/v2/bots/spot/fastactions/disable  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

#### - Enable action
`(POST)` `{@base_url}`/api/v2/bots/spot/buy/enable  
`(POST)` `{@base_url}`/api/v2/bots/spot/sell/enable  
`(POST)` `{@base_url}`/api/v2/bots/spot/cancel/enable  
`(POST)` `{@base_url}`/api/v2/bots/spot/fastactions/enable  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

#### - Set properties of action buy
`(POST)` `{@base_url}`/api/v2/bots/spot/buy/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| limit               | integer               | Total of orders open in parallel to buy.
| position            | integer               | Position in book that will open an order.

#### - Set properties of action sell
`(POST)` `{@base_url}`/api/v2/bots/spot/sell/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| limit               | integer               | Total of orders open in parallel to buy.
| position            | integer               | Position in book that will open an order.
| profit              | decimal               | The target aimed to sell an order.

#### - Set properties of action cancel
`(POST)` `{@base_url}`/api/v2/bots/spot/cancel/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| buy                 | integer               | Amount of time in minutes to cancel an order open on buy side.
| sell                | integer               | Amount of time in minutes toc ancel an order open om sell side.

#### - Set properties of action 'stop-loss'
`(POST)` `{@base_url}`/api/v2/bots/spot/stoploss/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| stoploss            | integer               | The target aimed to stop an order. This will enable sell an order if the entry reach more than the target in %. (ex: 10)

#### - Set properties of action 'work-price'
`(POST)` `{@base_url}`/api/v2/bots/spot/workprice/set  

| Param               | Type                  | Details |
| --------            | --------              | ------- |
| unique_name         | text                  | The Unique Name generated to your bot.
| buy_start           | decimal               | The price to enable the buy.
| buy_end             | decimal               | The price to disable the buy.
| sell_start          | decimal               | The price to enable the sell.
| sell_end            | decimal               | The price to disable the sell.

#### - Get trades list of an bot
`(POST)` `{@base_url}`/api/v2/bots/spot/trades/list  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

#### - Get latest trades of an bot
`(POST)` `{@base_url}`/api/v2/bots/spot/trades/latest  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.

#### - Get all orders open
`(POST)` `{@base_url}`/api/v2/bots/spot/orders/open  

@Note: There are no arguments to the endpoint. So, this endpoint will return orders open of all bots.

#### - Get last trade
`(POST)` `{@base_url}`/api/v2/bots/spot/stats/trade/last  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.
| symbol              | string             | Asset symbol

#### - Upload update
`(POST)` `{@base_url}`/api/v2/bots/spot/upload/update  

| Param               | Type               | Details |
| --------            | --------           | ------- |
| unique_name         | text               | The Unique Name generated to your bot.