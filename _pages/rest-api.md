---
title: "Posts by Year"
permalink: /rest-api/
# layout: posts
author_profile: true
---
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Public Rest API for Coins (2022-09-12)](#public-rest-api-for-coins-2022-09-12)
  - [General API Information](#general-api-information)
  - [HTTP Return Codes](#http-return-codes)
  - [Error Codes](#error-codes)
  - [General Information on Endpoints](#general-information-on-endpoints)
    - [LIMITS](#limits)
    - [Endpoint security type](#endpoint-security-type)
    - [SIGNED (TRADE and USER_DATA) Endpoint security](#signed-trade-and-user_data-endpoint-security)
    - [Timing security](#timing-security)
    - [SIGNED Endpoint Examples for POST /openapi/v1/order](#signed-endpoint-examples-for-post-openapiv1order)
      - [Example 1: As a query string](#example-1-as-a-query-string)
      - [Example 2: As a request body](#example-2-as-a-request-body)
      - [Example 3: Mixed query string and request body](#example-3-mixed-query-string-and-request-body)
  - [Public API Endpoints](#public-api-endpoints)
    - [Terminology](#terminology)
    - [ENUM definitions](#enum-definitions)
    - [General endpoints](#general-endpoints)
      - [Test connectivity](#test-connectivity)
      - [Check server time](#check-server-time)
      - [Get user ip](#get-user-ip)
      - [Exchange information](#exchange-information)
    - [Convert endpoints](#convert-endpoints)
    - [Market Data endpoints](#market-data-endpoints)
      - [Order book](#order-book)
      - [Recent trades list](#recent-trades-list)
      - [Kline/Candlestick data](#klinecandlestick-data)
      - [24hr ticker price change statistics](#24hr-ticker-price-change-statistics)
      - [Symbol price ticker](#symbol-price-ticker)
      - [Symbol order book ticker](#symbol-order-book-ticker)
      - [Current average price](#current-average-price)
      - [Cryptoasset trading pairs](#cryptoasset-trading-pairs)
    - [Spot Trading Endpoints](#account-endpoints)
      - [New order  (TRADE)](#new-order--trade)
      - [Test new order (TRADE)](#test-new-order-trade)
      - [Query order (USER_DATA)](#query-order-user_data)
      - [Cancel order (TRADE)](#cancel-order-trade)
      - [Cancel All Open Orders on a Symbol (TRADE)](#cancel-all-open-orders-on-a-symbol-trade)
      - [Current open orders (USER_DATA)](#current-open-orders-user_data)
      - [History orders (USER_DATA)](#history-orders-user_data)
      - [Account information (USER_DATA)](#account-information-user_data)
      - [Account trade list (USER_DATA)](#account-trade-list-user_data)
      - [withdraw (to coins_ph account) (USER_DATA)](#withdraw-to-coins_ph-account-user_data)
      - [deposit (to xyz account) (USER_DATA)](#deposit-to-xyz-account-user_data)
      - [withdraw order history (withdrawal order which withdraw from exchange to coins_ph) (USER_DATA)](#withdraw-order-history-withdrawal-order-which-withdraw-from-exchange-to-coins_ph-user_data)
      - [deposit order history(deposit order which deposit from coins_ph to exchange) (USER_DATA)](#deposit-order-historydeposit-order-which-deposit-from-coins_ph-to-exchange-user_data)
      - [Payment request (USER_DATA)](#payment-request-user_data)
      - [Trade Fee (USER_DATA)](#trade-fee-user_data)
    - [User data stream endpoints](#user-data-stream-endpoints)
      - [Start user data stream (USER_STREAM)](#start-user-data-stream-user_stream)
      - [Keepalive user data stream (USER_STREAM)](#keepalive-user-data-stream-user_stream)
      - [Close user data stream (USER_STREAM)](#close-user-data-stream-user_stream)
    - [Filters](#filters)
      - [Symbol filters](#symbol-filters)
        - [PRICE_FILTER](#price_filter)
        - [PERCENT_PRICE](#percent_price)
        - [PERCENT_PRICE_SA](#percent_price_sa)
        - [PERCENT_PRICE_BY_SIDE](#percent_price_by_side)
        - [PERCENT_PRICE_INDEX](#percent_price_index)
        - [PERCENT_PRICE_ORDER_SIZE](#percent_price_order_size)
        - [STATIC_PRICE_RANGE](#static_price_range)
        - [LOT_SIZE](#lot_size)
        - [NOTIONAL](#notional)
        - [MAX_NUM_ORDERS](#max_num_orders)
        - [MAX_NUM_ALGO_ORDERS](#max_num_algo_orders)
      - [Exchange Filters](#exchange-filters)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# Change log:

2022-09-12: change 'Cancel All Open Orders on a Symbol' API  request parameter 'symbol' as required.

2022-09-09: update orderId/transactTime/time/updateTime returning number instead of string in order related interfaces.

2022-08-24: update STOP_LOSS/TAKE_PROFIT description in 'New order  (TRADE)' API.

2022-08-23: fix incorrect depth infomation

2022-08-19: add weight infomation for all interfaces

2022-08-17: change endpoint to **https://api.qa.coinsxyz.me**, **http** should not be used.

2022-08-15: change endpoint to **http://api.qa.coinsxyz.me/**

2022-08-12: change maxNumOrders to 200 in filter MAX_NUM_ORDERS

2022-08-12: change maxNumAlgoOrders to 5 in filter MAX_NUM_ALGO_ORDERS

2023-04-10: add transfer interfaces


# Public Rest API for Coins (2022-09-12)

## General API Information

* The base endpoint is: **https://api.qa.coinsxyz.me** (sandbox)
* All endpoints return either a JSON object or array.
* Data is returned in **ascending** order. Oldest first, newest last.
* All time and timestamp related fields are in milliseconds.

## HTTP Return Codes
* HTTP `4XX` return codes are used for for malformed requests;the issue is on the sender's side.
* HTTP `403` return code is used when the WAF Limit (Web Application Firewall) has been violated.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on exchange's side.It is important to **NOT** treat this as a failure operation; the execution status is UNKNOWN and could have been a success.

## Error Codes
* Any endpoint can return an ERROR; the error payload is as follows:

```javascript
{
  "code": -1121,
  "msg": "Invalid symbol."
}
```

* Specific error codes and messages defined in another document.



## General Information on Endpoints

* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a
  `query string` or in the `request body` with content type
  `application/x-www-form-urlencoded`. You may mix parameters between both the
  `query string` and `request body` if you wish to do so.
* Parameters may be sent in any order.
* If a parameter sent in both the `query string` and `request body`, the
  `query string` parameter will be used.



### LIMITS

**General Info on Limits**

* intervalNum describes the amount of the interval. For example, intervalNum 5 with intervalLetter M means "Every 5 minutes".

* A 429 will be returned when either rate limit is violated.



### IP Limits

* Each route has a `weight` which determines for the number of requests each endpoint counts for. Heavier endpoints and endpoints that do operations on multiple symbols will have a heavier `weight`.
* When a 429 is recieved, it's your obligation as an API to back off and not spam the API.
* **Repeatedly violating rate limits and/or failing to back off after receiving 429s will result in an automated IP ban (http status 418).**
* IP bans are tracked and **scale in duration** for repeat offenders, **from 2 minutes to 3 days**.
* A `Retry-After` header is sent with a 418 or 429 responses and will give the **number of seconds**required to wait, in the case of a 429, to prevent a ban, or, in the case of a 418, until the ban is over.
* **The limits on the API are based on the IPs, not the API keys.**



### Order Rate Limits

- When the order count exceeds the limit, you will receive a 429 error without the `Retry-After` header.

- The order rate limit is counted against each IP and UID.



### Websocket Limits

- A single connection can listen to a maximum of 1024 streams.



### /api/ Limit Introduction

- Endpoints related to `/api/*`:

  - According to the two modes of IP and UID limit, each are independent.

  - Endpoints share the 1200 per minute limit based on IP.



### Endpoint security type

* Each endpoint has a security type that determines the how you will interact with it.This is stated next to the NAME of the endpoint.
  - If no security type is stated, assume the security type is NONE.
* API-keys are passed into the Rest API via the `X-COINS-APIKEY`header.
* API-keys and secret-keys **are case sensitive**.
* API-keys can be configured to only access certain types of secure endpoints.
  For example, one API-key could be used for TRADE only, while another API-key can access everything except for TRADE routes.
* By default, API-keys can access all secure routes.

Security Type | Description
------------ | ------------
NONE | Endpoint can be accessed freely.
TRADE | Endpoint requires sending a valid API-Key and signature.
USER_DATA | Endpoint requires sending a valid API-Key and signature.
USER_STREAM | Endpoint requires sending a valid API-Key.
MARKET_DATA | Endpoint requires sending a valid API-Key.

* `TRADE` and `USER_DATA` endpoints are `SIGNED` endpoints.



### SIGNED (TRADE and USER_DATA) Endpoint security

* `SIGNED` endpoints require an additional parameter, `signature`, to be
  sent in the  `query string` or `request body`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation.
  Use your `secretKey` as the key and `totalParams` as the value for the HMAC operation.
* The `signature` is **not case sensitive**.
* `totalParams` is defined as the `query string` concatenated with the
  `request body`.



### Timing security

* A `SIGNED` endpoint also requires a parameter, `timestamp`, to be sent which
  should be the millisecond timestamp of when the request was created and sent.
* An additional parameter, `recvWindow`, may be sent to specify the number of
  milliseconds after `timestamp` the request is valid for. If `recvWindow`
  is not sent, **it defaults to 5000**.
* Currently, `recvWindow` is only used when creates order.
* The logic is as follows:

  ```javascript
  if (timestamp < (serverTime + 1000) && (serverTime - timestamp) <= recvWindow) {
    // process request
  } else {
    // reject request
  }
  ```

**Serious trading is about timing.** Networks can be unstable and unreliable,
which can lead to requests taking varying amounts of time to reach the
servers. With `recvWindow`, you can specify that the request must be
processed within a certain number of milliseconds or be rejected by the
server.

**It recommended to use a small recvWindow of 5000 or less!The max cannot go beyond 60,000!**



### SIGNED Endpoint Examples for POST /openapi/v1/order

Here is a step-by-step example of how to send a valid signed payload from the
Linux command line using `echo`, `openssl`, and `curl`.

Key | Value
------------ | ------------
apiKey | tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW
secretKey | lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76

Parameter | Value
------------ | ------------
symbol | ETHBTC
side | BUY
type | LIMIT
timeInForce | GTC
quantity | 1
price | 0.1
recvWindow | 5000
timestamp | 1538323200000



#### Example 1: As a query string

* **queryString:** symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000" | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= 5f2750ad7589d1d40757a55342e621a44037dad23b5128cc70e18ec1d1c3f4c6
```

* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW" -X POST 'https://$HOST/openapi/v1/order?symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000&signature=5f2750ad7589d1d40757a55342e621a44037dad23b5128cc70e18ec1d1c3f4c6'
```



#### Example 2: As a request body

* **requestBody:** symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000" | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= 5f2750ad7589d1d40757a55342e621a44037dad23b5128cc70e18ec1d1c3f4c6
```

* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW" -X POST 'https://$HOST/openapi/v1/order' -d 'symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000&signature=5f2750ad7589d1d40757a55342e621a44037dad23b5128cc70e18ec1d1c3f4c6'
```



#### Example 3: Mixed query string and request body

* **queryString:** symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC
* **requestBody:** quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTCquantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000" | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= 885c9e3dd89ccd13408b25e6d54c2330703759d7494bea6dd5a3d1fd16ba3afa
```

* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW" -X POST 'https://$HOST/openapi/v1/order?symbol=ETHBTC&side=BUY&type=LIMIT&timeInForce=GTC' -d 'quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000&signature=885c9e3dd89ccd13408b25e6d54c2330703759d7494bea6dd5a3d1fd16ba3afa'
```

Note that the signature is different in example 3.
There is no & between "GTC" and "quantity=1".



## Public API Endpoints

### Terminology

These terms will be used throughout the documentation, so it is recommended especially for new users to read to help their understanding of the API.

* `base asset` refers to the asset that is the `quantity` of a symbol. For the symbol BTCUSDT, BTC would be the `base asset`.
* `quote asset` refers to the asset that is the `price` of a symbol. For the symbol BTCUSDT, USDT would be the `quote asset`.



### ENUM definitions

**Symbol status:**

* TRADING
* BREAK (ongoing)
* CANCEL_ONLY (ongoing)

**Order status:**

Status | Description
-----------| --------------
`NEW` | The order has been accepted by the engine.
`PARTIALLY_FILLED`| A part of the order has been filled.
`FILLED` | The order has been completed.
`PARTIALLY_CANCEL`| A part of the order has been cancelled with self trade.
`CANCELED` | The order has been canceled .
`REJECTED`       | The order was not accepted by the engine and not processed.

**Order types:**

* LIMIT
* MARKET
* LIMIT_MAKER
* STOP_LOSS
* STOP_LOSS_LIMIT
* TAKE_PROFIT
* TAKE_PROFIT_LIMIT



**Order Response Type (newOrderRespType):**

* ACK

* RESULT

* FULL



**Order side:**

* BUY
* SELL



**Time in force (timeInForce):**

This sets how long an order will be active before expiration.

Status | Description
-----------| --------------
`GTC` | Good Til Canceled <br> An order will be on the book unless the order is canceled.
`IOC` | Immediate Or Cancel <br> An order will try to fill the order as much as it can before the order expires.
`FOK`| Fill or Kill <br> An order will expire if the full order cannot be filled upon execution.

**Kline/Candlestick chart intervals:**

m -> minutes; h -> hours; d -> days; w -> weeks; M -> months

* 1m
* 3m
* 5m
* 15m
* 30m
* 1h
* 2h
* 4h
* 6h
* 8h
* 12h
* 1d
* 3d
* 1w
* 1M



### Filters

Filters define trading rules on a symbol or an exchange. Filters come in two forms: `symbol filters` and `exchange filters`.



#### Symbol filters

##### PRICE_FILTER

The `PRICE_FILTER` defines the `price` rules for a symbol. There are 3 parts:

* `minPrice` defines the minimum `price`/`stopPrice` allowed.
* `maxPrice` defines the maximum `price`/`stopPrice` allowed.
* `tickSize` defines the intervals that a `price`/`stopPrice` can be increased/decreased by.

In order to pass the `price filter`, the following must be true for `price`/`stopPrice`:

* `price` >= `minPrice`
* `price` <= `maxPrice`
* (`price`-`minPrice`) % `tickSize` == 0

**/exchangeInfo format:**

```javascript
  {
    "filterType": "PRICE_FILTER",
    "minPrice": "0.00000100",
    "maxPrice": "100000.00000000",
    "tickSize": "0.00000100"
  }
```



##### PERCENT_PRICE

The `PERCENT_PRICE` filter defines valid range for a price based on the weighted average of the previous trades. `avgPriceMins` is the number of minutes the weighted average price is calculated over.

In order to pass the `percent price`, the following must be true for `price`:

- `price` <= `weightedAveragePrice` * `multiplierUp`
- `price` >= `weightedAveragePrice` * `multiplierDown`

**/exchangeInfo format:**

```javascript
  {
    "filterType": "PERCENT_PRICE",
    "multiplierUp": "1.3000",
    "multiplierDown": "0.7000",
    "avgPriceMins": 5
  }
```



##### PERCENT_PRICE_SA

The `PERCENT_PRICE_SA` filter defines valid range for a price based on the  simple average of the previous trades. `avgPriceMins` is the number of minutes the simple average price is calculated over.

In order to pass the `percent_price_sa`, the following must be true for `price`:

- `price` <= `simpleAveragePrice` * `multiplierUp`
- `price` >= `simpleAveragePrice` * `multiplierDown`

**/exchangeInfo format:**

```javascript
  {
    "filterType": "PERCENT_PRICE_SA",
    "multiplierUp": "1.3000",
    "multiplierDown": "0.7000",
    "avgPriceMins": 5
  }
```



##### PERCENT_PRICE_BY_SIDE

The `PERCENT_PRICE_BY_SIDE` filter defines the valid range for the price based on the last price of the symbol.
There is a different range depending on whether the order is placed on the `BUY` side or the `SELL` side.

Buy orders will succeed on this filter if:

- `Order price` <= `bidMultiplierUp` * `lastPrice`
- `Order price` >= `bidMultiplierDown` * `lastPrice`

Sell orders will succeed on this filter if:

- `Order Price` <= `askMultiplierUp` * `lastPrice`
- `Order Price` >= `askMultiplierDown` * `lastPrice`

**/exchangeInfo format:**

```javascript
  {
    "filterType": "PERCENT_PRICE_BY_SIDE",
    "bidMultiplierUp": "1.2",
    "bidMultiplierDown": "0.2",
    "askMultiplierUp": "1.5",
    "askMultiplierDown": "0.8",
  }
```



##### PERCENT_PRICE_INDEX

The `PERCENT_PRICE_INDEX` filter defines valid range for a price based on the  index price which is calculated based on  several exhanges in the market by centain rule. (indexPrice wobsocket pushing will be available in future)

In order to pass the `percent_price_index`, the following must be true for `price`:

- `price` <= `indexPrice` * `multiplierUp`
- `price` >= `indexPrice` * `multiplierDown`

**/exchangeInfo format:**

```javascript
  {
    "filterType": "PERCENT_PRICE_INDEX",
    "multiplierUp": "1.3000",
    "multiplierDown": "0.7000",
  }
```



##### PERCENT_PRICE_ORDER_SIZE

The `PERCENT_PRICE_ORDER_SIZE` filter  is used to determine whether the execution of an order would cause the market price to fluctuate beyond the limit price, and if so, the order will be rejected.

In order to pass the `percent_price_order_size`, the following must be true:

- A buy order needs to meet: the market price after the order get filled  <`askPrice` * `multiplierUp`
- A sell order needs to meet: the market price after the order get filled  >`bidPrice` * `multiplierDown`

**/exchangeInfo format:**

```javascript
  {
    "filterType": "PERCENT_PRICE_ORDER_SIZE",
    "multiplierUp": "1.3000",
    "multiplierDown": "0.7000"
  }
```



##### STATIC_PRICE_RANGE

The `STATIC_PRICE_RANGE` filter defines a static valid range for the price.

In order to pass the `static_price_range`, the following must be true for `price`:

- `price` <= `priceUp`
- `price` >= `priceDown`

**/exchangeInfo format:**

```javascript
  {
    "filterType": "STATIC_PRICE_RANGE",
    "priceUp": "520",
    "priceDown": "160"
  }
```



##### LOT_SIZE

The `LOT_SIZE` filter defines the `quantity` (aka "lots" in auction terms) rules for a symbol. There are 3 parts:

* `minQty` defines the minimum `quantity` allowed.
* `maxQty` defines the maximum `quantity` allowed.
* `stepSize` defines the intervals that a `quantity`can be increased/decreased by.

In order to pass the `lot size`, the following must be true for `quantity`:

* `quantity` >= `minQty`
* `quantity` <= `maxQty`
* (`quantity`-`minQty`) % `stepSize` == 0

**/exchangeInfo format:**

```javascript
  {
    "filterType": "LOT_SIZE",
    "minQty": "0.00100000",
    "maxQty": "99999999.00000000",
    "stepSize": "0.00100000"
  }
```



##### NOTIONAL

The `NOTIONAL` filter defines the acceptable notional range allowed for an order on a symbol.

In order to pass this filter, the notional (`price * quantity`) has to pass the following conditions:

- `price * quantity` <= `maxNotional`
- `price * quantity` >= `minNotional`

**/exchangeInfo format:**

```javascript
{
   "filterType": "NOTIONAL",
   "minNotional": "10.00000000",
   "maxNotional": "10000.00000000"
}
```



##### MAX_NUM_ORDERS

The `MAX_NUM_ORDERS` filter defines the maximum number of orders an account is allowed to have open on a symbol.
Note that both triggered "algo" orders and normal orders are counted for this filter.

**/exchangeInfo format:**

```javascript
  {
    "filterType": "MAX_NUM_ORDERS",
    "maxNumOrders": 200
  }
```



##### MAX_NUM_ALGO_ORDERS

The `MAX_ALGO_ORDERS` filter defines the maximum number of untriggered "algo" orders an account is allowed to have open on a symbol.
"Algo" orders are `STOP_LOSS`, `STOP_LOSS_LIMIT`, `TAKE_PROFIT`, and `TAKE_PROFIT_LIMIT` orders.

**/exchangeInfo format:**

```javascript
  {
    "filterType": "MAX_NUM_ALGO_ORDERS",
    "maxNumAlgoOrders": 5
  }
```



#### Exchange Filters

None for now



### General endpoints

#### Test connectivity

```shell
GET /openapi/v1/ping
```

Test connectivity to the Rest API.

**Weight:** 1

**Parameters:** NONE

**Response:**

```javascript
{}
```



#### Check server time

```shell
GET /openapi/v1/time
```

Test connectivity to the Rest API and get the current server time.

**Weight:** 1

**Parameters:** NONE

**Response:**

```javascript
{
  "serverTime": 1538323200000
}
```



#### Get user ip

```shell
GET /openapi/v1/user/ip
```

Get the user ip.

**Weight:** 1

**Parameters:** NONE

**Response:**

```javascript
{
  "ip": "57.181.16.43"
}
```



#### Exchange information

```shell
GET /openapi/v1/exchangeInfo
```

Current exchange trading rules and symbol information

**Weight:** 10

**Parameters:**

| Name    | Type   | Mandatory | Description                                                  |
| ------- | ------ | --------- | ------------------------------------------------------------ |
| symbol  | STRING | NO        | Specify a trading pair, for example symbol=ETHBTC            |
| symbols | STRING | NO        | x-Specify multiple trading pairs, such as symbol=%5B"ETHBTC","BTCUSDT"%5D, note that %5B represents '[' left bracket, %5D represents ']' right bracket. Direct use of the format ["ETHBTC","BTCUSDT"] is not supported as it is not RFC 3986 compliant. |

**Response:**

```json
{
  "timezone": "UTC",
  "serverTime": 1538323200000,
  "exchangeFilters": [],
  "symbols": [
    {
      "symbol": "ETHBTC",
      "status": "TRADING",
      "baseAsset": "ETH",
      "baseAssetPrecision": 8,
      "quoteAsset": "BTC",
      "quoteAssetPrecision": 8,
      "orderTypes": [
        "LIMIT",
        "MARKET",
        "LIMIT_MAKER",
        "STOP_LOSS_LIMIT",
        "STOP_LOSS",
        "TAKE_PROFIT_LIMIT",
        "TAKE_PROFIT"
      ],
      "filters": [
        {
          "filterType": "PRICE_FILTER",
          "minPrice": "0.00000100",
          "maxPrice": "100000.00000000",
          "tickSize": "0.00000100"
        },
        {
          "filterType": "LOT_SIZE",
          "minQty": "0.00100000",
          "maxQty": "100000.00000000",
          "stepSize": "0.00100000"
        },
        {
          "filterType": "NOTIONAL",
          "minNotional": "0.00100000"
        },
        {
          "filterType": "MIN_NOTIONAL",
          "minNotional": "0.00100000"
        },
        {
          "filterType": "MAX_NUM_ORDERS",
          "maxNumOrders": 200
        },
        {
          "filterType": "MAX_NUM_ALGO_ORDERS",
          "maxNumAlgoOrders": 5
        }
      ]
    }
  ]
}
```

### Market Data endpoints

#### Order book

```shell
GET /openapi/quote/v1/depth
```

**Weight:**

Adjusted based on the limit:

Limit | Weight
------------ | ------------
5, 10, 20, 50, 100 | 1
200 | 5

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 100; max 200.

**Caution:** setting limit=0 can return 200 records.

**Response:**

[PRICE, QTY]

```javascript
{
  "lastUpdateId": 1027024,
  "bids": [
    [
      "4.90000000",   // PRICE
      "331.00000000"  // QTY
    ],
    [
      "4.00000000",
      "431.00000000"
    ]
  ],
  "asks": [
    [
      "4.00000200",  // PRICE
      "12.00000000"  // QTY
    ],
    [
      "5.10000000",
      "28.00000000"
    ]
  ]
}
```



#### Recent trades list

```shell
GET /openapi/quote/v1/trades
```

Get recent trades (up to last 60).

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |EXP: BTCUSDT
limit | INT | NO | Default 500; max 1000. if limit <=0 or > 1000 then return 1000

**Response:**

```javascript
[
  {
    "id": 28457,
    "price": "4.00000100",
    "qty": "12.00000000",
    "quoteQty": "48.000012",
    "time": 1499865549590,
    "isBuyerMaker": true,
    "isBestMatch": true
  }
]
```



#### Kline/Candlestick data

```shell
GET /openapi/quote/v1/klines
```

Kline/candlestick bars for a symbol.
Klines are uniquely identified by their open time.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |EXP: BTCUSDT
interval | ENUM | YES |
startTime | LONG | NO |
endTime | LONG | NO |
limit | INT | NO | Default 500; max 1000.

* If startTime and endTime are not sent, the most recent klines are returned.

**Response:**

```javascript
[
  [
    1499040000000,      // Open time
    "0.01634790",       // Open
    "0.80000000",       // High
    "0.01575800",       // Low
    "0.01577100",       // Close
    "148976.11427815",  // Volume
    1499644799999,      // Close time
    "2434.19055334",    // Quote asset volume
    308,                // Number of trades
    "1756.87402397",    // Taker buy base asset volume
    "28.46694368"       // Taker buy quote asset volume
  ]
]
```

#### Current average price

```shell
GET /openapi/quote/v1/avgPrice
```

Current average price for a symbol.

**Weight:** 1

**Parameters:**

| Name   | Type   | Mandatory | Description                                           |
| ------ | ------ | --------- | ----------------------------------------------------- |
| symbol | STRING | YES       | symbol is not case sensitive, e.g. BTCUSDT or btcusdt |


**Response:**

```javascript
{
  "mins": 5,
  "price": "9.35751834"
}
```

#### 24hr ticker price change statistics

```shell
GET /openapi/quote/v1/ticker/24hr
```

24 hour price change statistics. **Careful** when accessing this with no symbol.

**Weight:**

| Parameter | Symbols Provided            | Weight |
| --------- | --------------------------- | ------ |
| symbol    | 1                           | 1      |
|           | symbol parameter is omitted | 40     |
| symbols   | 1-20                        | 1      |
|           | 21-100                      | 20     |
|           | 101 or more                 | 40     |
|           | symbol parameter is omitted | 40     |

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |Example: BTCUSDT
symbols | STRING | NO |

* Parameter symbol and symbols cannot be used in combination.If neither parameter is sent, tickers for all symbols will be returned in an array.Examples of accepted format for the symbols parameter: ["BTCUSDT","BNBUSDT"] and not case sensitive

**Response:**

```javascript
{
  "symbol": "BNBBTC",
  "priceChange": "-94.99999800",
  "priceChangePercent": "-95.960",
  "weightedAvgPrice": "0.29628482",
  "prevClosePrice": "0.10002000",
  "lastPrice": "4.00000200",
  "lastQty": "200.00000000",
  "bidPrice": "4.00000000",
  "bidQty": "100.00000000",
  "askPrice": "4.00000200",
  "askQty": "100.00000000",
  "openPrice": "99.00000000",
  "highPrice": "100.00000000",
  "lowPrice": "0.10000000",
  "volume": "8913.30000000",
  "quoteVolume": "15.30000000",
  "openTime": 1499783499040,
  "closeTime": 1499869899040,
  "firstId": 28385,   // first trade id
  "lastId": 28460,    // Last tradeId
  "count": 76         // Trade count
}

```

OR

```javascript
[
  {
    "symbol": "BNBBTC",
    "priceChange": "-94.99999800",
    "priceChangePercent": "-95.960",
    "weightedAvgPrice": "0.29628482",
    "prevClosePrice": "0.10002000",
    "lastPrice": "4.00000200",
    "lastQty": "200.00000000",
    "bidPrice": "4.00000000",
    "bidQty": "100.00000000",
    "askPrice": "4.00000200",
    "askQty": "100.00000000",
    "openPrice": "99.00000000",
    "highPrice": "100.00000000",
    "lowPrice": "0.10000000",
    "volume": "8913.30000000",
    "quoteVolume": "15.30000000",
    "openTime": 1499783499040,
    "closeTime": 1499869899040,
    "firstId": 28385,   // First tradeId
    "lastId": 28460,    // Last tradeId
    "count": 76         // Trade count
  }
]
```



#### Symbol price ticker

```shell
GET /openapi/quote/v1/ticker/price
```

Latest price for a symbol or symbols.

**Weight:**

| Parameter | Symbols Provided            | Weight |
| --------- | --------------------------- | ------ |
| symbol    | 1                           | 1      |
|           | symbol parameter is omitted | 2      |
| symbols   | Any                         | 2      |

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |Example: BTCUSDT
symbols | STRING | NO |

* Parameter symbol and symbols cannot be used in combination.If neither parameter is sent, prices for all symbols will be returned in an array.Examples of accepted format for the symbols parameter: ["BTCUSDT","BNBUSDT"] and not case sensitive

**Response:**

```javascript
{
  "symbol": "LTCBTC",
  "price": "4.00000200"
}
```

OR

```javascript
[
  {
    "symbol": "LTCBTC",
    "price": "4.00000200"
  },
  {
    "symbol": "ETHBTC",
    "price": "0.07946600"
  }
]
```



#### Symbol order book ticker

```shell
GET /openapi/quote/v1/ticker/bookTicker
```

Best price/qty on the order book for a symbol or symbols.

**Weight:**

| Parameter | Symbols Provided            | Weight |
| --------- | --------------------------- | ------ |
| symbol    | 1                           | 1      |
|           | symbol parameter is omitted | 2      |
| symbols   | Any                         | 2      |

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | NO |
symbols | STRING | NO |

* Parameter symbol and symbols cannot be used in combination.If neither parameter is sent, bookTickers for all symbols will be returned in an array.Examples of accepted format for the symbols parameter: ["BTCUSDT","BNBUSDT"] and not case sensitive

**Response:**

```javascript
{
  "symbol": "LTCBTC",
  "bidPrice": "4.00000000",
  "bidQty": "431.00000000",
  "askPrice": "4.00000200",
  "askQty": "9.00000000"
}
```

OR

```javascript
[
  {
    "symbol": "LTCBTC",
    "bidPrice": "4.00000000",
    "bidQty": "431.00000000",
    "askPrice": "4.00000200",
    "askQty": "9.00000000"
  },
  {
    "symbol": "ETHBTC",
    "bidPrice": "0.07946700",
    "bidQty": "9.00000000",
    "askPrice": "100000.00000000",
    "askQty": "1000.00000000"
  }
]
```


#### Cryptoasset trading pairs

```shell
GET /openapi/v1/pairs
```
a summary on cryptoasset trading pairs available on the exchange

**Weight:** 1

**Parameters:**

None

**Response:**

```javascript
[
  {
    "symbol": "LTCBTC",
    "quoteToken": "LTC",
    "baseToken": "BTC"
  },
  {
    "symbol": "BTCUSDT",
    "quoteToken": "BTC",
    "baseToken": "USDT"
  }
]
```



### Spot Trading Endpoints

#### Test new order (TRADE)

```shell
POST /openapi/v1/order/test (HMAC SHA256)
```

Test new order creation and signature/recvWindow long.
Creates and validates a new order but does not send it into the matching engine.

**Weight:** 1

**Parameters:**

Same as `POST /openapi/v1/order`

**Response:**

```javascript
{}
```



#### New order  (TRADE)

```shell
POST /openapi/v1/order  (HMAC SHA256)
```

Send in a new order.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
side | ENUM | YES |
type | ENUM | YES |
timeInForce | ENUM | NO |
quantity | DECIMAL | YES |
quoteOrderQty | DECIMAL | NO |
price | DECIMAL | NO |
newClientOrderId | STRING | NO | A unique id among open orders. Automatically generated if not sent. Orders with the same `newClientOrderID` can be accepted only when the previous one is filled, otherwise the order will be rejected.
stopPrice | DECIMAL | NO | Used with `STOP_LOSS`, `STOP_LOSS_LIMIT`, `TAKE_PROFIT`, and `TAKE_PROFIT_LIMIT` orders.
newOrderRespType | ENUM | NO | Set the response JSON. `ACK`, `RESULT`, or `FULL`; `MARKET` and `LIMIT` order types default to `FULL`, all other orders default to `ACK`.
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

Additional mandatory parameters based on `type`:

Type | Additional mandatory parameters                  | Additional Information
------------ |--------------------------------------------------| ------------ 
`LIMIT` | `timeInForce`, `quantity`, `price`               |
`MARKET` | `quantity` or `quoteOrderQty`                    | `MARKET` orders using `quantity` field specifies the amount of the base asset the user wants to sell, E.g. `MARKET` order on BCHUSDT  will specify how much BCH the user is selling. <br />`MARKET` orders using `quoteOrderQty` field specifies the amount of the quote asset the user wants to buy, E.g. `MARKET` order on BCHUSDT  will specify how much USDT the user is buying.<br /> **It's not supported to use the `quantity` for buying or use  the  `quoteOrderQty` for selling  for now.**
`STOP_LOSS` | `quantity` or `quoteOrderQty`, `stopPrice`       | This will execute a `MARKET` order when`stopPrice` is met. Use `quantity` for selling, `quoteOrderQty` for buying.
`STOP_LOSS_LIMIT` | `timeInForce`, `quantity`,  `price`, `stopPrice` | This will execute a `LIMIT` order when`stopPrice` is met.
`TAKE_PROFIT` | `quantity` or `quoteOrderQty`, `stopPrice`            | This will execute a `MARKET` order when`stopPrice` is met. Use `quantity` for selling, `quoteOrderQty` for buying.
`TAKE_PROFIT_LIMIT` | `timeInForce`, `quantity`, `price`, `stopPrice`  | This will execute a `LIMIT` order when`stopPrice` is met.
`LIMIT_MAKER` | `quantity`, `price`                              | This is a `LIMIT` order that will be rejected if the order immediately matches and trades as a taker.

Trigger order price rules against market price for both MARKET and LIMIT versions:

* Price above market price: `STOP_LOSS/STOP_LOSS_LIMIT` `BUY`, `TAKE_PROFIT/TAKE_PROFIT_LIMIT` `SELL`
* Price below market price: `STOP_LOSS/STOP_LOSS_LIMIT` `SELL`, `TAKE_PROFIT/TAKE_PROFIT_LIMIT` `BUY`

**Response ACK:**

```javascript
{
  "symbol": "BCHUSDT",
  "orderId": 1202289462787244800,
  "clientOrderId": "165806007267756",
  "transactTime": 1656900365976
}
```

**Response RESULT:**

```javascript
{
    "symbol": "BCHUSDT",
    "orderId": 1202289462787244800,
    "clientOrderId": "165806007267756",
    "transactTime": 1656900365976,
    "price": "1",
    "origQty": "101",
    "executedQty": "101",
    "cummulativeQuoteQty": "101",
    "status": "FILLED",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "side": "SELL",
    "stopPrice": "0",
    "origQuoteOrderQty": "0"
}
```
**Response FULL:**

```javascript
{
    "symbol": "BCHUSDT",
    "orderId": 1202289462787244800,
    "clientOrderId": "165806007267756",
    "transactTime": 1656900365976,
    "price": "1",
    "origQty": "101",
    "executedQty": "101",
    "cummulativeQuoteQty": "101",
    "status": "FILLED",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "side": "SELL",
    "stopPrice": "0",
    "origQuoteOrderQty": "0"
    "fills": [
        {
            "price": "2",
            "qty": "100",
            "commission": "0.01",
            "commissionAsset": "USDT",
            "tradeId": "1205027741844507648"
        },
        {
            "price": "1",
            "qty": "1",
            "commission": "0.005",
            "commissionAsset": "USDT",
            "tradeId": "1205027331347975169"
        }
    ]
}
```



#### Query order (USER_DATA)

```shell
GET /openapi/v1/order (HMAC SHA256)
```

Check an order's status.

**Weight:** 2

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
orderId | LONG | NO |
origClientOrderId | STRING | NO |
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

Notes:

* Either `orderId` or `origClientOrderId` must be sent. If both parameters are sent, `orderId` takes precedence.

**Response:**

```javascript
{
  "symbol": "LTCBTC",
  "orderId": 1202289462787244800,
  "clientOrderId": "165806007267756",
  "price": "0.1",
  "origQty": "1",
  "executedQty": "0",
  "cummulativeQuoteQty": "0",
  "status": "NEW",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "BUY",
  "stopPrice": "0",
  "time": 1499827319559,
  "updateTime": 1499827319559,
  "isWorking": true,
  "origQuoteOrderQty": "0"
}
```



#### Cancel order (TRADE)

```shell
DELETE /openapi/v1/order  (HMAC SHA256)
```

Cancel an active order.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
orderId | LONG | NO |
origClientOrderId | STRING | NO |
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

Notes:

* Either `orderId` or `origClientOrderId` must be sent. If both parameters are sent, `orderId` takes precedence.

**Response:**

```javascript
{
  "symbol": "BCHBUSD",
  "orderId": 1205324142243592448,
  "clientOrderId": "165830718862761",
  "price": "2",
  "origQty": "10",
  "executedQty": "8",
  "cummulativeQuoteQty": "16",
  "status": "CANCELED",
  "timeInForce": "GTC",
  "type": "LIMIT",
  "side": "SELL",
  "stopPrice": "0",
  "origQuoteOrderQty": "0"
}
```



#### Cancel All Open Orders on a Symbol (TRADE)

```shell
DELETE /openapi/v1/openOrders  (HMAC SHA256)
```

Cancels all active orders on a symbol.

**Weight:** 1

**Parameters:**

| Name       | Type   | Mandatory | Description                              |
| ---------- | ------ |-----------| ---------------------------------------- |
| symbol     | STRING | YES       |                                          |
| recvWindow | LONG   | NO        | The value cannot be greater than `60000` |
| timestamp  | LONG   | YES       |                                          |

**Response:**

```javascript
[
    {
        "symbol": "BTCUSDT",
        "orderId": 1200757068661824000,
        "clientOrderId": "165787739706155",
        "price": "19999",
        "origQty": "0.01",
        "executedQty": "0",
        "cummulativeQuoteQty": "0",
        "status": "CANCELED",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657877397079,
        "updateTime": 1657877397092,
        "isWorking": false,
        "origQuoteOrderQty": "0"
    },
    {
        "symbol": "BTCUSDT",
        "orderId": 1200760572449167872,
        "clientOrderId": "165787781474653",
        "price": "19999",
        "origQty": "0.01",
        "executedQty": "0",
        "cummulativeQuoteQty": "0",
        "status": "CANCELED",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657877814763,
        "updateTime": 1657877814776,
        "isWorking": false,
        "origQuoteOrderQty": "0"
    },
    {
        "symbol": "BTCUSDT",
        "orderId": 1200760629206489600,
        "clientOrderId": "165787782151456",
        "price": "19999",
        "origQty": "0.01",
        "executedQty": "0",
        "cummulativeQuoteQty": "0",
        "status": "CANCELED",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657877821529,
        "updateTime": 1657877821542,
        "isWorking": false,
        "origQuoteOrderQty": "0"
    }
]
```



#### Current open orders (USER_DATA)

```shell
GET /openapi/v1/openOrders  (HMAC SHA256)
```

GET all open orders on a symbol. **Careful** when accessing this with no symbol.

**Weight:** 3 for a single symbol; **40** when the symbol parameter is omitted;

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | String | NO |
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

**Response:**

```javascript
[
    {
        "symbol": "BTCUSDT",
        "orderId": 1200757068661824000,
        "clientOrderId": "165787739706155",
        "price": "19999",
        "origQty": "0.01",
        "executedQty": "0",
        "cummulativeQuoteQty": "0",
        "status": "NEW",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657877397079,
        "updateTime": 1657877397092,
        "isWorking": true,
        "origQuoteOrderQty": "0"
    },
    {
        "symbol": "BTCUSDT",
        "orderId": 1200760572449167872,
        "clientOrderId": "165787781474653",
        "price": "19999",
        "origQty": "0.01",
        "executedQty": "0",
        "cummulativeQuoteQty": "0",
        "status": "NEW",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657877814763,
        "updateTime": 1657877814776,
        "isWorking": true,
        "origQuoteOrderQty": "0"
    },
    {
        "symbol": "BTCUSDT",
        "orderId": 1200760629206489600,
        "clientOrderId": "165787782151456",
        "price": "19999",
        "origQty": "0.01",
        "executedQty": "0",
        "cummulativeQuoteQty": "0",
        "status": "NEW",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657877821529,
        "updateTime": 1657877821542,
        "isWorking": true,
        "origQuoteOrderQty": "0"
    }
]
```



#### History orders (USER_DATA)

```shell
GET /openapi/v1/historyOrders (HMAC SHA256)
```

GET all orders of the account;  canceled, filled or rejected.

**Weight:** 10 with symbol, **40** when the symbol parameter is omitted;

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ |-----------| ------------
symbol | String | YES       |
orderId | LONG | NO        |
startTime | LONG | NO        |
endTime | LONG | NO        |
limit | INT | NO        | Default 500; max 1000.
recvWindow | LONG | NO        |The value cannot be greater than `60000`
timestamp | LONG | YES       |

**Notes:**

* If `orderId` is set, it will get orders >= that `orderId`. Otherwise most recent orders are returned.

**Response:**

```javascript
[
    {
        "symbol": "BCHBUSD",
        "orderId": 1194453962386908672,
        "clientOrderId": "1657126007990",
        "price": "4.56",
        "origQty": "1",
        "executedQty": "1",
        "cummulativeQuoteQty": "4.56",
        "status": "FILLED",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "SELL",
        "stopPrice": "0",
        "time": 1657126008273,
        "updateTime": 1657126008357,
        "isWorking": false,
        "origQuoteOrderQty": "0"
    },
    {
        "symbol": "BCHBUSD",
        "orderId": 1194453774196830976,
        "clientOrderId": "165712598575253",
        "price": "0",
        "origQty": "0",
        "executedQty": "4",
        "cummulativeQuoteQty": "18",
        "status": "FILLED",
        "timeInForce": "GTC",
        "type": "MARKET",
        "side": "BUY",
        "stopPrice": "0",
        "time": 1657126008363,
        "updateTime": 1657126008402,
        "isWorking": false,
        "origQuoteOrderQty": "18"
    },
    {
        "symbol": "BCHBUSD",
        "orderId": 1194460299787314688,
        "clientOrderId": "1657126763487",
        "price": "0.46",
        "origQty": "1",
        "executedQty": "1",
        "cummulativeQuoteQty": "4.56",
        "status": "FILLED",
        "timeInForce": "GTC",
        "type": "LIMIT",
        "side": "SELL",
        "stopPrice": "0",
        "time": 1657126763736,
        "updateTime": 1657126763786,
        "isWorking": false,
        "origQuoteOrderQty": "0"
    }
]
```



#### Account information (USER_DATA)

```shell
GET /openapi/v1/account (HMAC SHA256)
```

GET current account information.

**Weight:** 10

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

**Response:**

```javascript
{
  "canTrade": true,       
  "canWithdraw": true,    
  "canDeposit": true,     
  "updateTime": 123456789,
  "accountType": "SPOT",
  "balances": [
    {
      "asset": "BTC",
      "free": "4723846.89208129",
      "locked": "0.00000000"
    },
    {
      "asset": "LTC",
      "free": "4763368.68006011",
      "locked": "0.00000000"
    }
  ]
}
```



#### Account trade list (USER_DATA)

```shell
GET /openapi/v1/myTrades  (HMAC SHA256)
```

Get trades for a specific account and symbol.

**Weight:** 10

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
orderId | LONG | NO | This can only be used in combination with `symbol`.
startTime | LONG | NO |
endTime | LONG | NO |
fromId | LONG | NO | TradeId to fetch from. Default gets most recent trades.
limit | INT | NO | Default 500; max 1000.
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

**Notes:**

*  If fromId (tradeId) is set, it will get id (tradeId) >= that fromId (tradeId). Otherwise most recent trades are returned.

**Response:**

```javascript
[
  {
    "symbol": "BNBBTC",
    "id": 1194460299787317856,
    "orderId": 1194453774196830977,
    "price": "4.00000100",
    "qty": "12.00000000",
    "quoteQty": "48.000012",
    "commission": "10.10000000",
    "commissionAsset": "BNB",
    "time": 1499865549590,
    "isBuyer": true,
    "isMaker": false,
    "isBestMatch": true
  }
]
```



#### withdraw (to coins_ph account) (USER_DATA)

```shell
POST /openapi/v1/capital/withdraw/apply
```

withdraw

**Weight:** 1

**Parameters:**

Name                        | Type           | Mandatory | Description
---------------------------|----------------|-----------|-------------------------------------
coin                      | string         | YES       |
amount                    | decimal/string | YES       |
withdrawOrderId           | string         | NO        | client order id
recvWindow | LONG           | NO        |The value cannot be greater than `60000`
timestamp | LONG           | YES       |

**Response:**

```javascript
{
    "id": "1201515362324421632" -- order id
}
```

#### deposit (to xyz account) (USER_DATA)

```shell
POST /openapi/v1/capital/deposit/apply
```

deposit

**Weight:** 1

**Parameters:**

Name                        | Type           | Mandatory | Description
---------------------------|----------------|-----------|-------------------------------------
coin                      | string         | YES       |
amount                    | decimal/string | YES       |
depositOrderId           | string         | NO        | client order id
recvWindow | LONG           | NO        |The value cannot be greater than `60000`
timestamp | LONG           | YES       |

**Response:**

```javascript
{
    "id": "1201515362324421632" -- order id
}
```

#### Deposit order history(deposit order which deposit from coins_ph to exchange) (USER_DATA)

```shell
GET /openapi/v1/capital/deposit/history
```

**Weight:** 1

**Parameters:**

| Name           | Type   | Mandatory | Description                                                                                                                                   |
| -------------- | ------ | --------- |-----------------------------------------------------------------------------------------------------------------------------------------------|
| coin           | string | NO        |                                                                                                                                               |
| depositOrderId | string | NO        | client order id                                                                                                                               |
| status         | int    | NO        | 0:Init,1:Cancelled 2:Awaiting Approval 3:Rejected 4:Processing 5:Failure 6:Completed; default:0                                               |
| offset         | int    | NO        | Default is 0, means get the order history records from offset. For example, you can get the deposite history from third record with offset 3. |
| limit          | int    | NO        | query limit max 1000, default 1000                                                                                                            |
| startTime      | Long   | NO        | Get the deposit history records from startTime                                                                                                |
| endTime        | Long   | NO        | Get the deposit history records to endTime                                                                                                    |
| recvWindow     | LONG   | NO        | The value cannot be greater than `60000`                                                                                                      |
| timestamp      | LONG   | YES       |                                                                                                                                               |

**notes**

* Please notice the default startTime and endTime to make sure that time interval is within 0-90 days.
* If both startTime and endTime are sent, time between startTime and endTime must be less than 90 days.

  **Response:**

```javascript
[
    {
        "coin": "PHP",
        "address": "Internal Transfer",
        "addressTag": "Internal Transfer",
        "amount": "0.02",
        "id": "31312321312312312312322",
        "network": "Internal",
        "transferType": "0",
        "status": 3,
        "confirmTimes": "",
        "unlockConfirm": "",
        "txId": "Internal Transfer",
        "insertTime": 1657623798000,
        "depositOrderId": "the deposit id which created by client"
    }
]
```



#### Withdraw order history (withdrawal order which withdraw from exchange to coins_ph) (USER_DATA)

```shell
GET /openapi/v1/capital/withdraw/history
```

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
coin            | string | NO        |
withdrawOrderId | string | NO        | client order id
status          | int    | NO        | 0:Init,1:Cancelled 2:Awaiting Approval 3:Rejected 4:Processing 5:Failure 6:Completed; default: 0
offset          | int    | NO        | default 0,means get the order history records from offset. For example, you can get the deposite history from third record with offset 3. Application will try to get all the orders with default.
limit           | int    | NO        | query limit, default 1000, max 1000
startTime       | Long   | NO        | Get the withdraw history records from startTime
endTime          | Long   | NO        | Get the withdraw history records to endTime
recvWindow | LONG   | NO        |The value cannot be greater than `60000`
timestamp | LONG   | YES       |

* Please notice the default startTime and endTime to make sure that time interval is within 0-90 days.
* If both startTime and endTimeare sent, time between startTimeand endTimemust be less than 90 days.
* If withdrawOrderId is sent, time between startTime and endTime must be less than 7 days.
* If withdrawOrderId is sent, startTime and endTime are not sent, will return last 7 days records by default.

**Response:**
**Response:**
```javascript
[
    {
        "coin": "BTC",
        "address": "Internal Transfer",
        "amount": "0.1",
        "id": "1201515362324421632",
        "withdrawOrderId": null,
        "network": "Internal",
        "transferType": "0",
        "status": 0,
        "transactionFee": "0",
        "confirmNo": 0,
        "info": "{}",
        "txId": "Internal Transfer",
        "applyTime": 1657967792000
    }
]
```



#### Trade Fee (USER_DATA)

```shell
GET /openapi/v1/asset/tradeFee (HMAC SHA256)
```

Fetch trade fee

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
symbol            | STRING | NO        |
recvWindow | LONG   | NO        | The value cannot be greater than `60000`
timestamp          | LONG   | YES        |

**Response:**

```javascript
  [
    {
      "symbol": "BTCUSDT",
      "makerCommission": "0.002",
      "takerCommission": "0.003"
    },
    {
      "symbol": "ETHUSDT",
      "makerCommission": "0.001",
      "takerCommission": "0.001"
    }
  ]
```



### User data stream endpoints

Specifics on how user data streams work is in another document(user-data-stream.md).



#### Start user data stream (USER_STREAM)

```shell
POST /openapi/v1/userDataStream
```

Start a new user data stream. The stream will close after 60 minutes unless a keepalive is sent.

**Weight:** 1

**Parameters:**

None

**Response:**

```javascript
{
  "listenKey": "xDqtskqOciCzRashthgjTHBcymasBBShEEzPiXgOGEujviYWCuyYwcPDVPeezJOT"
}
```



#### Keepalive user data stream (USER_STREAM)

```shell
PUT /openapi/v1/userDataStream
```

Keepalive a user data stream to prevent a time out. User data streams will close after 60 minutes. It's recommended to send a ping about every 30 minutes.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES |

**Response:**

```javascript
{}
```



#### Close user data stream (USER_STREAM)

```shell
DELETE /openapi/v1/userDataStream
```

Close out a user data stream.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
listenKey | STRING | YES |

**Response:**

```javascript
{}
```


### Convert endpoints

#### Get supported trading pairs
```shell
POST /openapi/convert/v1/get-supported-trading-pairs
```

This continuously updated endpoint returns a list of all available trading pairs. The response includes information on the minimum and maximum amounts that can be traded for the source currency, as well as the level of precision in decimal places used for the source currency.
**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------|------| ------------ | ------------
|      |      |

**Response:**

```javascript
{
  "status":"Success",
  "error":"OK",
  "data":[
     {
      "sourceCurrency":"PHP",
      "targetCurrency":"BTC",
      "minSourceAmount":"1000",
      "maxSourceAmount":"15000",
      "precision":"2"
    },
    {
      "sourceCurrency":"BTC",
      "targetCurrency":"PHP",
      "minSourceAmount":"0.0001",
      "maxSourceAmount":"0.1",
      "precision":"8"
    },
    {
      "sourceCurrency":"PHP",
      "targetCurrency":"ETH",
      "minSourceAmount":"1000",
      "maxSourceAmount":"18000",
      "precision":"2"
    },
    {
      "sourceCurrency":"ETH",
      "targetCurrency":"PHP",
      "minSourceAmount":"0.003",
      "maxSourceAmount":"4.2",
      "precision":"8"
    }
  ]
}
```



#### Fetch a quote

```shell
POST /openapi/convert/v1/get-quote
```

This endpoint returns a quote for a specified source currency (sourceCurrency) and target currency (targetCurrency) pair.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
sourceCurrency | STRING | YES |The currency the user holds
targetCurrency | STRING | YES |The currency the user would like to obtain
sourceAmount | STRING | YES |The amount of sourceCurrency

**Response:**

```javascript
{
  "status": 0, 
  "error": "OK", 
  "data": {
            "quoteId": "2182b4fc18ff4556a18332245dba75ea",
            "sourceCurrency": "BTC",
            "targetCurrency": "PHP",
            "sourceAmount": "0.1",
            "price": "59999",             //1BTC=59999PHP
            "targetAmount": "5999",       //The amount of PHP the user holds
            "expiry": "10"
  }
}
```

#### Accept the quote


```shell
POST /openapi/convert/v1/accpet-quote
```

Use this endpoint to accept the quote and receive the result instantly.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
quoteId | STRING | YES |The ID assigned to the quote


**Response:**

```javascript
{
  "status": 0, 
  "data": {
         "orderId" : "49d10b74c60a475298c6bbed08dd58fa",
         "status": "SUCCESS"
  },
  "error": "ok"
}
```

#### Retrieve order history


```shell
POST /openapi/convert/v1/query-order-history
```
This endpoint retrieves order history with the option to define a specific time period using start and end times.

**Weight:** 1

**Parameters:**

Name | Type   | Mandatory | Description
------------ |--------|---------| ------------
startTime | STRING | No |The starting point of the required period. If no period is defined, the entire order history is returned.
endTime | STRING | No |The end point of the required period. If no period is defined, the entire order history is returned.
page | int    | No |
size | int    | No |


**Response:**

```javascript
{
  "status": 0,
   "error": "OK",
   "data": [
    {
      "orderId": "49d10b74c60a475298c6bbed08dd58fa",
      "quoteId": "cfbe49acf56b43a698d99ca470658a5c",
      "sourceCurrency": "BTC",
      "targetCurrency": "PHP",
      "sourceAmount": "0.00014252",
      "targetAmount": "131.1432432",
      "price": "920174.31378052",
      "fee": "0",
      "status": "SUCCESS",
      "createdAt": "1672283052000",
      "errorCode": "",
      "errorMessage": ""
    },
    {
      "orderId": "ad3bb743e60747a8a57c2d33317c3149",
      "quoteId": "5217a22fdbf044ecaf147fb99bc00be6",
      "sourceCurrency": "ETH",
      "targetCurrency": "BTC",
      "sourceAmount": "0.01583701",
      "targetAmount": "0.024643",
      "price": "1.55603867",
      "fee": "0",
      "status": "FAILED",
      "createdAt": "1672025632000",
      "errorCode": "",
      "errorMessage": ""
    },
    {
      "orderId": "49678eef7b3b4003ab3552141bb2d2dd",
      "quoteId": "ac04e42b37fd439cb06fb2d8b5b21559",
      "sourceCurrency": "ETH",
      "targetCurrency": "BTC",
      "sourceAmount": "0.01584514",
      "targetAmount": "0.024643",
      "price": "1.55524028",
      "fee": "0",
      "status": "FAILED",
      "errorMessage" : "Insufficient balance",
      "createdAt": "1672025122000",
      "errorCode": "",
      "errorMessage": ""
    },
    {
      "orderId": "69e4b2d166f24155b9f2221999a5271c",
      "quoteId": "671f88b5330043949f6dfda222bcd5f5",
      "sourceCurrency": "PHP",
      "targetCurrency": "XRP",
      "sourceAmount": "43.46",
      "targetAmount": "1",
      "price": "0.02300966",
      "fee": "0",
      "status": "SUCCESS",
      "createdAt": "1671800356000",
      "errorCode": "",
      "errorMessage": ""
    },
    {
      "orderId": "25a9b92bcd4d4b2598c8be97bc65b466",
      "quoteId": "1ecce9a7265a4a329cce80de46e2c583",
      "sourceCurrency": "BTC",
      "targetCurrency": "PHP",
      "sourceAmount": "0.11",
      "targetAmount": "4466.89275956",
      "price": "40608.115996",
      "fee": "0",
      "status": "SUCCESS",
      "createdAt": "1671797993000",
      "errorCode": "",
      "errorMessage": ""
    }
  ],
  "total": 23
}
```

### Fiat endpoints

#### Get supported fiat channels
```shell
GET openapi/fiat/v1/support-channel
```

This continuously updated endpoint returns a list of all available fiat channels. 
**Weight:** 1

**Parameters:**

 Name            | Type   | Mandatory | Description
-----------------|--------| ------------ | ------------
| transactionType | STRING | Yes |Set this parameter to -1 to indicate a cash-out transaction. At present, only cash-out transactions are supported.
| currency        | STRING | Yes |The parameter represents the currency used in the transaction and should be set to PHP as it is the only currency currently supported.


**Response:**

```javascript
{
  "status":0,
  "error":"OK",
  "data":
   [
    {
      "id": 782,
      "transactionChannel": "SWIFTPAY_PESONET",
      "transactionChannelName": "swiftpay_pesonet",
      "transactionSubject": "alamanah",
      "transactionSubjectType": "bank",
      "transactionSubjectTypeLabel": "Banks",
      "transactionSubjectName": "Al-Amanah Islamic Invest. Bank",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/PesoNet@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/Al-Amanah+Islamic+Bank.png",
      "maximum": "100000000",
      "minimum": "5",
      "dailyLimit": "50",
      "monthlyLimit": "10000001",
      "annualLimit": "500000",
      "remainingDailyLimit": "50",
      "remainingMonthlyLimit": "10000001",
      "remainingAnnualLimit": "500000",
      "precision": "2",
      "fee": "5",
      "feeType": "fixed",
      "status": "1",
      "maxWithdrawBalance": "10000037084.37574788"
    },
    {
      "id": 622,
      "transactionChannel": "INSTAPAY",
      "transactionChannelName": "instapay",
      "transactionSubject": "allbank",
      "transactionSubjectType": "bank",
      "transactionSubjectTypeLabel": "Banks",
      "transactionSubjectName": "AllBank (A Thrift Bank), Inc.",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/InstaPay@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/AllBank.png",
      "maximum": "50000",
      "minimum": "50",
      "dailyLimit": "100000",
      "monthlyLimit": "100000",
      "annualLimit": "1000000",
      "remainingDailyLimit": "100000",
      "remainingMonthlyLimit": "100000",
      "remainingAnnualLimit": "999656",
      "precision": "2",
      "fee": "10",
      "feeType": "fixed",
      "status": "1",
      "maxWithdrawBalance": "10000037084.37574788"
    },
    {
      "id": 783,
      "transactionChannel": "SWIFTPAY_PESONET",
      "transactionChannelName": "swiftpay_pesonet",
      "transactionSubject": "allbank",
      "transactionSubjectType": "bank",
      "transactionSubjectTypeLabel": "Banks",
      "transactionSubjectName": "AllBank (A Thrift Bank), Inc.",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/PesoNet@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/AllBank.png",
      "maximum": "100000000",
      "minimum": "5",
      "dailyLimit": "50",
      "monthlyLimit": "10000001",
      "annualLimit": "500000",
      "remainingDailyLimit": "50",
      "remainingMonthlyLimit": "10000001",
      "remainingAnnualLimit": "500000",
      "precision": "2",
      "fee": "5",
      "feeType": "fixed",
      "status": "0",
      "maxWithdrawBalance": "10000037084.37574788"
    },
    {
      "id": 785,
      "transactionChannel": "SWIFTPAY_PESONET",
      "transactionChannelName": "swiftpay_pesonet",
      "transactionSubject": "anz",
      "transactionSubjectType": "bank",
      "transactionSubjectTypeLabel": "Banks",
      "transactionSubjectName": "ANZ Banking Group Ltd",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/PesoNet@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/Australia+&+New+Zealand+Bank.png",
      "maximum": "100000000",
      "minimum": "5",
      "dailyLimit": "50",
      "monthlyLimit": "10000001",
      "annualLimit": "500000",
      "remainingDailyLimit": "50",
      "remainingMonthlyLimit": "10000001",
      "remainingAnnualLimit": "500000",
      "precision": "2",
      "fee": "5",
      "feeType": "fixed",
      "status": "1",
      "maxWithdrawBalance": "10000037084.37574788"
    },
    {
      "id": 647,
      "transactionChannel": "INSTAPAY",
      "transactionChannelName": "instapay",
      "transactionSubject": "guinobatan",
      "transactionSubjectType": "bank",
      "transactionSubjectTypeLabel": "Banks",
      "transactionSubjectName": "Asenso",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/InstaPay@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/Asenso.png",
      "maximum": "50000",
      "minimum": "50",
      "dailyLimit": "100000",
      "monthlyLimit": "100000",
      "annualLimit": "1000000",
      "remainingDailyLimit": "100000",
      "remainingMonthlyLimit": "100000",
      "remainingAnnualLimit": "999656",
      "precision": "2",
      "fee": "10",
      "feeType": "fixed",
      "status": "1",
      "maxWithdrawBalance": "10000037084.37574788"
    },
    {
      "id": 837,
      "transactionChannel": "SWIFTPAY_PESONET",
      "transactionChannelName": "swiftpay_pesonet",
      "transactionSubject": "guinobatan",
      "transactionSubjectType": "bank",
      "transactionSubjectTypeLabel": "Banks",
      "transactionSubjectName": "Asenso",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/PesoNet@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/Asenso.png",
      "maximum": "100000000",
      "minimum": "5",
      "dailyLimit": "50",
      "monthlyLimit": "10000001",
      "annualLimit": "500000",
      "remainingDailyLimit": "50",
      "remainingMonthlyLimit": "10000001",
      "remainingAnnualLimit": "500000",
      "precision": "2",
      "fee": "5",
      "feeType": "fixed",
      "status": "1",
      "maxWithdrawBalance": "10000037084.37574788"
    },
    {
      "id": 623,
      "transactionChannel": "INSTAPAY",
      "transactionChannelName": "instapay",
      "transactionSubject": "aub",
      "transactionSubjectType": "e-wallet",
      "transactionSubjectTypeLabel": "E-wallets",
      "transactionSubjectName": "Asia United Bank",
      "transactionType": -1,
      "paymentMethod": "BANK_TRANSFER",
      "channelIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/InstaPay+Pesonet+copy/InstaPay@136.png",
      "subjectIcon": "https://static.pro.coins.xyz/resource/images/fiat/fiat-png/AUB.png",
      "maximum": "50000",
      "minimum": "50",
      "dailyLimit": "100000",
      "monthlyLimit": "100000",
      "annualLimit": "1000000",
      "remainingDailyLimit": "100000",
      "remainingMonthlyLimit": "100000",
      "remainingAnnualLimit": "999656",
      "precision": "2",
      "fee": "10",
      "feeType": "fixed",
      "status": "1",
      "maxWithdrawBalance": "10000037084.37574788"
    }
  ]
}
```


#### Cash out
```shell
GET openapi/fiat/v1/cash-out
```

This endpoint allows users to withdraw funds from their fiat account.

**Weight:** 1

**Parameters:**

 Name            | Type   | Mandatory | Description
-----------------|--------| ------------ | ------------
| internalOrderId | STRING | Yes | Internal ID assigned to the funds withdrawal order.
| currency        | STRING | Yes | The parameter represents the currency used in the transaction and should be set to PHP as it is the only currency currently supported.
| amount          | STRING | Yes | The amount of currency to be withdrawn.
| channelName     | STRING | Yes | The payment channel or method that the user wishes to use for the cash-out transaction.
| channelSubject  | STRING | Yes | Additional information about the payment channel or method that the user wishes to use for the cash-out transaction.


**Response:**

```javascript
{
  "status": 0,
  "error": "OK", 
  "data": {
         "externalOrderId": "1380692028693995623",
         "internalOrderId": "1388420429697583896",
          },
  "params": null
}
```

#### Fiat trade history
```shell
POST openapi/fiat/v1/history
```

This endpoint fetches a comprehensive record of the user’s fiat trading activity. The response contains detailed information about the past trades, including the transaction date, transaction amount, and any associated fees. The user can also specify a date range for retrieving transaction data.
**Weight:** 1

**Parameters:**

Name            | Type    | Mandatory | Description
-----------------|---------|-----------| ------------
| startDate | Date    | No        | Represents the start of the date range and specifies the earliest date for which transaction data should be retrieved. Transactions that occurred on or after the startDate will be included in the results.
| endDate   | Date    | No        | Represents the end of the date range and specifies the latest date for which transaction data should be retrieved. Transactions that occurred before or on the endDate will be included in the results.


**Response:**

```javascript
{
  "status":0,
  "error":"OK",
  "data":[
    {
      "externalOrderId":"1380692028693995750",
      "internalOrderId":"1392039268679225857",
      "paymentOrderId":"502864",
      "fiatCurrency":"PHP",
      "fiatAmount":"13",
      "transactionType":-1,
      "transactionChannel":"INSTAPAY_QR",
      "transactionSubject":"pnb",
      "transactionChannelName":"instapay_qr",
      "transactionSubjectName":"PNB",
      "transactionSubjectType":"bank",
      "feeCurrency":"PHP",
      "channelFee":"0",
      "platformFee":"0",
      "status":"PENDING",
      "errorCode":"",
      "errorMessage":"",
      "completedTime":"2023-04-05T07:33:35.000+00:00",
      "source":"WEB",
      "createdAt":"2023-04-05T07:33:35.000+00:00",
      "orderExtendedMap":{
        "amount":"13",
        "tfrName":"est registration",
        "tfrAcctNo":"111111111222111",
        "channelName":"INSTAPAY_QR",
        "currency":"PHP",
        "source":"WEB",
        "channelSubject":"pnb",
        "userId":1198653352181844736,
        "orgId":9001
      },
      "dealCancel":false
    },
    {
      "externalOrderId":"1380692028693995749",
      "internalOrderId":"1392011368101908738",
      "paymentOrderId":"356142",
      "fiatCurrency":"PHP",
      "fiatAmount":"13",
      "transactionType":-1,
      "transactionChannel":"INSTAPAY_QR",
      "transactionSubject":"pnb",
      "transactionChannelName":"instapay_qr",
      "transactionSubjectName":"PNB",
      "transactionSubjectType":"bank",
      "feeCurrency":"PHP",
      "channelFee":"0",
      "platformFee":"0",
      "status":"PENDING",
      "errorCode":"",
      "errorMessage":"JWT signature does not match locally computed signature. JWT validity cannot be asserted and should not be trusted.",
      "completedTime":"2023-04-05T06:38:09.000+00:00",
      "source":"WEB",
      "createdAt":"2023-04-05T06:38:09.000+00:00",
      "channelSubject":"pnb",
      "userId":1198653352181844736,
      "orgId":9001
        },
      "dealCancel":false
     },
     {
        "externalOrderId":"1380596328526306871",
        "internalOrderId":"1380596591861487948",
        "paymentOrderId":"687428",
        "fiatCurrency":"PHP",
        "fiatAmount":"45",
        "transactionType":-1,
        "transactionChannel":"INSTAPAY",
        "transactionSubject":"coins.ph",
        "transactionChannelName":"instapay",
        "transactionSubjectName":"DCPAY PHILIPPINES, INC.",
        "transactionSubjectType":"bank",
        "feeCurrency":"PHP",
        "channelFee":"0",
        "platformFee":"10",
        "status":"SUCCEEDED",
        "errorCode":"",
        "errorMessage":"",
        "completedTime":"2023-03-20T12:39:03.000+00:00",
        "source":"WEB",
        "createdAt":"2023-03-20T12:39:00.000+00:00",
        "channelSubject":"coins.ph",
        "userId":1198653352181844736,
        "orgId":9001
     },
  "dealCancel":false
 }
],
"total":3
} 
```


#### Fiat order detail
```shell
GET openapi/fiat/v1/details
```

This endpoint retrieves information about a specific fiat currency order. The response provides detailed information about the respective trade, including the transaction date, transaction amount, and any associated fees.

**Weight:** 1

**Parameters:**

Name            | Type   | Mandatory | Description
-----------------|--------| ------------ | ------------
| internalOrderId | STRING | Yes | ID of the order for which the user wishes to retrieve details.

**Response:**

```javascript
{
  "status": 0,
  "error": "OK",
  "data": {
            "id": "1380692028693995623",
            "orderId": "1388420429697583896",
            "paymentOrderId": "455628",
            "fiatCurrency": "PHP",
            "fiatAmount": "60",
            "transactionType": -1,
            "transactionChannel": "INSTAPAY",
            "transactionSubject": "coins.ph",
            "transactionSubjectType": "bank",
            "transactionChannelName": "instapay",
            "transactionSubjectName": "DCPAY PHILIPPINES, INC.",
            "feeCurrency": "PHP",
            "channelFee": "0",
            "platformFee": "10",
            "status": "SUCCEEDED",
            "errorCode": "",
            "errorMessage": "",
            "completedTime": "2023-03-31T07:44:42.000+00:00",
            "source": "WEB",
            "createdAt": "2023-03-31T07:43:37.000+00:00",
            "orderExtendedMap": {
             "amount": "50",
              "sendAcctNo": "1326887193441018369",
              "tfrName": "XUAN sissisissi sissi CHEN6",
              "tfrAcctNo": "447415103851",
              "channelName": "INSTAPAY",
              "currency": "PHP",
              "source": "WEB",
              "channelSubject": "coins.ph",
              "userId": 1326887193348759809,
              "orgId": 9001
    },
    "dealCancel": false
  }
}
```
------
### Old endpoints from coins.ph (Legacy)

#### Create a new sellorder
```shell
POST openapi/migration/v4/sellorder
```

This endpoint converts digital assets into real-world cash, making it easy for users to withdraw their funds as cash. It provides a streamlined and secure process for cashing out, ensuring that users can access their funds quickly and easily. Additionally, this API endpoint ensures that users' financial information is kept secure and confidential, providing a safe and reliable way to withdraw funds.

 Name           | Type   | Mandatory | Description
----------------|--------|-----------| ------------
| payment_outlet | STRING | Yes       | The payment outlet used to transfer funds to another Coins wallet. Once the sell order is completed, the fiat cashout will be processed through the chosen payout outlet and the funds will be transferred to the specified destination wallet via coins_transfer.
| currency       | STRING | Yes       |  The currency symbol of the sell order.
| amount         | STRING | Yes       | The quantity of digital assets that the user wishes to sell and the corresponding amount of fiat cash that they will receive in exchange.
| id             | STRING | No        |  A unique identifier for the sell order
| pay_with_wallet             | STRING | Yes       | The wallet code from which the user wishes to initiate the sell order.
| bank_account_number             | STRING | Yes       | Cash out bank account number
| bank_account_name             | STRING | Yes       | Cash out bank account bane
| recipient_phone_number             | STRING | Yes       | Recipient phone number
| recipient_bank_code             | STRING | Yes       | Recipient bank code


**Weight:** 1

 reference: https://docs.coins.asia/reference/sellorder-create

------

#### Retrieve an existing sellorder
```shell
GET openapi/migration/v4/sellorder/{sell_order_id}
```

This endpoint retrieves information about a previously executed sell order that involved a cash-out of fiat currency.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------| ------------ | ------------
| sell_order_id       | STRING | No |  A unique identifier that is used to specify the sell order that a user wants to retrieve information about.


**Weight:** 1

reference: https://docs.coins.asia/reference/sellorder-retrieve

------

#### Validate field values
```shell
GET openapi/migration/v4/validate-field
```

This endpoint validates field values for fiat sell order to ensure that the values provided for the different fields are valid before the actual cashout process begins.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------| ------------ | ------------
| field_type       | STRING | No |  The type of field that needs to be validated. Accepts two valid values: "account_number" and "mobile_number"..
| account_number       | STRING | No |  When the field_type parameter is set to "account_number", this parameter is used to specify the account number that needs to be validated for the fiat sell order.
| account_type       | STRING | No | When the field_type parameter is set to "account_number", this parameter is used to specify the payment outlet ID where the account number field value should be validated against.
| mobile_number       | STRING | No | When the field_type parameter is set to "mobile_number", this parameter is used to specify the mobile number that needs to be validated for the fiat sell order. The mobile number provided must be in the correct format, following the E.164 phone number formatting, and associated with the correct user.
| region       | STRING | No |  When the field_type parameter is set to "mobile_number", this parameter is used to specify the region of the mobile number that needs to be validated for the fiat sell order. Valid values for the region parameter are "PH" and "TH", which represent the regions of the Philippines and Thailand, respectively.


**Weight:** 1

reference: https://docs.coins.asia/reference/validate-field

------

#### Retrieve supported payout-outlets
```shell
GET openapi/migration/v4/payout-outlets/{id}
```

This endpoint retrieves a list of supported payout outlets for sell orders. It is designed to provide users with a comprehensive list of available payout options so they can select the one that is most convenient for them.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------|-----------| ------------
| outlet_category       | STRING | No       |  A payment outlet category ID that is used to filter the list of supported payout outlets.
| name       | STRING | No       | A payment outlet name that is used to filter the list of supported payout outlets.
| region       | STRING | No       |The name of a region that is used to filter the list of supported payout outlets.
| is_enabled       | STRING | No     | A boolean value that determines whether or not to include disabled payout outlets in the results.


**Weight:** 1

reference: https://docs.coins.asia/reference/payout-outlets

------

#### Retrieve supported payout-outlet-categories
```shell
GET openapi/migration/v4/payout-outlet-categories/{id}
```

This endpoint retrieves the list of payout outlet categories that are supported for fiat sell orders. Payout outlet categories are used to classify and organize the different payout outlet options that are available to users when they choose to cash out their fiat currency. This endpoint takes no parameters and returns a JSON response that includes an array of payout outlet categories.
**Weight:** 1

reference: https://docs.coins.asia/reference/payout-outlet-categories


------

#### Retrieve current payout-outlet-fees
```shell
GET openapi/migration/v4/payout-outlet-fees
```

This endpoint retrieves the current payout outlet fees for the supported payout outlets for fiat sell orders. Payout outlet fees are the fees charged by the payout outlet providers for processing the cash-out transactions. This endpoint takes no parameters and returns a JSON response that includes an array of payout outlet fees.


**Weight:** 1

reference: https://docs.coins.asia/reference/payout-outlet-fees

