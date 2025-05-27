---
title: "Rest-Api"
permalink: /rest-api/
layout: default
nav: sidebar/rest-api.html

---


# Change log:

2025-05-21: Added the `statuses` parameter to the `/openapi/wallet/v1/deposit/history` endpoint.

2025-04-23: Added the `/openapi/v1/fund-collect/collect-from-sub-account`,`/openapi/v1/fund-collect/get-fund-record` endpoint.

2025-04-03: Updated status parameter descriptions for endpoints: `openapi/fiat/v1/details`, `openapi/fiat/v1/history`, `openapi/fiat/v2/history`.

2025-03-25: Added the `/openapi/v1/asset/transaction/history` endpoint.

2025-01-02: Added the `/openapi/fiat/v2/history` endpoint.

2024-10-11: Added the `/openapi/v1/sub-account/wallet/deposit/address`,`/openapi/v1/sub-account/wallet/deposit/history` endpoint.

2024-08-26: Added the `startTime` `endTime` parameter to the `openapi/fiat/v1/history` endpoint.

2024-05-10: Added the `from_address` `to_address` parameter to the `/openapi/transfer/v3/transfers` endpoint.

2024-04-29: Added the `inversePrice` response parameter to the `/openapi/convert/query-order-history` endpoint.

2024-04-24: Added <a href="#sub-account-endpoints">Sub-account</a> endpoints: `/openapi/v1/sub-account/list`, `/openapi/v1/sub-account/create`, `/openapi/v1/sub-account/asset`, `/openapi/v1/sub-account/transfer/universal-transfer`, `/openapi/v1/sub-account/transfer/sub-to-master`, `/openapi/v1/sub-account/transfer/universal-transfer-history`, `/openapi/v1/sub-account/transfer/sub-history`, `/openapi/v1/sub-account/apikey/ip-restriction`, `/openapi/v1/sub-account/apikey/add-ip-restriction`, `/openapi/v1/sub-account/apikey/delete-ip-restriction`.

2024-04-17: Added the `targetAmount` parameter to the `/openapi/convert/v1/get-quote` endpoint.

2024-02-19: Added the `openapi/v1/user/ip` endpoint.

2023-12-29: Added kyc remaining and limit to the `/openapi/v1/account` endpoint.

2023-12-06: Added the `internalOrderId` generate rule description to the `/openapi/fiat/v1/cash-out` endpoint.

2023-09-20: Added the `message` parameter to the `/openapi/transfer/v3/transfers` endpoint.

2023-08-30: Added `/openapi/convert/v1/query-order-history`, and updated`openapi/fiat/v1/cash-out`, `openapi/fiat/v1/support-channel` docs.

2023-08-17: Updated `/openapi/convert/v1/accept-quote`, `openapi/fiat/v1/history`, `openapi/fiat/v1/cash-out`, `openapi/fiat/v1/support-channel`, `openapi/migration/v4/sellorder`, `openapi/migration/v4/validate-field`, `openapi/migration/v4/payout-outlets/{id}`, `openapi/migration/v4/payout-outlet-categories/{id}`, `openapi/migration/v4/payout-outlet-fees` docs.

2023-08-08: Updated `openapi/fiat/v1/history`, `openapi/fiat/v1/cash-out`, `openapi/fiat/v1/support-channel`, `openapi/migration/v4/sellorder`, `openapi/migration/v4/validate-field`, `openapi/migration/v4/payout-outlets/{id}`, `openapi/migration/v4/payout-outlet-categories/{id}`, `openapi/migration/v4/payout-outlet-fees` docs.  

2023-07-31: Added `openapi/fiat/v1/history` endpoint to query fiat currency order history 

2023-07-15: Updated `MARKET order type now supports quantity for buy and quoteOrderQty for sell` 

2023-07-15: Added  `stpFlag` in the request of New order (TRADE) endpoint for anti self-trading behaviour.

2023-07-15: Added order status `EXPIRED`.

2023-06-08: Added the `payment request` interface.

2023-05-17: The disclaimer regarding the following endpoints being in the QA phase has been removed as the QA process has been successfully completed: `/openapi/account/v3/crypto-accounts`, `/openapi/transfer/v3/transfers`, and `/openapi/transfer/v3/transfers/{id}`.

2023-05-08: Added the following endpoints: `/openapi/account/v3/crypto-accounts`, `/openapi/transfer/v3/transfers`, and `/openapi/transfer/v3/transfers/{id}`. The endpoints are still in QA and are appropriately marked as such.

2023-05-04: Removed the endpoints `/openapi/convert/v1/query-order-history` and `openapi/fiat/v1/history`; removed a personal detail from a code sample in `/openapi/fiat/v1/details`. 

2023-04-13: Added the `invoice` interface.

2023-04-10: Added the `transfer` interfaces.

2022-09-12: Modified the `symbol` in the `Cancel All Open Orders on a Symbol` API request as required.

2022-09-09: Changed the `orderId/transactTime/time/updateTime` response from string to number in order related interfaces.

2022-08-24: Updated the `STOP_LOSS/TAKE_PROFIT` description in the `New order (TRADE)` API.

2022-08-23: Fixed incorrect depth information.

2022-08-19: Added weight information for all interfaces.

2022-08-12: Changed `maxNumOrders` to 200 in `filter MAX_NUM_ORDERS`.

2022-08-12: Changed `maxNumAlgoOrders` to 5 in `filter MAX_NUM_ALGO_ORDERS`.

<!--more-->

**# Public Rest API for Coins (2022-09-12)

## General API Information

* The base endpoint is: **https://api.pro.coins.ph**
* All endpoints return data in either a JSON object or array format.
* Data is returned in **ascending** order, with the oldest records appearing first and the newest records appearing last.
* All time and timestamp related fields are expressed in milliseconds.

## HTTP Return Codes
* HTTP `4XX` return codes are used for malformed requests; the issue is on the sender's side.
* HTTP `403` return code is used when the WAF Limit (Web Application Firewall) has been violated.
* HTTP `429` return code is used when breaking a request rate limit.
* HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving `429` codes.
* HTTP `5XX` return codes are used for internal errors; the issue is on exchange's side. It is important to **NOT** treat this as a failure operation; the execution status is UNKNOWN and could have been a success.

## Error Codes
* Any endpoint can return an ERROR; the error payload is as follows:

```javascript
{
  "code": -1000,
  "msg": "An unknown error occurred while processing the request."
}
```

* Specific error codes and messages are defined in another document.

## API Library

### Connectors

The following are lightweight libraries that work as connectors to the Coins public API, written in different languages:

* Python <a href="https://github.com/coins-docs/coins-connector-python">https://github.com/coins-docs/coins-connector-python</a> 

### Postman Collections


Postman collections are available, and they are recommended for new users seeking a quick and easy start with the API.

* Postman <a href="https://github.com/coins-docs/coins-api-postman">https://github.com/coins-docs/coins-api-postman</a> 


## General Information on Endpoints

* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a `query string` or in the `request body` with content type
  `application/x-www-form-urlencoded`. It is also possible to use a combination of parameters in both the query string and the request body if desired.
* Parameters can be sent in any order.
* If a parameter is included in both the `query string` and the `request body`, the value of the parameter from the `query string` will take precedence and be used.



### LIMITS

**General Info on Limits**

* `intervalNum` describes the amount of the interval. For example, `intervalNum 5` with `intervalLetter M` means "Every 5 minutes".

* An HTTP status code 429 will be returned when the rate limit is violated.



### IP Limits

* Each route has a `weight` which determines the number of requests each endpoint counts for. Endpoints with heavier operations or those that involve multiple symbols will have a higher `weight`.
* When a 429 response code is received, it is mandatory for the API user to back off and refrain from making further requests.
* **Repeated failure to comply with rate limits and a disregard for backing off after receiving 429 responses can result in an automated IP ban. The HTTP status code 418 is used for IP bans.**
* IP bans are tracked and their duration increases for repeat offenders, ranging **from 2 minutes to 3 days**.
* A `Retry-After` header is included in 418 or 429 responses, indicating the number of seconds that need to be waited in order to prevent a ban (for 429) or until the ban is lifted (for 418).
* **The limits imposed by the API are based on IP addresses rather than API keys.**



### Order Rate Limits

* When the order count exceeds the limit, you will receive a 429 error without the `Retry-After` header.

* The order rate limit is counted against each IP and UID.



### Websocket Limits

* A single connection can listen to a maximum of 1024 streams.



### /api/ Limit Introduction

* For endpoints related to `/api/*`:

  * There are two modes of limit enforcement: IP limit and UID limit. Each mode operates independently.

  * The IP limit allows a maximum of 1200 requests per minute across all endpoints within the `/api/*` namespace.



### Endpoint Security Type

* Each endpoint is associated with a security type, which indicates how you should interact with it. The security type is specified next to the name of the endpoint.
  * If no security type is mentioned, assume that the security type is NONE.
* API keys are passed to the Rest API via the `X-COINS-APIKEY` header.
* Both API keys and secret keys **are case sensitive**.
* API keys can be configured to have access only to specific types of secure endpoints. For example, one API key may be restricted to TRADE routes only, while another API key can have access to all routes except TRADE.
* By default, API keys have access to all secure routes.

Security Type | Additional parameter          | Description
------------ |-------------------------------| ------------
NONE | none                          | Endpoint can be accessed freely.
TRADE| `X-COINS-APIKEY`、`signature`、`timestamp` | Endpoint requires sending a valid API Key and signature and timing security. 
USER_DATA| `X-COINS-APIKEY`、`signature`、`timestamp`                         | Endpoint requires sending a valid API Key and signature and timing security.. 
USER_STREAM | `X-COINS-APIKEY`                         | Endpoint requires sending a valid API Key.               
MARKET_DATA | `X-COINS-APIKEY`                         | Endpoint requires sending a valid API Key.               

* `TRADE` and `USER_DATA` endpoints are `SIGNED` endpoints.



### SIGNED (TRADE and USER_DATA) Endpoint Security

* `SIGNED` endpoints require an additional parameter, `signature`, to be
  sent in the  `query string` or `form request body` or `header`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation.
  Use your `secretKey` as the key and `totalParams` as the value for the HMAC operation.
* The `signature` is **not case sensitive**.
* `totalParams` is defined as the `query string` concatenated with the
  `request body`(exclude `signature` parameters and values If signature parameters are in both).
* We recommend the use of the `query string` for GET requests and the `form request body` for POST requests. However, for Spot Trading APIs, we recommend using the `query string`.



### Timing Security

* A `SIGNED` endpoint requires an additional parameter, `timestamp`, to be
  sent in the  `query string` or `form request body` or `header`(Not recommended). The `timestamp` should be in millisecond timestamp indicating when the request was created and sent.
* An optional parameter, `recvWindow`, can be included to specify the validity duration of the request in milliseconds after the timestamp. If `recvWindow` is not provided, **it will default to 5000 milliseconds**.
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

**To ensure optimal performance, it is recommended to use a `recvWindow` value of 5000 milliseconds or less. The maximum value allowed for `recvWindow` is 60,000 milliseconds and should not exceed this limit.**



### SIGNED Endpoint Examples for POST /openapi/v1/order

Here is a step-by-step example of how to send a valid signed payload from the
Linux command line using `echo`, `openssl`, and `curl`:

Key | Value
------------ | ------------
apiKey | tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW
secretKey | lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76

Parameter | Value
------------ | ------------
symbol | BTCPHP
side | BUY
type | LIMIT
timeInForce | GTC
quantity | 1
price | 0.1
recvWindow | 5000
timestamp | 1538323200000



#### Example 1: As a query string

* **queryString:** symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000" | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= d7b09aa959094bafd1de10be3985651691fff6cc04b5cd94aea8cc1ca02e0ed8
```

* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW" -X POST 'https://$HOST/openapi/v1/order?symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000&signature=d7b09aa959094bafd1de10be3985651691fff6cc04b5cd94aea8cc1ca02e0ed8'
```



