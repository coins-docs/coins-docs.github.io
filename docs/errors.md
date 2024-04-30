---
title: "Errors"
permalink: /errors/
layout: default
nav: sidebar/errors.html

---
# Error codes for Coins (2024-04-30)

Errors consist of two parts: an error code and a message. Codes are universal,
 but messages can vary. Here is the error JSON payload:

```javascript
{
  "code": -1000,
  "msg": "An unknown error occurred while processing the request."
}
```

## 10xx - General Server or Network issues

### -1000 UNKNOWN_ERROR

* An unknown error occurred while processing the request.

### -1001 DISCONNECTED

* Internal error.

### -1002 UNAUTHORIZED

* You are not authorized to execute this request
* How to resolve: Put X-COINS-APIKEY in http header, refer to api doc "Endpoint Security Type" part.

### -1003 TOO_MANY_REQUESTS

* Too many requests, current limit is %s requests per %s.
* How to resolve: Your request is breaking our rate limit, if you need higher rate limit, please contact us.

### -1010 BAD_REQUEST

* Bad request

### -1015 TOO_MANY_ORDERS

* Too many new orders; current limit is %s orders per %s.

### -1020 UNSUPPORTED_OPERATION

* This operation is not supported.
* How to resolve: Please create and use functional API key instead of using read-only API key.

### -1021 TIMESTAMP_OUT_OF_WINDOW

* Timestamp for this request is outside of the recv window.
* How to resolve: The request is valid if server timestamp <= (recvWindow + timestamp), refer to API doc "Timing Security" part.

### -1022 INVALID_SIGNATURE

* Signature for this request is not valid.
* How to resolve: Your signature is not valid, refer to API doc "SIGNED Endpoint Examples for POST /openapi/v1/order" part.

### -1023 BIND_IP_WHITE_LIST_FIRST

* set ip white_list before use
* How to resolve: You need to set up IP white list for your api key.

### -1024 MISS_HEADER_ERROR

* Header '%s' is required.
* How to resolve: Missing parameter in http header.

### -1025 INVALID_PARAMETER

* Parameter '%s' is not valid.
* How to resolve: The parameter passed is not valid.
  
### -1026 CREATE_LISTEN_KEY_RATE_LIMIT

* Create listenKey rate limited(%s per hour), please try again next hour
* How to resolve: Create listenKey request is breaking our rate limit.

### -1027 INVALID_ORG_ID

* This api only support for coins.ph.
* How to resolve: This api is not supported in coins.xyz.

  
## 11xx - Request issues

### -1103 UNKNOWN_PARAM

* Required param not found, please ensure the parameters being sent correctly, more information please refer to the openapi documentation 'SIGNED Endpoint Examples' section.

### -1105 MISS_PARAMETER

* Parameter '%s' is required.

### -1106 PARAMETER_NOT_REQUIRED

* Parameter '%s' sent when not required

### -1107 CONFLICT_PARAMETER_ERROR

* Parameter '%s' should not be both set.

### -1108 ORDER_CANCEL_REPLACE_PARAMETER_ERROR

* Either the cancelOrigClientOrderId or cancelOrderId must be provided

### -1125 LISTEN_KEY_EXPIRED

* This listenKey does not exist

### -1126 CREATE_LISTEN_KEY_FAILED

* Create listenKey failed, please try again later

### -1131 INSUFFICIENT_BALANCE

* Balance insufficient

### -1132 ORDER_CANCEL_REPLACE_FAILED

* Order cancel-replace failed.

### -1133 ORDER_CANCEL_REPLACE_PARTIALLY_FAILED

* Order cancel-replace partially failed.

### -1150 MERCHANT_AUTHENTICATION_FAIL

* merchant authentication failed.

### -1151 USER_NOT_FOUND

* merchant user not found

### -1152 NO_PERMISSION

* Unauthorized to operate this API.


### -2010 NEW_ORDER_REJECTED

