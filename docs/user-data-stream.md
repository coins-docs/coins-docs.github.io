---
permalink: /user-data-stream/
title: "User-Data-Stream"
nav: sidebar/user-data-stream.html
layout: default
---


# Change Log
2023-07-15: update `balanceUpdate` trigger condition

2022-08-19: add weight infomation for all interfaces


# User Data Streams for Coins (2022-08-19)

## General WSS information

* The base endpoint is: **https://api.pro.coins.ph**
* A User Data Stream `listenKey` is valid for 60 minutes after creation.
* Doing a `PUT` on a `listenKey` will extend its validity for 60 minutes.
* Doing a `DELETE` on a `listenKey` will close the stream and invalidate the listenKey.
* Doing a `POST` on an account with an active `listenKey` will return the currently active `listenKey` and extend its validity for 60 minutes.
* The base websocket endpoint is: **wss://wsapi.pro.coins.ph**
* User Data Streams are accessed at **/openapi/ws/\<listenKey\>**
* A single connection to api endpoint is only valid for 24 hours; expect to be disconnected at the 24 hour mark

## API Endpoints

### Create a listenKey

```shell
POST /openapi/v1/userDataStream
```

Start a new user data stream. The stream will close after 60 minutes unless a keepalive is sent.that listenKey will be returned and its validity will be extended for 60 minutes.

**Weight:** 1

**Parameters:**

None

**Response:**

```javascript
{
  "listenKey": "1A9LWJjuMwKWYP4QQPw34GRm8gz3x5AephXSuqcDef1RnzoBVhEeGE963CoS1Sgj"
}
```

### Ping/Keep-alive a listenKey

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

### Close a listenKey

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

## Web Socket Payloads

### Account Update

`outboundAccountPosition` is sent any time an account balance has changed and contains the assets that were possibly changed by the event that generated the balance change.

**Payload:**
balance snapshot

```javascript
{
  "e": "outboundAccountPosition", // Event Type
  "E": 1564034571105,             // Event Time
  "u": 1564034571073,             // Account Last Update time
  "B": [                          // balance
    {
      "a": "ETH",                 // asset
      "f": "10000.000000",        // free
      "l": "0.000000"             // locked
    }
  ]
}
```


### Balance Update

`balanceUpdate`  is pushed upon non trading fund activities - transfer / deposit / withdrawal

**Payload:**
contains balance changed

```javascript
{
  "e": "balanceUpdate",         //Event Type
  "E": 1573200697110,           //Event Time
  "a": "ABC",                   //Asset
  "d": "100.00000000",          //Balance Delta
  "T": 1573200697068            //Clear Time
}
```

### Order Update

Orders are updated with the `executionReport` event. Check the API documentation and below for relevant enum definitions.
Average price can be found by doing `Z` divided by `z`.

**Payload:**

```javascript
{
  "e": "executionReport",        // Event type
  "E": 1499405658658,            // Event time
  "s": "ETHBTC",                 // Symbol
  "c": "1000087761",             // Client order ID
  "S": "BUY",                    // Side
  "o": "LIMIT",                  // Order type
  "f": "GTC",                    // Time in force
  "q": "1.00000000",             // Order quantity
  "p": "0.10264410",             // Order price
  "P": "0.00000000",             // Stop price
  "x": "NEW",                    // Current execution type
  "X": "NEW",                    // Current order status
  "r": "NONE",                   // Order reject reason
  "i": 1241518645726809840,      // Order ID
  "l": "0.00000000",             // Last executed quantity
  "z": "0.00000000",             // Cumulative filled quantity
  "L": "0.00000000",             // Last executed price
  "n": "0",                      // Commission amount
  "N": null,                     // Commission asset
  "T": -1,                       // Transaction time
  "t": -1,                    	 // Trade ID
  "w": true,                     // Is the order on the book?
  "m": false,                    // Is this trade the maker side?
  "O": 1499405658657,            // Order creation time
  "Z": "0.00000000"              // Cumulative quote asset transacted quantity
  "Y": "0.00000000",             // Last quote asset transacted quantity (i.e. lastPrice * lastQty)
  "Q": "0.00000000"              // Quote Order Qty
}
```

**Execution types:**

* NEW \- The order has been accepted into the engine.
* CANCELED \- The order has been canceled by the user.
* REJECTED  \- The order has been rejected and was not processed.
* TRADE - Part of the order or all of the order's quantity has filled.
* EXPIRED - The order was canceled according to the order type's rules (e.g. LIMIT FOK orders with no fill, LIMIT IOC or MARKET orders that partially fill) or by the exchange.