#### Example 2: As a request body

* **requestBody:** symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000" | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= d7b09aa959094bafd1de10be3985651691fff6cc04b5cd94aea8cc1ca02e0ed8
```

* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW" -X POST 'https://$HOST/openapi/v1/order' -d 'symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC&quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000&signature=d7b09aa959094bafd1de10be3985651691fff6cc04b5cd94aea8cc1ca02e0ed8'
```



#### Example 3: Mixed query string and request body

* **queryString:** symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC
* **requestBody:** quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n "symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTCquantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000" | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= 340037ed5366e650bd0e09e170db4a6ace0a9cba3e8af4e5c37ba2143fb84de0
```

* **curl command:**

```shell
(HMAC SHA256)
[linux]$ curl -H "X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW" -X POST 'https://$HOST/openapi/v1/order?symbol=BTCPHP&side=BUY&type=LIMIT&timeInForce=GTC' -d 'quantity=1&price=0.1&recvWindow=5000&timestamp=1538323200000&signature=340037ed5366e650bd0e09e170db4a6ace0a9cba3e8af4e5c37ba2143fb84de0'
```

Note that in Example 3, the signature is different from the previous examples. Specifically, there should be no `&` character between `GTC` and `quantity=1`.


## Public API Endpoints

### Terminology

These terms will be used throughout the documentation, so new users are encouraged to read them to help their understanding of the API.

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
`PARTIALLY_CANCELED` | A part of the order has been cancelled with self trade.
`CANCELED` | The order has been canceled by the user
`EXPIRED`       | The order has been cancelled by matching-engine: LIMIT FOK order not filled, limit order not fully filled etc 

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



**Anti self-trading behaviour(stpFlag):**

| Value | Description                                     |
| ----- | ----------------------------------------------- |
| `CB`  | Both orders will be cancelled by match engine   |
| `CN`  | The new order will be cancelled by match engine |
| `CO`  | The old order will be cancelled by match engine |



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

**Weight:** 1

**Parameters:**

| Name    | Type   | Mandatory | Description                                                  |
| ------- | ------ | --------- | ------------------------------------------------------------ |
| symbol  | STRING | NO        | Specify a trading pair, for example symbol=BTCPHP            |
| symbols | STRING | NO        | Specify multiple trading pairs using commas, for example: symbols=BTCPHP,BTCUSDT |

**Response:**

```json
{
  "timezone": "UTC",
  "serverTime": 1538323200000,
  "exchangeFilters": [],
  "symbols": [
    {
      "symbol": "BTCPHP",
      "status": "TRADING",
      "baseAsset": "BTC",
      "baseAssetPrecision": 8,
      "quoteAsset": "PHP",
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



### Wallet endpoints

#### All Coins' Information (USER_DATA)

```shell
GET /openapi/wallet/v1/config/getall  (HMAC SHA256)
```

Get information on coins (available for deposit and withdrawal) for the user.

**Weight(IP):** 10

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
recvWindow | LONG | NO |
timestamp | LONG | YES |

**Response:**

```javascript
[
    {
        "coin": "ETH",
        "name": "ETH",
        "depositAllEnable": true,
        "withdrawAllEnable": true,
        "free": "1.9144",
        "locked": "0.0426",
        "networkList": [
            {
                "addressRegex": "0x([0-9a-fA-F]){40}",
                "memoRegex": "^[0-9A-Za-z\\-_]{1,120}$",
                "network": "ETH",
                "name": "ERC20",
                "depositEnable": true,
                "minConfirm": 8,
                "unLockConfirm": 12,
                "withdrawDesc": "1234567890",
                "withdrawEnable": true,
                "withdrawFee": "0",
                "withdrawIntegerMultiple": "0.00000001",
                "withdrawMax": "1",
                "withdrawMin": "0.001",
                "sameAddress": false
            }
        ],
        "legalMoney": false
    }
  ]
```



#### Deposit Address (USER_DATA)

```shell
GET /openapi/wallet/v1/deposit/address  (HMAC SHA256)
```

Fetch a deposit address along with its network.

**Weight(IP):** 10

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
coin | STRING | YES | The value is from All Coins' Information API
network | STRING | YES | The value is from All Coins' Information API
recvWindow | LONG | NO |
timestamp | LONG | YES |

**Response:**

```javascript
{
    "coin": "ETH",
    "address": "0xfe98628173830bf79c59f04585ce41f7de168784",
    "addressTag": ""
}
```



#### Withdraw (USER_DATA)

```shell
POST /openapi/wallet/v1/withdraw/apply  (HMAC SHA256)
```

Submit a withdrawal request.

**Weight(UID):** 100

**Parameters:**

| Name            | Type    | Mandatory | Description                                              |
| --------------- | ------- | --------- | -------------------------------------------------------- |
| coin            | STRING  | YES       |                                                          |
| network         | STRING  | YES       |                                                          |
| address         | STRING  | YES       |                                                          |
| addressTag      | STRING  | NO        | Secondary address identifier for coins like XRP,XMR etc. |
| amount          | DECIMAL | YES       |                                                          |
| withdrawOrderId | STRING  | NO        | client id for withdraw, length is limited to 30.         |
| recvWindow      | LONG    | NO        |                                                          |
| timestamp       | LONG    | YES       |                                                          |

* Please note that the `coin`/`network`/`address`/`addressTag` combination **MUST** be in the withdrawal address whitelist. You must set up the withdrawal address whitelist before making this API call.

**Response:**

```javascript
{
  "id":"459165282044051456"
}
```



#### Deposit History (USER_DATA)

```shell
GET /openapi/wallet/v1/deposit/history  (HMAC SHA256)
```

Fetch deposit history.

**Weight(IP):** 2

**Parameters:**

| Name       | Type   | Mandatory | Description                                                  |
| ---------- | ------ | --------- | ------------------------------------------------------------ |
| coin       | STRING | NO        |                                                              |
| txId       | STRING | NO        |                                                              |
| status     | INT    | NO        | 0-PROCESSING, 1-SUCCESS, 2-FAILED, 3-NEED_FILL_DATA(travel rule info) |
| statuses   | STRING | NO        | Specify multiple statuses using commas, for example: statuses=1,3 |
| startTime  | LONG   | NO        | Default: 90 days from current timestamp                      |
| endTime    | LONG   | NO        | Default: current timestamp                                   |
| offset     | INT    | NO        | Default:0                                                    |
| limit      | LONG   | NO        | Default:1000, Max:1000                                       |
| recvWindow | LONG   | NO        |                                                              |
| timestamp  | LONG   | YES       |                                                              |

* Please note the default `startTime` and `endTime` to ensure the time interval is within 0-90 days.

* If both `startTime` and `endTime` are sent, time between `startTime` and `endTime` must be less than 90 days.

* Please note you can send either `status` or `statuses`, but not both.


**Response:**

```javascript
[
    {
        "id": "d_769800519366885376",
        "amount": "0.001",
        "coin": "BNB",
        "network": "BNB",
        "status": 0,
        "address": "bnb136ns6lfw4zs5hg4n85vdthaad7hq5m4gtkgf23",
        "addressTag": "101764890",
        "txId": "98A3EA560C6B3336D348B6C83F0F95ECE4F1F5919E94BD006E5BF3BF264FACFC",
        "insertTime": 1661493146000,
        "confirmNo": 10,
    },
    {
        "id": "d_769754833590042625",
        "amount":"0.5",
        "coin":"IOTA",
        "network":"IOTA",
        "status":1,
        "address":"SIZ9VLMHWATXKV99LH99CIGFJFUMLEHGWVZVNNZXRJJVWBPHYWPPBOSDORZ9EQSHCZAMPVAPGFYQAUUV9DROOXJLNW",
        "addressTag":"",
        "txId":"ESBFVQUTPIWQNJSPXFNHNYHSQNTGKRVKPRABQWTAXCDWOAKDKYWPTVG9BGXNVNKTLEJGESAVXIKIZ9999",
        "insertTime":1599620082000,
        "confirmNo": 20,
    }
]
```



#### Withdraw History (USER_DATA)

```shell
GET /openapi/wallet/v1/withdraw/history  (HMAC SHA256)
```

Fetch withdrawal history.

**Weight(IP):** 2

**Parameters:**

| Name       | Type   | Mandatory | Description                                                  |
| ---------- | ------ | --------- | ------------------------------------------------------------ |
| coin       | STRING | NO        |                                                              |
| withdrawOrderId       | STRING | NO      |                                                              |
| status     | INT    | NO        | 0-PROCESSING, 1-SUCCESS, 2-FAILED |
| startTime  | LONG   | NO        | Default: 90 days from current timestamp                      |
| endTime    | LONG   | NO        | Default: current timestamp                                   |
| offset     | INT    | NO        | Default:0                                                    |
| limit      | LONG   | NO        | Default:1000, Max:1000                                       |
| recvWindow | LONG   | NO        |                                                              |
| timestamp  | LONG   | YES       |                                                              |

* Please note the default `startTime` and `endTime` to ensure the time interval is within 0-90 days.

* If both `startTime` and `endTime` are sent, time between `startTime` and `endTime` must be less than 90 days.

* If `withdrawOrderId` is sent, time between `startTime` and `endTime` must be less than 7 days.

* If `withdrawOrderId` is sent and `startTime` and `endTime` are not provided, the API will return records from the last 7 days by default.


**Response:**

```javascript
[
    {
        "id": "459890698271244288",
        "amount": "0.01",
        "transactionFee": "0",
        "coin": "ETH",
        "status": 1,
        "address": "0x386AE30AE2dA293987B5d51ddD03AEb70b21001F",
        "addressTag": "",
        "txId": "0x4ae2fed36a90aada978fc31c38488e8b60d7435cfe0b4daed842456b4771fcf7",
        "applyTime": 1673601139000,
        "network": "ETH",
        "withdrawOrderId": "thomas123",
        "info": "",
        "confirmNo": 100
    },
    {
        "id": "451899190746456064",
        "amount": "0.00063",
        "transactionFee": "0.00037",
        "coin": "ETH",
        "status": 1,
        "address": "0x386AE30AE2dA293987B5d51ddD03AEb70b21001F",
        "addressTag": "",
        "txId": "0x62690ca4f9d6a8868c258e2ce613805af614d9354dda7b39779c57b2e4da0260",
        "applyTime": 1671695815000,
        "network": "ETH",
        "withdrawOrderId": "",
        "info": "",
        "confirmNo": 100
    }
]
```



#### Transfers (USER_DATA)

```shell
POST /openapi/transfer/v3/transfers
```
This endpoint is used to transfer funds between two accounts.

**Weight:** 50

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
client_transfer_id | STRING | NO | Client Transfer ID, cannot send duplicate ID
account      | STRING | YES    | Either the token (e.g. PHP, BTC, ETH) or the Balance ID (e.g. `1447779051242545455`) to be transferred.
target_address   | STRING | YES    | The phone number or email for recipient account (e.g. `+63 9686490252` or `testsub@gmail.com`)
amount      | BigDecimal | YES    | The amount being transferred
recvWindow | LONG  | NO    | This value cannot be greater than `60000`
timestamp     | LONG  | YES    | A point in time when the transfer is performed
message     | STRING  | NO    | The message sent to the recipient account

If the client_transfer_id or id parameter is passed in, the type parameter is invalid.

**Request:**
```javascript
{
  "account": "1451431230880900352",
  "target_address": "christina@coins.ph",
  "amount": "1232"
}
```

**Response:**
```javascript
{
  "transfer":
    {
      "id": "1451431230880900352",
      "status": "success",//status enum: pending,success,failed
      "account": "90dfg03goamdf02fs",
      "target_address": "testsub@gmail.com",
      "amount": "1",
      "exchange": "1",
      "payment": "23094j0amd0fmag9agjgasd",
      "client_transfer_id": "1487573639841995271",
      "message": "example",
      "errorMessage":""//Error message returned when transfer fails, eg: Insufficient balance
     }
}
```



#### Payment request (USER_DATA)

```shell
POST /openapi/v3/payment-request/payment-requests (HMAC SHA256)
```
Initiate a new payment transaction by creating a payment request.
**Weight:** 1

**Parameters:**

Name              | Type    | Mandatory | Description
-----------------|---------|----------|--------------------------------------------------------------------------------------
payer_contact_info            | STRING  | YES      | The contact information, typically an email address, to which the payment request should be sent.
receiving_account | STRING  | YES      |  Either the token (e.g. PHP) or the Balance ID (e.g. 1447779051242545455 Refer to the id field in /openapi/account/v3/crypto-accounts.) to be transferred.
amount          | DECIMAL | YES      |  The requested amount to be transferred to the requestor's receiving_account.
message          | STRING  | YES      | An arbitrary message that will be attached to the payment request.
supported_payment_collectors          | STRING  | NO       | Methods of payment that are available to a user when they view a payment request, optional items `coins_peso_wallet,CEBL,MLH,PLWN`,  e.g. `["coins_peso_wallet"]` or `["coins_peso_wallet","CEBL","MLH","PLWN"]`. Note: when a payment method is closed, it will be unavailable. 
expires_at          | STRING  | NO       | The expiration date of the payment request. Expected to be in ISO 8601 datetime format (e.g., 2016-10-20T13:00:00.000000Z) or a time delta from the current time (Option:1w,3d,2h,32m,5s). The default expiration period is set to 7 days.
recvWindow | LONG    | NO        | The value cannot be greater than `60000`
timestamp          | LONG    | YES        |

**Response:**

```javascript
{
    "payment-request": {
        "message": "i am boss",
        "id": "1433341829953096704",
        "invoice": "1433341829953096704",
        "amount": "20",
        "currency": "PHP",
        "status": "pending",//pending,fully_paid,expired,canceled
        "created_at": "1685603661217",
        "updated_at": "1685603661217",
        "expires_at": "1686208461219",
        "supported_payment_collectors": "[\"coins_peso_wallet\"]",
        "payment_url": "https://www.pro.coins.ph/payment/invoice/1433341829953096704",
        "payer_contact_info": "jennins@coins.ph"
    }
}
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
   "accountType":"SPOT",
   "canDeposit":true,
   "canTrade":true,
   "canWithdraw":true,
   "balances":[
      {
         "asset":"456",
         "free":"100",
         "locked":"0"
      },
      {
         "asset":"APE",
         "free":"0",
         "locked":"0"
      },
      {
         "asset":"AXS",
         "free":"0.00005",
         "locked":"0"
      }
   ],
   "token":"PHP",
   "daily":{
      "cashInLimit":"500000",
      "cashInRemaining":"499994",
      "cashOutLimit":"500000",
      "cashOutRemaining":"500000",
      "totalWithdrawLimit":"500000",
      "totalWithdrawRemaining":"500000"
   },
   "monthly":{
      "cashInLimit":"10000000",
      "cashInRemaining":"9999157",
      "cashOutLimit":"10000000",
      "cashOutRemaining":"10000000",
      "totalWithdrawLimit":"10000000",
      "totalWithdrawRemaining":"10000000"
   },
   "annually":{
      "cashInLimit":"120000000",
      "cashInRemaining":"119998577",
      "cashOutLimit":"120000000",
      "cashOutRemaining":"119999488",
      "totalWithdrawLimit":"120000000",
      "totalWithdrawRemaining":"119998487.97"
   },
   "updateTime":1707273549694
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
    "symbol": "BTCPHP",
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
    "symbol": "BTCPHP",
    "bidPrice": "0.07946700",
    "bidQty": "9.00000000",
    "askPrice": "100000.00000000",
    "askQty": "1000.00000000"
  }
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
quantity | DECIMAL | NO |
quoteOrderQty | DECIMAL | NO |
price | DECIMAL | NO |
newClientOrderId | STRING | NO | A unique id among open orders. Automatically generated if not sent. Orders with the same `newClientOrderID` can be accepted only when the previous one is filled, otherwise the order will be rejected.
stopPrice | DECIMAL | NO | Used with `STOP_LOSS`, `STOP_LOSS_LIMIT`, `TAKE_PROFIT`, and `TAKE_PROFIT_LIMIT` orders.
newOrderRespType | ENUM | NO | Set the response JSON. `ACK`, `RESULT`, or `FULL`; `MARKET` and `LIMIT` order types default to `FULL`, all other orders default to `ACK`.
stpFlag | ENUM | NO | The anti self-trading behaviour, Default anti self-dealing behaviour is CB 
recvWindow | LONG | NO |The value cannot be greater than `60000`
timestamp | LONG | YES |

Additional mandatory parameters based on `type`:

Type | Additional mandatory parameters                  | Additional Information
------------ |--------------------------------------------------| ------------ 
`LIMIT` | `quantity`, `price`               |
`MARKET` | `quantity` or `quoteOrderQty`                    | `MARKET` orders using `quantity` field specifies the amount of the base asset the user wants to buy/sell, E.g. `MARKET` order on BCHUSDT  will specify how much BCH the user is buying/selling. <br />`MARKET` orders using `quoteOrderQty` field specifies the amount of the quote asset the user wants to buy/sell, E.g. `MARKET` order on BCHUSDT  will specify how much USDT the user is buying/selling.<br /> 
`STOP_LOSS` | `quantity` or `quoteOrderQty`, `stopPrice`       | This will execute a `MARKET` order when`stopPrice` is met. Use `quantity` for selling, `quoteOrderQty` for buying.
`STOP_LOSS_LIMIT` | `quantity`,  `price`, `stopPrice` | This will execute a `LIMIT` order when`stopPrice` is met.
`TAKE_PROFIT` | `quantity` or `quoteOrderQty`, `stopPrice`            | This will execute a `MARKET` order when`stopPrice` is met. Use `quantity` for selling, `quoteOrderQty` for buying.
`TAKE_PROFIT_LIMIT` | `quantity`, `price`, `stopPrice`  | This will execute a `LIMIT` order when`stopPrice` is met.
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

* Either `orderId` or `origClientOrderId` must be sent. If both parameters are sent, `orderId` takes precedence. A list will be returned for orders with identical clientOrderId.

**Response Single Order:**

```javascript
{
    'clientOrderId': 'test5678',
    'cummulativeQuoteQty': '3946.87326',
    'executedQty': '0.001',
    'isWorking': False,
    'orderId': 1799249051008066560,
    'origQty': '0.001',
    'origQuoteOrderQty': '3946.87326',
    'price': '0',
    'side': 'BUY',
    'status': 'FILLED',
    'stopPrice': '0',
    'symbol': 'BTCPHP',
    'time': 1729223201090,
    'timeInForce': 'GTC',
    'type': 'MARKET',
    'updateTime': 1729223201201
}
```
**Response Order List:**

```javascript
[
    {
        'clientOrderId': 'test5678',
        'cummulativeQuoteQty': '3946.87326',
        'executedQty': '0.001',
        'isWorking': False,
        'orderId': 1799249051008066560,
        'origQty': '0.001',
        'origQuoteOrderQty': '3946.87326',
        'price': '0',
        'side': 'BUY',
        'status': 'FILLED',
        'stopPrice': '0',
        'symbol': 'BTCPHP',
        'time': 1729223201090,
        'timeInForce': 'GTC',
        'type': 'MARKET',
        'updateTime': 1729223201201
    },
    {
        'clientOrderId': 'test5678',
        'cummulativeQuoteQty': '127.24738',
        'executedQty': '2.21',
        'isWorking': False,
        'orderId': 1799253321187025920,
        'origQty': '2.21',
        'origQuoteOrderQty': '127.24738',
        'price': '0',
        'side': 'BUY',
        'status': 'FILLED',
        'stopPrice': '0',
        'symbol': 'USDCPHP',
        'time': 1729223710134,
        'timeInForce': 'GTC',
        'type': 'MARKET',
        'updateTime': 1729223710186
    }
]
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
        "origQuoteOrderQty": "0"
    }
]
```



#### Current open orders (USER_DATA)

```shell
GET /openapi/v1/openOrders  (HMAC SHA256)
```

GET all open orders on a symbol. **Careful** when accessing this with no symbol.

**Weight:** 10

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



### Convert endpoints
#### Get supported trading pairs (TRADE)
```shell
POST /openapi/convert/v1/get-supported-trading-pairs
```

This continuously updated endpoint returns a list of all available trading pairs. 

**Weight:** 1

**Parameters:**

 N/A



**Response:**

Field name	| Description
----|---
sourceCurrency	| Source token.
targetCurrency	| Target token.
minSourceAmount	| amount range min value.
maxSourceAmount	| amount range max value.
precision	| The level of precision in decimal places used.

```javascript
{
  "status":0, 
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



#### Fetch a quote (TRADE)

```shell
POST /openapi/convert/v1/get-quote
```

This endpoint returns a quote for a specified source currency (sourceCurrency) and target currency (targetCurrency) pair.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ |-----------| ------------
sourceCurrency | STRING | YES       |The currency the user holds
targetCurrency | STRING | YES       |The currency the user would like to obtain
sourceAmount | STRING | NO        |The amount of sourceCurrency. You only need to fill in either the source amount or the target amount. If both are filled, it will result in an error.
targetAmount | STRING | NO        |The amount of targetCurrency. You only need to fill in either the source amount or the target amount. If both are filled, it will result in an error.

**Response:**

Field name	| Description
----|---
quoteId	| Quote unique id.
sourceCurrency	| Source token.
targetCurrency	| Target token.
sourceAmount	| Source token amount.
price	| Trading pairs price.
targetAmount	| Targe token amount.
expiry	| Quote expire time seconds.

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

#### Accept the quote (TRADE)


```shell
POST /openapi/convert/v1/accept-quote
```

Use this endpoint to accept the quote and receive the result instantly.

**Weight:** 1

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
quoteId | STRING | YES |The ID assigned to the quote


**Response:**

Field name	| Description
----|---
status	| 0 mean order are created. 
data.orderId	| Order ID generated by the server.
data.status	| The order status is an enumeration with values `SUCCESS`, `PROCESSING`;PROCESSING mean that the server is processing,SUCCESS means the order is successful.

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

***Error code description:***

status code           | Description
----------------| ------------
0 | means that the call is processed normally.(Applicable to other endpoint if there is a status structure)
10000003 | Failed to fetch account verification information.
10000003 | Quote expired.
10000003 | Unable to fetch account information.
10000003 | The price has changed! Please confirm the updated rate to complete the transaction.
10000003 | Insufficient balance.
10000003 | Failed to fetch liquidity. Try again later.

#### Retrieve order history (USER_DATA)


```shell
POST /openapi/convert/v1/query-order-history
```
This endpoint retrieves order history with the option to define a specific time period using start and end times.

**Weight:** 1

**Parameters:**

Name | Type   | Mandatory | Description
------------ |--------|---------| ------------
startTime | STRING | No | Numeric string representing milliseconds. The starting point of the required period. If no period is defined, the entire order history is returned.
endTime | STRING | No |Numeric string representing milliseconds. The end point of the required period. If no period is defined, the entire order history is returned.
status | STRING | No | deliveryStatus, If this field is available, use it with startTime. `TODO`, `SUCCESS`, `FAILED`, `PROCESSING`
page | int    | No |
size | int    | No |


**Response:**

Field name	| Description
----|---
orderId	| Order ID generated by the server.
quoteId	| Order reference quote Id.
userId	| user id.
sourceCurrency	| source currency.
targetCurrency	| target currency.
sourceAmount	| source currency amount.
targetAmount	| target currency amount.
price	| price.
status	| Order status.`TODO`, `SUCCESS`, `FAILED`, `PROCESSING`
createdAt	| Order create time.
errorMessage	| Error message if order failed.



```javascript
{
  "status": 0,
   "error": "OK",
   "data": [
    {
      "id":"",
      "orderId": "25a9b92bcd4d4b2598c8be97bc65b466",
      "quoteId": "1ecce9a7265a4a329cce80de46e2c583",
      "userId":"",
      "sourceCurrency": "BTC",
      "sourceCurrencyIcon":"",
      "targetCurrency": "PHP",
      "targetCurrencyIcon":"",
      "sourceAmount": "0.11",
      "targetAmount": "4466.89275956",
      "price": "40608.115996",
      "status": "SUCCESS",
      "createdAt": "1671797993000",
      "errorCode": "",
      "errorMessage": "",
      "inversePrice": "3306.115996"
    }
  ],
  "total": 23
}
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

The `PERCENT_PRICE` filter defines the valid range for a price based on the weighted average of the previous trades. `avgPriceMins` is the number of minutes the weighted average price is calculated over.

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

The `PERCENT_PRICE_SA` filter defines the valid range for a price based on the simple average of the previous trades. `avgPriceMins` is the number of minutes the simple average price is calculated over.

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

The `PERCENT_PRICE_INDEX` filter defines the valid range for a price based on the index price, which is calculated from several exchanges in the market according to certain rules. (indexPrice websocket pushing will be available in the future)

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



### Fiat endpoints

#### Get supported fiat channels (TRADE)
```shell
POST openapi/fiat/v1/support-channel
```

This continuously updated endpoint returns a list of all available fiat channels.
**Weight:** 1

**Parameters(suggest json body):**

Name            | Type   | Mandatory | Description
----------------|--------| ------------ | ------------
transactionType | STRING | Yes |Set this parameter to -1 to indicate a cash-out transaction. At present, only cash-out transactions are supported.
currency        | STRING | Yes |The parameter represents the currency used in the transaction and should be set to PHP as it is the only currency currently supported.
transactionChannel | STRING | No |  Filter the list by first-level channel. The first-level channel is an enumeration. The optional values are INSTAPAY, SWIFTPAY_PESONET.
transactionSubject        | STRING | No | Filter the list by secondary channels. Such as Gcash.
amount        | STRING | No | Used to match the fee rate. If the fee rate is calculated based on the amount range, the default value is 0 if not filled in.

**Response:**

FieldName            | Type   | Description
----------------|--------| ------------
id | Long   |channel id
transactionChannel | String |The first-level channel to which it belongs.
transactionChannelName | String |The name of the first-level channel to which it belongs
transactionSubject | String |Channel code.
transactionSubjectType | String |Channel type.
transactionSubjectTypeLabel | String |Channel label.
transactionSubjectName | String |Channel name.
transactionType | int    |1 mean cash in,-1 mean cash out.
paymentMethod | String |Payment type, BANK_TRANSFER only. 
channelIcon | url    |first-level channel Icon.
subjectIcon | url    |Channel Icon.
maximum | String |Maximum order amount.
minimum | String |Minimum order amount.
dailyLimit | String |Channel daily limit.
monthlyLimit | String |Channel monthly limit.
annualLimit | String |Channel annual limit.
remainingDailyLimit | String |Channel daily limit remaining.
remainingMonthlyLimit | String |Channel monthly limit remaining.
remainingAnnualLimit | String |Channel annual limit remaining.
precision | String |Preserved precision.
fee | String |Fee rate.
feeType | String |Channel fee method, FIXED stands for fixed, value PERCENTAGE stands for percentage charging.
status | String | Channel status 1 means it is normally available, 0 means the channel is under maintenance.
maxWithdrawBalance | String |Current user’s balance available for withdrawal.


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


#### Cash out (TRADE)
```shell
POST openapi/fiat/v1/cash-out
```

This endpoint allows users to withdraw funds from their fiat account.

**Weight:** 1

**Parameters(with json body):**

Name         | Type   | Mandatory | Description
-------------|--------|-----------| ------------
internalOrderId | STRING | yes       | Internal ID assigned to the funds withdrawal order, all are numbers and does not start with 0, Length is 10 to 20
currency     | STRING | Yes       | The parameter represents the currency used in the transaction and should be set to PHP as it is the only currency currently supported.
amount       | STRING | Yes       | The amount of currency to be withdrawn.
channelName  | STRING | Yes       | The first-level channel to which it belongs.The first-level channel is an enumeration. The optional values are INSTAPAY, SWIFTPAY_PESONET.
channelSubject | STRING | Yes       | Channel code.
extendInfo | JSON Object | No        | A JSON object with additional information. Its structure and content may vary depending on the specific channel(Refer to demo below). The fields within the JSON object are: `recipientName`, `recipientAccountNumber`, `recipientAddress`, `remarks`.

**Request:**

- SWIFTPAY_PESONET （Large amount arrives on T+1）
```javascript
{
  "amount": "1000",
  "internalOrderId":"2023090410571114",
  "currency":"PHP",
  "channelName": "SWIFTPAY_PESONET",
  "channelSubject":"gcash",
  "extendInfo":{    
    "recipientAccountNumber": "095656x4060",
    "recipientName":"Joseph Pal Fajagut",
    "recipientAddress": "Santo",  // optional
    "remarks": "pesonet Cash out"  // optional
  }
}
```
- INSTAPAY （Near real-time payment）
```javascript
{
  "amount": "1000",
  "internalOrderId":"2023090410571114",
  "currency":"PHP",
  "channelName": "INSTAPAY",
  "channelSubject":"gcash",
  "extendInfo":{    
    "recipientAccountNumber": "095656x4060",
    "recipientName":"Joseph Pal Fajagut"
  }
}
```

**Response:**

FieldName            | Type   | Description
----------------|--------| ------------
externalOrderId | String   | Coins internal ID, query not supported yet; ignore it.
internalOrderId | String   | The unique order id generated by the server or the passed `internalOrderId`.

```javascript
{
  "status": 0,
  "error": "OK", 
  "data": {
         "externalOrderId": "1380692028693995623",
         "internalOrderId": "1388420429697583896"
          },
  "params": null  //Reserved field Ignore it
}
```
***status code description:***

status code           | Description
----------------| ------------
0 | means that the call is processed normally.(Applicable to other endpoint if there is a status structure) 
88010002 | User account disabled,please contact support for assistance.
88010003 | order amount lower than channel minAmount.
88010004 | order amount great than channel maxAmount.
88010005 | Daily cash-out limit exceeded.
88010006 | Monthly cash-out limit exceeded.
88010007 | Annual cash-out limit exceeded.
88010008,88010009,88010010,88010011 | Cash-outs method unavailable. please try again later
88010012 | Cash-out order in progress. Wait for its completion and create a new one.
88010013 | Insufficient balance.Please make sure you have enough available balance
88010014,88010015,88010016,88010018 | Please pass kyc first.
88010017 | Please bind your phone number under Account Settings first.
88010000 | Server side error,please contact support for assistance.


#### Curl Example

* **queryString:** k1=v1&k2=v2
* **requestBody:** {"key":"value"}
* **HMAC SHA256 signature:**

```shell
[linux]$ echo -n 'k1=v1&k2=v2{"key":"value"}' | openssl dgst -sha256 -hmac "lH3ELTNiFxCQTmi9pPcWWikhsjO04Yoqw3euoHUuOLC3GYBW64ZqzQsiOEHXQS76"
(stdin)= ef230e573e76e304d4579ef7f777a7f236aec25b8881ccc9797bb91b186e24dd
```

* **curl command:**

Note: X-COINS-APIKEY and signature are both placed in the request header

```shell
(HMAC SHA256)
[linux]$ curl -X POST 'https://$HOST/openapi/fiat/v1/cash-out?k1=v1&k2=v2' \
-H 'X-COINS-APIKEY: tAQfOrPIZAhym0qHISRt8EFvxPemdBm5j5WMlkm3Ke9aFp0EGWC2CGM8GHV4kCYW' \
-H 'signature: ef230e573e76e304d4579ef7f777a7f236aec25b8881ccc9797bb91b186e24dd' \
-H 'Content-Type: application/json' \
--data '{"key":"value"}'
```

#### Fiat order detail (USER_DATA)
```shell
GET openapi/fiat/v1/details
```

This endpoint retrieves information about a specific fiat currency order. The response provides detailed information about the respective trade, including the transaction date, transaction amount, and any associated fees.

**Weight:** 1

**Parameters:**

Name            | Type   | Mandatory | Description
----------------|--------|-----------| ------------
internalOrderId | STRING | YES       | ID of the order for which the user wishes to retrieve details.

**Response:**

FieldName            | Type    | Description
----------------|---------| ------------
externalOrderId | String  | Coins internal ID, query not supported yet; ignore it.
internalOrderId | String  |The unique order id generated by the server.
paymentOrderId | String  |The payment order id generated by the channel.
fiatCurrency | String  |PHP as it is the only currency currently supported
fiatAmount | String  |The order amount.
transactionType | String  | 1 means cash in, -1 means cash out.
transactionChannel | String  |The first-level channel to order it belongs.
transactionSubject | String  |The channel code of order.
transactionSubjectType | String  |Channel type.
transactionChannelName | String  |First level channel Name.
transactionSubjectName | String  |channel Name.
feeCurrency | String  |PHP as it is the only currency currently supported
channelFee | String  |The fee of order always 0 Deprecated, see platformFee.
platformFee | String  |The fee of order.
status | String  | **Enum:** `PENDING`, `SUCCEEDED`, `FAILED`, `CANCEL`<br>Indicates the current state of the order.<br>• `PENDING`: The order is still being processed and is not in a final state.<br>• `SUCCEEDED`: The order has been successfully processed.<br>• `FAILED`: The order processing has failed.<br>• `CANCEL`: The order has been canceled by the user and is considered equivalent to a failure.<br><br>Only orders in a final state (`SUCCEEDED`, `FAILED`, or `CANCEL`) are considered complete and reliable for further actions. Avoid relying on orders in a non-final state (`PENDING`) as their status may change.
errorCode | String  |Order fail with errorCode.
errorMessage | String  |Order fail with error message.
completedTime | String  |The time of order completed.
source | String  |Order create client Type，such as WEB,ANDROID,IOS,open-api.
createdAt | String  |The time of order created.
orderExtendedMap | Object  |The order extend data.
dealCancel | boolean | If order can be canceled, value will be true.


```javascript
{
  "status": 0,
  "error": "OK",
  "data": {
            "externalOrderId": "1380692028693995623",
            "internalOrderId": "1388420429697583896",
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
              "sendAcctNo"
              "1326887193441018369",
              "tfrName": "Li Mei Ling",
              "tfrAcctNo": "095656x4060",
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
#### Fiat order history V1 (USER_DATA) [Deprecated]
```shell
POST openapi/fiat/v1/history
```

This endpoint is used to query all fiat related history

**Weight:** 1

**Parameters(with json body):**

Name            | Type   | Mandatory                                    | Description
----------------|--------|----------------------------------------------| ------------
pageNum | STRING | No                                           | Page number default 1.
pageSize | STRING | No                                           | Page size,default 10.
internalOrderId | STRING | No | Coins returns a unique tracking order number.
transactionType | STRING | No                                           | Order Transaction Type 1: cash-in, -1: cash-out.
transactionChannel | STRING | No                                           | Transaction channel, the optional values are INSTAPAY, SWIFTPAY_PESONET.
transactionSubject | STRING | No                                           | Secondary channels, such as Gcash supported under instapay.
status | STRING | No                                           | **Enum:** `PENDING`, `SUCCEEDED`, `FAILED`, `CANCEL`<br>Indicates the current state of the order.<br>• `PENDING`: The order is still being processed and is not in a final state.<br>• `SUCCEEDED`: The order has been successfully processed.<br>• `FAILED`: The order processing has failed.<br>• `CANCEL`: The order has been canceled by the user and is considered equivalent to a failure.<br><br>Only orders in a final state (`SUCCEEDED`, `FAILED`, or `CANCEL`) are considered complete and reliable for further actions. Avoid relying on orders in a non-final state (`PENDING`) as their status may change.
fiatCurrency | STRING | No                                           | fiat currecy.
startDate | STRING | No                                           | the order's create time will between startDate and endDate. This parameter accepts input in the ISO 8601 format for date and time, which is based on the Coordinated Universal Time (UTC) time zone (e.g., "2016-10-20T13:00:00.000000Z"). Alternatively, you can provide a time delta from the current time (e.g., "1w 3d 2h 32m 5s").
endDate | STRING | No                                           | the order's create time will between startDate and endDate. This parameter accepts input in the ISO 8601 format for date and time, which is based on the Coordinated Universal Time (UTC) time zone (e.g., "2016-10-20T13:00:00.000000Z"). Alternatively, you can provide a time delta from the current time (e.g., "1w 3d 2h 32m 5s").
startTime | LONG   | No                                           | the order's create time will between startTime and endTime. Works when both startDate and endDate are empty. The server time zone is utc+0
endTime | LONG   | No                                           | the order's create time will between startTime and endTime. Works when both startDate and endDate are empty. The server time zone is utc+0

**Response:**

FieldName            | Type    | Description
----------------|---------| ------------
externalOrderId | String  | Coins internal ID, query not supported yet; ignore it.
internalOrderId | String  |The unique order id generated by the server.
paymentOrderId | String  |The payment order id generated by the channel.
fiatCurrency | String  |PHP as it is the only currency currently supported
fiatAmount | String  |The order amount.
transactionType | String  |1 mean cash in,-1 mean cash out.
transactionChannel | String  |The first-level channel to order it belongs.
transactionSubject | String  |The channel code of order.
transactionSubjectType | String  |Channel type.
transactionChannelName | String  |First level channel Name.
transactionSubjectName | String  |channel Name.
feeCurrency | String  |PHP as it is the only currency currently supported
channelFee | String  |The fee of order always 0 Deprecated, see platformFee.
platformFee | String  |The fee of order.
status | String  |**Enum:** `PENDING`, `SUCCEEDED`, `FAILED`, `CANCEL`<br>Indicates the current state of the order.<br>• `PENDING`: The order is still being processed and is not in a final state.<br>• `SUCCEEDED`: The order has been successfully processed.<br>• `FAILED`: The order processing has failed.<br>• `CANCEL`: The order has been canceled by the user and is considered equivalent to a failure.<br><br>Only orders in a final state (`SUCCEEDED`, `FAILED`, or `CANCEL`) are considered complete and reliable for further actions. Avoid relying on orders in a non-final state (`PENDING`) as their status may change.
errorCode | String  |Order fail with errorCode.
errorMessage | String  |Order fail with error message.
completedTime | String  |The time of order completed.
source | String  |Order create client Type，such as WEB,ANDROID,IOS,open-api.
createdAt | String  |The time of order created.
orderExtendedMap | Object  |The order extend data.
dealCancel | boolean | If order can be canceled, value will be true.

```javascript
{
    "status": 0,
    "error": "OK",
    "data": [
        {
            "externalOrderId": "1476829580936625682",
            "internalOrderId": "1476829580936625681",
            "paymentOrderId": "000008",
            "fiatCurrency": "PHP",
            "fiatAmount": "11111",
            "transactionType": 1,
            "transactionChannel": "INSTAPAY",
            "transactionSubject": "allbank",
            "transactionChannelName": "instapay",
            "transactionSubjectName": "AllBank (A Thrift Bank), Inc.",
            "transactionSubjectType": "bank",
            "feeCurrency": "PHP",
            "channelFee": "0",
            "platformFee": "0",
            "status": "SUCCEEDED",
            "errorCode": "SUCCEEDED",
            "errorMessage": "",
            "completedTime": "2023-07-31T07:16:47.000+00:00",
            "source": "WEBHOOK",
            "createdAt": "2023-07-31T07:16:46.000+00:00",
            "orderExtendedMap": {
                "channelReferenceNo": "000008",
                "senderAccountNumber": "8613989193526"
            },
            "dealCancel": false
        },
        {
            "externalOrderId": "1476829580936625686",
            "internalOrderId": "1474038775754799133",
            "paymentOrderId": "2023072799",
            "fiatCurrency": "PHP",
            "fiatAmount": "100",
            "transactionType": 1,
            "transactionChannel": "SWIFTPAY_PESONET",
            "transactionSubject": "aub",
            "transactionChannelName": "swiftpay_pesonet",
            "transactionSubjectName": "Asia United Bank",
            "transactionSubjectType": "e-wallet",
            "feeCurrency": "PHP",
            "channelFee": "0",
            "platformFee": "10",
            "status": "SUCCEEDED",
            "errorCode": "SUCCEEDED",
            "errorMessage": "",
            "completedTime": "2023-07-27T10:52:41.000+00:00",
            "source": "WEBHOOK",
            "createdAt": "2023-07-27T10:52:40.000+00:00",
            "orderExtendedMap": {
                "channelReferenceNo": "000008",
                "senderAccountNumber": "8613989193526"
            },
            "dealCancel": false
        }
    ],
    "total": 2
}
```
------

#### Fiat order history V2 (USER_DATA)
```shell
POST openapi/fiat/v2/history
```

This endpoint is used to query all fiat related history

**Weight:** 1

**Parameters(with json body):**

Name            | Type   | Mandatory                                    | Description
----------------|--------|----------------------------------------------| ------------
pageNum | STRING | No                                           | Page number default 1.
pageSize | STRING | No                                           | Page size,default 10.
externalOrderId | String  | No                                             | Coins internal ID.
internalOrderId | String  |No |The unique order id generated by the server. 
transactionType | STRING | No                                           | Order Transaction Type 1: cash-in, -1: cash-out.
transactionChannel | STRING | No                                           | Transaction channel, the optional values are INSTAPAY, SWIFTPAY_PESONET.
transactionSubject | STRING | No                                           | Secondary channels, such as Gcash supported under instapay.
status | STRING | No                                           | **Enum:** `PENDING`, `SUCCEEDED`, `FAILED`, `CANCEL`<br>Indicates the current state of the order.<br>• `PENDING`: The order is still being processed and is not in a final state.<br>• `SUCCEEDED`: The order has been successfully processed.<br>• `FAILED`: The order processing has failed.<br>• `CANCEL`: The order has been canceled by the user and is considered equivalent to a failure.<br><br>Only orders in a final state (`SUCCEEDED`, `FAILED`, or `CANCEL`) are considered complete and reliable for further actions. Avoid relying on orders in a non-final state (`PENDING`) as their status may change.
fiatCurrency | STRING | No                                           | fiat currecy.
startDate | STRING | No                                           | the order's create time will between startDate and endDate. This parameter accepts input in the ISO 8601 format for date and time, which is based on the Coordinated Universal Time (UTC) time zone (e.g., "2016-10-20T13:00:00.000000Z"). Alternatively, you can provide a time delta from the current time (e.g., "1w 3d 2h 32m 5s").
endDate | STRING | No                                           | the order's create time will between startDate and endDate. This parameter accepts input in the ISO 8601 format for date and time, which is based on the Coordinated Universal Time (UTC) time zone (e.g., "2016-10-20T13:00:00.000000Z"). Alternatively, you can provide a time delta from the current time (e.g., "1w 3d 2h 32m 5s").
startTime | LONG   | No                                           | the order's create time will between startTime and endTime. Works when both startDate and endDate are empty. The server time zone is utc+0
endTime | LONG   | No                                           | the order's create time will between startTime and endTime. Works when both startDate and endDate are empty. The server time zone is utc+0

**Response:**

FieldName            | Type    | Description
----------------|---------| ------------
externalOrderId | String  | Coins internal ID, query not supported yet; ignore it.
internalOrderId | String  |The unique order id generated by the server.
paymentOrderId | String  |The payment order id generated by the channel.
fiatCurrency | String  |PHP as it is the only currency currently supported
fiatAmount | String  |The order amount.
transactionType | String  |1 mean cash in,-1 mean cash out.
transactionChannel | String  |The first-level channel to order it belongs.
transactionSubject | String  |The channel code of order.
transactionSubjectType | String  |Channel type.
transactionChannelName | String  |First level channel Name.
transactionSubjectName | String  |channel Name.
feeCurrency | String  |PHP as it is the only currency currently supported
channelFee | String  |The fee of order always 0 Deprecated, see platformFee.
platformFee | String  |The fee of order.
status | String  |**Enum:** `PENDING`, `SUCCEEDED`, `FAILED`, `CANCEL`<br>Indicates the current state of the order.<br>• `PENDING`: The order is still being processed and is not in a final state.<br>• `SUCCEEDED`: The order has been successfully processed.<br>• `FAILED`: The order processing has failed.<br>• `CANCEL`: The order has been canceled by the user and is considered equivalent to a failure.<br><br>Only orders in a final state (`SUCCEEDED`, `FAILED`, or `CANCEL`) are considered complete and reliable for further actions. Avoid relying on orders in a non-final state (`PENDING`) as their status may change.
errorCode | String  |Order fail with errorCode.
errorMessage | String  |Order fail with error message.
completedTime | String  |The time of order completed.
source | String  |Order create client Type，such as WEB,ANDROID,IOS,open-api.
createdAt | String  |The time of order created.
orderExtendedMap | Object  |The order extend data.
dealCancel | boolean | If order can be canceled, value will be true.

```javascript
{
    "status": 0,
    "error": "OK",
    "data": [
        {
            "externalOrderId": "1476829580936625682",
            "internalOrderId": "1476829580936625681",
            "paymentOrderId": "000008",
            "fiatCurrency": "PHP",
            "fiatAmount": "11111",
            "transactionType": 1,
            "transactionChannel": "INSTAPAY",
            "transactionSubject": "allbank",
            "transactionChannelName": "instapay",
            "transactionSubjectName": "AllBank (A Thrift Bank), Inc.",
            "transactionSubjectType": "bank",
            "feeCurrency": "PHP",
            "channelFee": "0",
            "platformFee": "0",
            "status": "SUCCEEDED",
            "errorCode": "SUCCEEDED",
            "errorMessage": "",
            "completedTime": "2023-07-31T07:16:47.000+00:00",
            "source": "WEBHOOK",
            "createdAt": "2023-07-31T07:16:46.000+00:00",
            "orderExtendedMap": {
                "channelReferenceNo": "000008",
                "senderAccountNumber": "8613989193526"
            },
            "dealCancel": false
        },
        {
            "externalOrderId": "1476829580936625686",
            "internalOrderId": "1474038775754799133",
            "paymentOrderId": "2023072799",
            "fiatCurrency": "PHP",
            "fiatAmount": "100",
            "transactionType": 1,
            "transactionChannel": "SWIFTPAY_PESONET",
            "transactionSubject": "aub",
            "transactionChannelName": "swiftpay_pesonet",
            "transactionSubjectName": "Asia United Bank",
            "transactionSubjectType": "e-wallet",
            "feeCurrency": "PHP",
            "channelFee": "0",
            "platformFee": "10",
            "status": "SUCCEEDED",
            "errorCode": "SUCCEEDED",
            "errorMessage": "",
            "completedTime": "2023-07-27T10:52:41.000+00:00",
            "source": "WEBHOOK",
            "createdAt": "2023-07-27T10:52:40.000+00:00",
            "orderExtendedMap": {
                "channelReferenceNo": "000008",
                "senderAccountNumber": "8613989193526"
            },
            "dealCancel": false
        }
    ],
    "total": 2
}
```
------



## Sub-account endpoints

### Query Sub-account List (For Master Account)

Applies to master accounts only.

```shell
GET /openapi/v1/sub-account/list
```

**Weight:** 1

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
email      | STRING | NO    | Sub account email
page    | INT | NO | Current page, default value: 1
limit    | INT | NO | Quantity per page, default value 10, maximum `200`
recvWindow | LONG  | NO    | This value cannot be greater than `60000`
timestamp     | LONG  | YES    | A point in time for which transfers are being queried.


**Response:**
```json
{
  "subAccounts": [
    {
      "createTime": "1689744671462",
      "email": "testsub@gmail.com",
      "isFreeze": false
    },
    {
      "createTime": "1689744700710",
      "email": "testsub2@gmail.com",
      "isFreeze": false
    }
  ],
 "total": 2
}
```

### Create a Virtual Sub-account(For Master Account)

This interface currently supports the creation of virtual sub-accounts (maximum 30).

```shell
POST /openapi/v1/sub-account/create
```

**Weight:** 1

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
accountName      | STRING | YES       | Sub account email
recvWindow | LONG  | NO        | This value cannot be greater than `60000`
timestamp     | LONG  | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "email": "testsub@gmail.com",
  "createTime": 1689744700710,
  "isFreeze": false
}
```


### Query Sub-account Assets (For Master Account)

Query detailed balance information of a sub-account via the master account (applies to master accounts only).

```shell
GET /openapi/v1/sub-account/asset
```

**Weight:** 1

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
email      | STRING | YES       | Sub account email
recvWindow | LONG  | NO        | This value cannot be greater than `60000`
timestamp     | LONG  | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "balances": [
    {
      "asset": "BTC",
      "free": "0.1",
      "locked": "0"
    },
    {
      "asset": "ETH",
      "free": "0.1",
      "locked": "0"
    }
  ]
}
```



### Universal Transfer (For Master Account)

Master account can initiate a transfer from any of its sub-accounts to the master account, or from the master account to any sub-account.

```shell
POST /openapi/v1/sub-account/transfer/universal-transfer
```

**Weight:** 1

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
fromEmail      | STRING | NO        | 
toEmail      | STRING | NO        | 
clientTranId      | STRING | NO        | Must be unique
asset      | STRING | YES       | 
amount      | DECIMAL | YES        | 
recvWindow | LONG  | NO        | This value cannot be greater than `60000`
timestamp     | LONG  | YES       | A point in time for which transfers are being queried.

- Transfer from master account by default if fromEmail is not sent.
- Transfer to master account by default if toEmail is not sent.
- Specify at least one of fromEmail and toEmail.
- Supported transfer scenarios:
  - Master account transfer to sub-account 
  - Sub-account transfer to master account 
  - Sub-account transfer to Sub-account


**Response:**
```json
{
  "clientTransferId": "1487573639841995271",
  "result": true//true:success,false:failed
}
```

### Transfer to Master (For Sub-account)

Sub-account can initiate a transfer from itself to the master account.

```shell
POST /openapi/v1/sub-account/transfer/sub-to-master
```

**Weight:** 1

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
asset      | STRING | YES       |
amount      | DECIMAL | YES        |
clientTranId      | STRING | NO        | Must be unique
recvWindow | LONG  | NO        | This value cannot be greater than `60000`
timestamp     | LONG  | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "clientTransferId": "1487573639841995271",
  "result": true//true:success,false:failed
}
```

### Query Universal Transfer History (For Master Account)

Applies to master accounts only.
If startTime and endTime are not sent, this will return records of the last 30 days by default.

```shell
GET /openapi/v1/sub-account/transfer/universal-transfer-history
```

**Weight:** 1

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
fromEmail      | STRING | NO        |
toEmail      | STRING | NO        |
clientTranId      | STRING | NO        | 
tokenId      | STRING | NO        | 
startTime      | LONG | NO        | Millisecond timestamp
endTime      | LONG | NO        | Millisecond timestamp,Data excluding the endTime.
page      | INT | NO        | Current page, default value: 1
limit      | INT | NO        | Quantity per page, default value `500`, maximum `500`
recvWindow | LONG  | NO        | This value cannot be greater than `60000`
timestamp     | LONG  | YES       | A point in time for which transfers are being queried.


- fromEmail and toEmail cannot be sent at the same time.
- Return fromEmail equal master account email by default.
- The query time period must be less than 30 days. 
- If startTime and endTime not sent, return records of the last 30 days by default.

**Response:**
```json
{
  "result": [
    {
      "clientTranId": "1",
      "fromEmail": "testsub@gmail",
      "toEmail": "testsub1@gmail",
      "asset": "BTC",
      "amount": "0.1",
      "createdAt": 1689744700710,
      "status": "success"//success,pending,failed
    }
  ],
  "total": 1
}
```


### Sub-account Transfer History (For Sub-account)

Applies to sub-accounts only.
If startTime and endTime are not sent, this will return records of the last 30 days by default.

```shell
GET /openapi/v1/sub-account/transfer/sub-history
```

**Weight:** 1

**Parameters:**

Name       | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
asset      | STRING | NO        |
type      | INT | NO        | 1: transfer in, 2: transfer out. If the type parameter is not provided or provided incorrectly, the data returned will be for transfer out.
startTime      | LONG   | NO        | Millisecond timestamp
endTime      | LONG   | NO        | Millisecond timestamp,Data excluding the endTime.
page      | INT    | NO        | Current page, default value: 1
limit      | INT | NO        | Quantity per page, default value `500`, maximum `500`
recvWindow | LONG   | NO        | This value cannot be greater than `60000`
timestamp     | LONG   | YES       | A point in time for which transfers are being queried.
clientTranId     | STRING   | NO       |

- If type is not sent, the records of type 2: transfer out will be returned by default.
- If startTime and endTime are not sent, the recent 30-day data will be returned.

**Response:**
```json
{
  "result": [
    {
      "clientTranId": "1",
      "fromEmail": "testsub@gmail",
      "toEmail": "testsub1@gmail",
      "asset": "BTC",
      "amount": "0.1",
      "createdAt": 1689744700710,
      "status": "success"//success,pending,failed
    }
  ],
  "total": 1
}
```


### Get IP Restriction for a Sub-account API Key (For Master Account)

Query detailed IPs for a sub-account API key.

```shell
GET /openapi/v1/sub-account/apikey/ip-restriction
```

**Weight:** 1

**Parameters:**

Name       | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
apikey      | STRING | YES        | 
email      | STRING | YES        | 	Sub account email
recvWindow | LONG   | NO        | This value cannot be greater than `60000`
timestamp     | LONG   | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "apikey": "k5V49ldtn4tszj6W3hystegdfvmGbqDzjmkCtpTvC0G74WhK7yd4rfCTo4lShf",
  "ipList": [
    "8.8.8.8"
  ],
  "ipRestrict": true,
  "role": "2,3,4,5,6",//0:READ_ONLY, 2:TRADE_ONLY, 3:CONVERT_ONLY, 4:CRYPTO_WALLET_ONLY, 5:FIAT_ONLY, 6:ACCOUNT_ONLY
  "updateTime": 1689744700710
}
```

###  Add IP Restriction for Sub-Account API key (For Master Account)

```shell
POST /openapi/v1/sub-account/apikey/add-ip-restriction
```

**Weight:** 1

**Parameters:**

Name       | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
apikey      | STRING | YES       |
email      | STRING | YES       | 	Sub account email
ipAddress      | STRING | NO        | 	Can be added in batches, separated by commas
ipRestriction      | STRING | YES       | 	IP Restriction status. 2 = IP Unrestricted. 1 = Restrict access to trusted IPs only.
recvWindow | LONG   | NO        | This value cannot be greater than `60000`
timestamp     | LONG   | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "apikey": "k5V49ldtn4tszj6W3hystegdfvmGbqDzjmkCtpTvC0G74WhK7yd4rfCTo4lShf",
  "ipList": [
    "8.8.8.8"
  ],
  "ipRestrict": true,
  "role": "2,3,4,5,6",//0:READ_ONLY, 2:TRADE_ONLY, 3:CONVERT_ONLY, 4:CRYPTO_WALLET_ONLY, 5:FIAT_ONLY, 6:ACCOUNT_ONLY
  "updateTime": 1689744700710
}
```

###  Delete IP List For a Sub-account API Key (For Master Account)

```shell
POST /openapi/v1/sub-account/apikey/delete-ip-restriction
```

**Weight:** 1

**Parameters:**

Name       | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
apikey      | STRING | YES       |
email      | STRING | YES       | 	Sub account email
ipAddress      | STRING | YES       | 	Can be added in batches, separated by commas
recvWindow | LONG   | NO        | This value cannot be greater than `60000`
timestamp     | LONG   | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "apikey": "k5V49ldtn4tszj6W3hystegdfvmGbqDzjmkCtpTvC0G74WhK7yd4rfCTo4lShf",
  "ipList": [
    "8.8.8.8"
  ],
  "ipRestrict": true,
  "role": "2,3,4,5,6",//0:READ_ONLY, 2:TRADE_ONLY, 3:CONVERT_ONLY, 4:CRYPTO_WALLET_ONLY, 5:FIAT_ONLY, 6:ACCOUNT_ONLY
  "updateTime": 1689744700710
}
```


###  Get Sub-account Deposit Address(For Master Account)

```shell
GET /openapi/v1/sub-account/wallet/deposit/address
```

Fetch sub account deposit address with network.


**Weight:** 10

**Parameters:**

Name       | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
email      | STRING | YES       | Sub account email
coin      | STRING | YES       | 	 The value is from All Coins' Information api
network      | STRING | YES       | 	The value is from All Coins' Information api
recvWindow | LONG   | NO        | This value cannot be greater than `60000`
timestamp     | LONG   | YES       | A point in time for which transfers are being queried.


**Response:**
```json
{
  "coin": "ETH",
  "address": "0xfe98628173830bf79c59f04585ce41f7de168784",
  "addressTag": ""
}
```

###  Get Sub-account Deposit History(For Master Account)

```shell
GET /openapi/v1/sub-account/wallet/deposit/history
```

Fetch deposit history.


**Weight(IP):** 2

**Parameters:**

| Name       | Type   | Mandatory | Description                                                  |
|------------| ------ |-----------| ------------------------------------------------------------ |
| email      | STRING | YES       | Sub account email                                                             |
| coin       | STRING | NO        |                                                              |
| txId       | STRING | NO        |                                                              |
| depositId  | STRING | NO        |                                                              |
| status     | INT    | NO        | 0-PROCESSING, 1-SUCCESS, 2-FAILED, 3-NEED_FILL_DATA(travel rule info) |
| startTime  | LONG   | NO        | Default: 90 days from current timestamp                      |
| endTime    | LONG   | NO        | Default: current timestamp                                   |
| offset     | INT    | NO        | Default:0                                                    |
| limit      | LONG   | NO        | Default:1000, Max:1000                                       |
| recvWindow | LONG   | NO        |                                                              |
| timestamp  | LONG   | YES       |                                                              |

* Please note the default `startTime` and `endTime` to ensure the time interval is within 0-90 days.

* If both `startTime` and `endTime` are sent, time between `startTime` and `endTime` must be less than 90 days.


**Response:**

```javascript
[
    {
        "id": "d_769800519366885376",
        "amount": "0.001",
        "coin": "BNB",
        "network": "BNB",
        "status": 0,
        "address": "bnb136ns6lfw4zs5hg4n85vdthaad7hq5m4gtkgf23",
        "addressTag": "101764890",
        "txId": "98A3EA560C6B3336D348B6C83F0F95ECE4F1F5919E94BD006E5BF3BF264FACFC",
        "insertTime": 1661493146000,
        "confirmNo": 10,
    },
    {
        "id": "d_769754833590042625",
        "amount":"0.5",
        "coin":"IOTA",
        "network":"IOTA",
        "status":1,
        "address":"SIZ9VLMHWATXKV99LH99CIGFJFUMLEHGWVZVNNZXRJJVWBPHYWPPBOSDORZ9EQSHCZAMPVAPGFYQAUUV9DROOXJLNW",
        "addressTag":"",
        "txId":"ESBFVQUTPIWQNJSPXFNHNYHSQNTGKRVKPRABQWTAXCDWOAKDKYWPTVG9BGXNVNKTLEJGESAVXIKIZ9999",
        "insertTime":1599620082000,
        "confirmNo": 20,
    }
]
```

#### Collect sub-account assets (For Master Account)
```shell
POST /openapi/v1/fund-collect/collect-from-sub-account
```

If there are tasks with a status of INIT, resubmission is not allowed. This interface will return `{'code': -10324, 'msg': 'Request repeated.'}`.

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
clientRequestId            | STRING | YES       | Request ID, must be unique, Maximum length: 200
remark | LONG   | NO        |  


**Response:**

```javascript
{
  "clientRequestId": "777d3f71-4715-4150-9fd1-d13246d7e02b",
  "status": "INIT",
  "comment": ""
}
```
**Response Description**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
clientRequestId            | STRING | YES       | Request ID
status            | STRING | YES        | Collection Task Status: `INIT`, `SUCCESS`, `PARTIAL_SUCCESS`, `PROCESSING`,`FAILED`
remark | LONG   | NO        |




#### Retrieve asset collection records (USER_DATA)

```shell
GET /openapi/v1/fund-collect/get-fund-record
```
Retrieve asset collection records.

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
clientRequestId            | STRING | NO        | Request ID, must be unique
page | INT   | NO        |  Page number 
size | INT   | NO        |  Page size (default:100,max:100)

**Response:**

```javascript
[
  {
    "clientRequestId": "09533266-1fea-11f0-8ff9-2a3efdea066c",
    "comment": "",
    "status": "SUCCESS"
  },
  {
    "clientRequestId": "09533266-1fea-11f0-8ff9-2a3efdea066b",
    "comment": "",
    "status": "SUCCESS"
  }
]
```
 Collection Task Status: `INIT`, `SUCCESS`, `PARTIAL_SUCCESS`, `PROCESSING`,`FAILED`

#### Get payment request (USER_DATA)

```shell
GET /openapi/v3/payment-request/get-payment-request (HMAC SHA256)
```
Retrieve either a single existing payment request or a list of existing payment requests.

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
id            | STRING | NO        | The ID of a specific payment request to retrieve.
start_time | LONG   | NO        |  The start time of a time range within which to search for payment requests.
end_time          | LONG   | NO       |  The end time of a time range within which to search for payment requests.
limit          | INT    | NO       | The maximum number of records to return in a single response. The default value is 500, and the maximum allowed value is 1000.
recvWindow | LONG   | NO        | The value cannot be greater than `60000`
timestamp          | LONG   | YES        |

**Response:**

```javascript
{
    "payment-request": {
        "message": "i am boss",
        "id": "1433341829953096704",
        "invoice": "1433341829953096704",
        "amount": "20",
        "currency": "PHP",
        "status": "pending",//pending,fully_paid,expired,canceled
        "created_at": "1685603661217",
        "updated_at": "1685603661217",
        "expires_at": "1686208461219",
        "supported_payment_collectors": "[\"coins_peso_wallet\"]",
        "payment_url": "https://www.pro.coins.ph/payment/invoice/1433341829953096704",
        "payer_contact_info": "jennins@coins.ph"
    }
}
```

#### Cancel payment request (USER_DATA)

```shell
POST /openapi/v3/payment-request/delete-payment-request (HMAC SHA256)
```
Cancel an existing payment request.

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
id            | STRING | YES       | The ID the payment request that needs to be canceled.
recvWindow | LONG   | NO        | The value cannot be greater than `60000`
timestamp          | LONG   | YES        |

**Response:**

```javascript
{
    "payment-request": {
        "message": "i am boss",
        "id": "1433341829953096704",
        "invoice": "1433341829953096704",
        "amount": "20",
        "currency": "PHP",
        "status": "canceled",//pending,fully_paid,expired,canceled
        "created_at": "1685603661217",
        "updated_at": "1685603661217",
        "expires_at": "1686208461219",
        "supported_payment_collectors": "[\"coins_peso_wallet\"]",
        "payment_url": "https://www.pro.coins.ph/payment/invoice/1433341829953096704",
        "payer_contact_info": "jennins@coins.ph"
    }
}
```

#### Send reminder for payment request (USER_DATA)

```shell
POST /openapi/v3/payment-request/payment-request-reminder (HMAC SHA256)
```
Send a reminder to the recipient to fulfill the payment request.

**Weight:** 1

**Parameters:**

Name              | Type   | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
id            | STRING | YES       | The ID of the payment request for which the reminder notification needs to be sent.
recvWindow | LONG   | NO        | The value cannot be greater than `60000`
timestamp          | LONG   | YES        |

**Response:**

```javascript
true
```



## Merchant Endpoints

### Signature


**Common Headers**

The table below shows all of the common API Headers you will encounter in the Coins API.

Header Name | Required | Type | Example | Description
------------ |----------|--| ------------|--
X-Merchant-Key | YES   | STRING |     | The authorized merchant key
X-Merchant-Sign | YES | STRING |   | The authorized merchant request sign
X-Timestamp | YES | LONG  | 1671158910| Request initiation time
X-Trace-Id | NO | STRING |    |  Request log trace ID

To craft an X-Merchant-Sign:
1. Construct a message according to the following pseudo-grammar: ‘X-Timestamp’ +URL(http://127.0.0.1/merchant-api/account?paramKey=paramValue&paramKey2=paramValue2) + BODY(key1=value1&key=value2)
2. Calculate an HMAC with the message string you just created, your API secret as the key, and SHA256 as the hash algorithm

### Invoicing

An invoice is a document that outlines the details of a transaction between two parties, typically a seller and a buyer. The transaction could be for goods or services rendered, or it could be a transfer of funds from one user to another.

In an invoice, there are two main entities involved:

1) The payee, who is the recipient of the payment for the goods or services provided (the merchant).
2) The payer, who is the individual or organization making the payment to fulfill the invoice (the customer).