* New order rejected.

### -2011 CANCEL_REJECTED

* CANCEL_REJECTED

### -2013 NO_SUCH_ORDER

* Order does not exist.

### -2015 API_KEY_NOT_ENABLE

* API-key is not enabled.
* How to resolve: Cannot find the api key or the api key is not enabled.

### -2017 IP_NOT_IN_WHITELIST

* Request ip is not in the whitelist
* How to resolve: The request IP is not in the IP white list of your api key, please update your api key. You can get your current IP by endpoint openapi/v1/user/ip.

### -2018 API_KEY_NOT_EXIST

* API-key does not exist.
* How to resolve: Cannot find the api key.

### -2019 API_KEY_TYPE_WRONG

* API-key type is wrong.

## Messages for -1010 BAD_REQUEST, -2010 NEW_ORDER_REJECTED and -2011 CANCEL_REJECTED

This code is sent when an error has been returned by the matching engine.
The following messages which will indicate the specific error:

| Error message                                                | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| "Unknown order sent."                                        | The order (by either `orderId`, `clOrdId`, `origClOrdId`) could not be found |
| "Duplicate order sent."                                      | The `clOrdId` is already in use                              |
| "Market is closed."                                          | The symbol is not trading                                    |
| "Account has insufficient balance for requested action."     | Not enough funds to complete the action                      |
| "Market orders are not supported for this symbol."           | `MARKET` is not enabled on the symbol                        |
| "Iceberg orders are not supported for this symbol."          | `icebergQty` is not enabled on the symbol                    |
| "Stop loss orders are not supported for this symbol."        | `STOP_LOSS` is not enabled on the symbol                     |
| "Stop loss limit orders are not supported for this symbol."  | `STOP_LOSS_LIMIT` is not enabled on the symbol               |
| "Take profit orders are not supported for this symbol."      | `TAKE_PROFIT` is not enabled on the symbol                   |
| "Take profit limit orders are not supported for this symbol." | `TAKE_PROFIT_LIMIT` is not enabled on the symbol             |
| "Price* QTY is zero or less."                                | `price`* `quantity` is too low                               |
| "IcebergQty exceeds QTY."                                    | `icebergQty` must be less than the order quantity            |
| "This action disabled is on this account."                   | Contact customer support; some actions have been disabled on the account. |
| "Unsupported order combination"                              | The `orderType`, `timeInForce`, `stopPrice`, and/or `icebergQty` combination isn't allowed. |
| "Order would trigger immediately."                           | The order's stop price is not valid when compared to the last traded price. |
| "Cancel order is invalid. Check origClOrdId and orderId."    | No `origClOrdId` or `orderId` was sent in.                   |
| "Order would immediately match and take."                    | `LIMIT_MAKER` order type would immediately match and trade, and not be a pure maker order. |

## -9xxx Filter failures

| Error message                            | Description                                                  |
| ---------------------------------------- | ------------------------------------------------------------ |
| "Filter failure: PRICE_FILTER"           | `price` is too high, too low, and/or not following the tick size rule for the symbol. |
| "Filter failure: LOT_SIZE"               | `quantity` is too high, too low, and/or not following the step size rule for the symbol. |
| "Filter failure: MIN_NOTIONAL"           | `price`* `quantity` is too low to be a valid order for the symbol. |
| "Filter failure: MAX_NUM_ORDERS"         | Account has too many open orders on the symbol.              |
| "Filter failure: MAX_ALGO_ORDERS"        | Account has too many open stop loss and/or take profit orders on the symbol. |
| "Filter failure: BROKER_MAX_NUM_ORDERS"  | Account has too many open orders on the broker.              |
| "Filter failure: BROKER_MAX_ALGO_ORDERS" | Account has too many open stop loss and/or take profit orders on the broker. |
| "Filter failure: ICEBERG_PARTS"          | Iceberg order would break into too many parts; icebergQty is too small. |