The API endpoints described in this section allow you to integrate invoicing functionality into your application. Creating, sending, and managing invoices directly from the application simplifies the invoicing process and improves the user experience.

#### Creating Invoices


```shell
POST /merchant-api/v1/invoices (HMAC SHA256)
```

This endpoint generates an invoice based on the provided parameters and returns a response with details of the created invoice.

**Weight:** 1

**Parameters:**

Name              | Type  | Mandatory | Description
-----------------|-------|-----------|--------------------------------------------------------------------------------------
amount            | DECIMAL | YES       |The amount expected from the customer.
currency | STRING      | YES       | Currency of transaction.
supported_payment_collectors          | STRING  | YES       |Methods of payment that are available to a user when they view a payment request, optional items `coins_peso_wallet,CEBL,MLH,PLWN`,  e.g. `["coins_peso_wallet"]` or `["coins_peso_wallet","CEBL","MLH","PLWN"]`. Note: when a payment method is closed, it will be unavailable. 
external_transaction_id          | STRING  | YES       | To maintain transactional integrity, each transaction_id must be unique.
expires_at          | STRING  | NO        |The date and time at which the invoice will expire. This parameter accepts input in the ISO 8601 format for date and time, which is based on the Coordinated Universal Time (UTC) time zone (e.g., "2016-10-20T13:00:00.000000Z"). Alternatively, you can provide a time delta from the current time (Option:`1w,3d,2h,32m,5s`).

**Payment Options**

Code |Description
----|----
coins_peso_wallet|Pay with the user's Peso Coins wallet.


**Request:**

```javascript
{
    "amount": 100,
    "currency": "PHP",
    "supported_payment_collectors": "["coins_peso_wallet"]",
    "external_transaction_id": "1",
    "expires_at": "1w"
}
```

**Response:**

```javascript
{
    "invoice": {
        "id": "1783304323757262592",
        "amount": "100",
        "amount_due": "100",
        "currency": "PHP",
        "status": "pending",//pending,fully_paid,expired,canceled
        "external_transaction_id": "test_111",
        "created_at": "1690453041000",
        "updated_at": "1690453041000",
        "expires_at": "1690453041000",
        "supported_payment_collectors": "[\"coins_peso_wallet\"]",
        "payment_url": "http:xxxx",
        "expires_in_seconds": "60",
        "incoming_address":""
    }
}
```
#### Retrieving Invoices


```shell
GET /merchant-api/v1/get-invoices (HMAC SHA256)
```

This endpoint retrieves information about a specific invoice.

**Weight:** 1

**Parameters:**

Name              | Type  | Mandatory | Description
-----------------|-------|-----------|--------------------------------------------------------------------------------------
invoice_id            | STRING | NO        | The ID of a specific invoice to retrieve.
start_time            | LONG  | NO        | The start time of a time range within which to search for invoices.
end_time            | LONG  | NO        | The end time of a time range within which to search for invoices.
limit            | INT   | NO        | The maximum number of records to return in a single response. The default value is 500, and the maximum allowed value is 1000.

If the invoice_id parameter is provided, only the data for the specified invoice will be returned.
If the start_time and end_time parameters are not provided, the response will include the records within the last 90 days by default. Developers can provide a specific time range by setting the time parameter to a value that specifies the start and end times of the desired range.

**Response:**

```javascript
{
    "invoice": [{
        "id": "1783304323757262592",
        "amount": "100",
        "amount_due": "100",
        "currency": "PHP",
        "status": "pending",//pending,fully_paid,expired,canceled
        "external_transaction_id": "test_111",
        "created_at": "1690453041000",
        "updated_at": "1690453041000",
        "expires_at": "1690453041000",
        "supported_payment_collectors": "[\"coins_peso_wallet\"]",
        "payment_url": "http:xxxx",
        "expires_in_seconds": "60",
        "incoming_address":""
    }]
}
```


#### Canceling Invoices


```shell
POST /merchant-api/v1/invoices-cancel (HMAC SHA256)
```

This endpoint cancels an existing invoice.

**Weight:** 1

**Parameters:**

Name              | Type  | Mandatory | Description
-----------------|-------|-----------|--------------------------------------------------------------------------------------
invoice_id            | STRING | YES       | The ID of a specific invoice to cancel.

**Response:**

```javascript
{
    "invoice": {
        "id": "1783304323757262592",
        "amount": "100",
        "amount_due": "100",
        "currency": "PHP",
        "status": "canceled",//pending,fully_paid,expired,canceled
        "external_transaction_id": "test_111",
        "created_at": "1690453041000",
        "updated_at": "1690453041000",
        "expires_at": "1690453041000",
        "supported_payment_collectors": "[\"coins_peso_wallet\"]",
        "payment_url": "http:xxxx",
        "expires_in_seconds": "60",
        "incoming_address":""
    }
}
```

### Invoice Callbacks

During the lifecycle of an invoice, various events may occur. For example, when an invoice is fully paid, the invoice.fully_paid event is triggered. These events can be tracked and acted upon using the Coins API's event system.

Merchants can specify a callback URL when creating or updating an invoice, which is a web address that the API will send event data to. When an event occurs, the API will send a POST request to the specified callback_URL, containing data about the event. The merchant can then process this data as needed, such as by updating their internal systems or notifying the customer.

To ensure that the events are delivered securely, merchants must include an authorization header with their Merchant API key in each POST request. This header, with the format Authorization: Token MERCHANT_APIKEY, confirms that the request is coming from a trusted source and provides an additional layer of security for the event data.

Event payloads follow this convention:

**Request Url**

Please contact our partner support team to configure the callback address.

**Request Heder**

Name             | Type  | Mandatory | Description
-----------------|-------|-----------|--------------------------------------------------------------------------------------
Authorization    | STRING | YES       | The merchant API key used when creating an invoice

**Body**

```javascript
{
  "event": {
    "name": "invoice.name",//event
    "data": {
        "id": "invoice_id",
        "currency": "PHP",
        "amount": "100",
        "amount_received": "0",
        "external_transaction_id": "1"
        }
    }
}
```

Events which may be consumed by callbacks are described in the table below:

Event Name	| Description
----|---
invoice.created	| The invoice has been created.
invoice.updated	| The invoice has been updated. This may be due to the payment received for the invoice.
invoice.fully_paid	| The invoice payment has been completed.
invoice.payment_reference_number_generated| The invoice payment reference number has been generated.



### Old endpoints from coins.ph (Legacy)

#### Create a new sellorder
```shell
POST openapi/migration/v4/sellorder
```

This endpoint converts digital assets into real-world cash, making it easy for users to withdraw their funds as cash. It provides a streamlined and secure process for cashing out, ensuring that users can access their funds quickly and easily. Additionally, this API endpoint ensures that users' financial information is kept secure and confidential, providing a safe and reliable way to withdraw funds.
It is best to no longer use this endpoint; it is only compatible with the old endpoint.

Name        | Type   | Mandatory | Description
------------|--------|-----------| ------------
payment_outlet | STRING | Yes       | The payment outlet used to transfer funds to another Coins wallet. Once the sell order is completed, the fiat cashout will be processed through the chosen payout outlet and the funds will be transferred to the specified destination wallet via coins_transfer.
base_amount      | STRING | Yes       | The quantity of digital assets that the user wishes to sell and the corresponding amount of fiat cash that they will receive in exchange.
currency    | STRING | Yes       |  The currency symbol of the sell order.
bank_account_number       | STRING | Yes       | Cash out bank account number
bank_account_name        | STRING | Yes       | Cash out bank account name
recipient_phone_number      | STRING | Yes       | Recipient phone number
recipient_bank_code       | STRING | No       | Recipient bank code
external_transaction_id          | STRING | No        |  A unique identifier for the sell order, should be number.
pay_with_wallet         | STRING | No       | The wallet code from which the user wishes to initiate the sell order.
sender_first_name	| STRING | No       | sender first name.
sender_middle_name	| STRING | No       | sender middle name.
sender_last_name	| STRING | No       | sender last name.
sender_address	| STRING | No       | sender address.
sender_city	| STRING | No       | sender city.
sender_province	| STRING | No       | sender province.
sender_country	| STRING | No       | sender country.
sender_postal_code	| STRING | No       | sender postal code.
sender_phone	| STRING | No       | sender phone.
sender_email	| STRING | No       | sender email.
sender_company_name	| STRING | No       | sender company name.

**Request:**

```javascript
{
  "base_amount": "10",
  "external_transaction_id":"12121212212121212", // number not string
  "currency":"PHP",
  "bank_account_number":"9163011937",  // recipient phone （pesonet）or credit card numbers （instapy）
  "bank_account_name":"Mike",  // recipient name
  "recipient_phone_number":"9163011937", // recipient phone
  "payment_outlet":"citi_SWIFTPAY_PESONET"
}
```

**Weight:** 1

reference: https://docs.coins.asia/reference/sellorder-create

------

#### Retrieve an existing sellorder
```shell
GET openapi/migration/v4/sellorder/{sell_order_id}
```

This endpoint retrieves information about a previously executed sell order that involved a cash-out of fiat currency.
It is best to no longer use this endpoint; it is only compatible with the old endpoint.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------| ------------ | ------------
sell_order_id       | STRING | No |  A unique identifier that is used to specify the sell order that a user wants to retrieve information about.


**Weight:** 1

reference: https://docs.coins.asia/reference/sellorder-retrieve

------

#### Validate field values
```shell
POST openapi/migration/v4/validate-field
```

This endpoint validates field values for fiat sell order to ensure that the values provided for the different fields are valid before the actual cashout process begins.
It is best to no longer use this endpoint; it is only compatible with the old endpoint.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------| ------------ | ------------
field_type       | STRING | No |  The type of field that needs to be validated. Accepts two valid values: "account_number" and "mobile_number"..
account_number    | STRING | No |  When the field_type parameter is set to "account_number", this parameter is used to specify the account number that needs to be validated for the fiat sell order.
account_type      | STRING | No | When the field_type parameter is set to "account_number", this parameter is used to specify the payment outlet ID where the account number field value should be validated against.
mobile_number     | STRING | No | When the field_type parameter is set to "mobile_number", this parameter is used to specify the mobile number that needs to be validated for the fiat sell order. The mobile number provided must be in the correct format, following the E.164 phone number formatting, and associated with the correct user.
region       | STRING | No |  When the field_type parameter is set to "mobile_number", this parameter is used to specify the region of the mobile number that needs to be validated for the fiat sell order. Valid values for the region parameter are "PH" and "TH", which represent the regions of the Philippines and Thailand, respectively.


**Weight:** 1

reference: https://docs.coins.asia/reference/validate-field

------

#### Retrieve supported payout-outlets
```shell
GET openapi/migration/v4/payout-outlets/{id}
```

This endpoint retrieves a list of supported payout outlets for fiat sell orders. It is designed to provide users with a comprehensive list of available payout options so they can select the one that is most convenient for them.
It is best to no longer use this endpoint; it is only compatible with the old endpoint.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------|-----------| ------------
id       | STRING | No       |  A unique identifier for the payout outlet.
outlet_category       | STRING | No       |  A payment outlet category ID that is used to filter the list of supported payout outlets.
name       | STRING | No       | A payment outlet name that is used to filter the list of supported payout outlets.
region       | STRING | No       |The name of a region that is used to filter the list of supported payout outlets.
is_enabled       | STRING | No     | A boolean value that determines whether or not to include disabled payout outlets in the results.


**Weight:** 1

reference: https://docs.coins.asia/reference/payout-outlets

------

#### Retrieve supported payout-outlet-categories
```shell
GET openapi/migration/v4/payout-outlet-categories/{id}
```

This endpoint retrieves the list of payout outlet categories that are supported for fiat sell orders. Payout outlet categories are used to classify and organize the different payout outlet options that are available to users when they choose to cash out their fiat currency. This endpoint takes no parameters and returns a JSON response that includes an array of payout outlet categories.
It is best to no longer use this endpoint; it is only compatible with the old endpoint.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------|-----------| ------------
id       | STRING | No       |   ID of a particular category to retrieve. If not provided, all categories will be retrieved.


**Weight:** 1

reference: https://docs.coins.asia/reference/payout-outlet-categories


------

#### Retrieve current payout-outlet-fees
```shell
GET openapi/migration/v4/payout-outlet-fees
```

This endpoint retrieves the current payout outlet fees for the supported payout outlets for fiat sell orders. Payout outlet fees are the fees charged by the payout outlet providers for processing the cash-out transactions. This endpoint takes no parameters and returns a JSON response that includes an array of payout outlet fees.
It is best to no longer use this endpoint; it is only compatible with the old endpoint.

**Parameters:**

Name           | Type   | Mandatory | Description
----------------|--------|-----------| ------------
payment_outlet       | STRING | Yes       |   query param to specify the target payout outlet.
currency       | STRING | Yes       |   query param to specify the target currency.


**Weight:** 1

reference: https://docs.coins.asia/reference/payout-outlet-fees

#### Query balance (USER_DATA)

```shell
GET /openapi/account/v3/crypto-accounts
```

This endpoint allows users to retrieve their current account balance.

**Weight:** 1
**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------
currency      | STRING | NO    | The currency for which the balance is being queried.
recvWindow | LONG  | YES    | This value cannot be greater than `60000`
timestamp     | LONG  | YES    | A point in time for which the balance is being queried.

**Response:**
```javascript
 {
  "crypto-accounts": [
    {
      "id": "1451431230880900352",
      "name": "name",
      "currency": "PBTC",
      "balance": "100",
      "pending_balance": "200"
    }
  ]
}
```

#### Query transfers (USER_DATA)

```shell
GET /openapi/transfer/v3/transfers/{id}
```
If an ID is provided, this endpoint retrieves an existing transfer record; otherwise, it returns a paginated list of transfers.

**Weight:** 10

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
id      | STRING | NO    | ID of the transfer record
client_transfer_id| STRING | NO | Client Transfer ID, Maximum length 100
page    | INT | NO | Current page, default is `1`
per_page    | INT | NO | Quantity per page, default 2000, maximum `2000`
from_address |STRING|NO| The phone number or email for sender account (e.g. +63 9686490252 or testsub@gmail.com)
to_address  |STRING|NO| The phone number or email for recipient account (e.g. +63 9686490252 or testsub@gmail.com)
recvWindow | LONG  | YES    | This value cannot be greater than `60000`
timestamp     | LONG  | YES    | A point in time for which transfers are being queried.

- If client_transfer_id both the id and  parameters are passed, the id parameter will take precedence.
- If the client_transfer_id or id parameter is passed, then the client_transfer_id or id takes precedence.
- The from_address and to_address parameters cannot be passed simultaneously.

**Response:**
```json
 {
  "transfers": [
    {
      "id": "2309rjw0amf0sq9me0gmadsmfoa",
      "client_transfer_id": "1487573639841995270",
      "account": "90dfg03goamdf02fs",
      "amount": "1",
      "fee_amount": "0",
      "currency": "PBTC",
      "sourceAddress": "test1@gmail.com",
      "target_address": "test2@gmail.com",
      "payment": "23094j0amd0fmag9agjgasd",
      "type": 2,//2:transfer out,1:transfer in
      "status": "success",
      "message": "example",
      "created_at": "2019-07-04T03:28:50.531599Z"
    }
  ],
  "meta": {
    "total_count": 0,
    "next_page": 2,
    "previous_page": 0
  }
}
```



### Note

### Request Parameters

- Email address should be encoded. e.g. test@gmail.com should be encoded into test%40gmail.com


#### Query transaction history (USER_DATA)

```shell
GET /openapi/v1/asset/transaction/history
```

**Weight:** 10

**Parameters:**

Name       | Type  | Mandatory | Description
-----------------|--------|-----------|--------------------------------------------------------------------------------------
tokenId      | STRING | YES       | The token to retrieve data for. Example: “tokenId”: “BTC”
startTime| LONG | NO        | Timestamp in milliseconds. Timespan between startTime and endTime cannot exceed 7 days. If only startTime is provided, returns data 7 days after.
endTime| LONG | NO        | Timestamp in milliseconds. Timespan between startTime and endTime cannot exceed 7 days. If only endTime is provided, returns data 7 days before.
subUserId    | LONG | NO        | UID of sub-account.
pageNum    | INT | NO        | The current page number. Ranges from 1 - 1000, with default being 1.
pageSize    | INT | NO        | Records per page. Ranges from 1 - 100, with default being 20.
recvWindow | LONG  | NO         | To specify the number of milliseconds after timestamp the request is valid for. Must be less than 60000.
timestamp     | LONG  | YES       | A point in time for which transfers are being queried. Unix timestamp format in milliseconds.

**Response:**
```json
 {
  "transfers": [
    {
      "txId": "2309rjw0amf0sq9me0gmadsmfoa",
      "bizSubject": "CHAIN_WITHDRAWAL",
      "tokenId": "BTC",
      "status": "success",
      "changed": "1",
      "time": "1742896126999"
    }
  ],
  "meta": {
    "has_next": true,
    "next_page": 2,
    "previous_page": 0
  }
}
```



