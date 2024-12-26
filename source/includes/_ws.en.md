# Websocket

## Overview

Websocket is a new HTML5 Protocol. It achieves full-duplex data transmission between the client and the server, allowing data to be transferred effectively in both directions. With just only one handshake, the connection between the client and the server is established. The server will then be able to push data to the client according to preset rules. Its advantages include:

* The WebSocket request header for data transmission between client and server is approximately 2 bytes only.
* Either the client or server can initiate a data transmission.
* As there is no need to create and delete TCP connection repeatedly, it saves resources for both bandwidth and server.

<aside class="notice">We highly recommend that developers use the WebSocket API for retrieving market data and order book depth.</aside>

## Ws Information

* Basic spot market data site: <wss://wspool.hiotc.pro/kline-api/ws>
* Basic futures market data site: <wss://wspool.xxx.yyy/kline-api/ws>, Replace xxx.xxx with the exchange's main domain.
* The returned data will be binary compressed except the heartbeat data (the user needs to decompress through Gzip algorithm).

### Heartbeat

To maintain a valid and stable connection, it is recommended to perform the following steps:

1. After receiving a message, the user sets a timer for N seconds, where N is less than 30.

2. If the timer is triggered (no new messages are received within N seconds), send the string 'ping'.

3. Expect a text string 'pong' as a response. If no response is received within N seconds, raise an error or reconnect.

> response example

```json
{
    "pong": 15359750
}
```

### Demo

[Websocket Demo](https://github.com/exchange2021/openapidemo/blob/master/src/main/java/com/ws/WsTest.java)



## Command Format

| event   | channel                       | description                                |
| :-------| :-----------------------------| :------------------------------------------|
| `sub`   | `market_$symbol_depth_step0`  | `Subscribe to depth`                       |
| `unsub` | `market_$symbol_depth_step0`  | `Unsubscribe from depth`                   |
| `sub`   | `market_$symbol_trade_ticker` | `Subscribe to real-time trade`             |
| `unsub` | `market_$symbol_trade_ticker` | `Unsubscribe from real-time trade`         |
| `sub`   | `market_$symbol_ticker`       | `Subscribe to 24h market data`             |
| `unsub` | `market_$symbol_ticker`       | `Unsubscribe from 24h market data`         |
| `sub`   | `market_$symbol_kline_1min`   | `Subscribe to 1min k-line information`     |
| `req`   | `market_$symbol_kline_1month` | `Request 1 month historical k-line record` |


## Subscription

### Subscribe Full Depth

> sub example

```json
{
    "event": "sub",
    "params": {
        "channel": "market_$symbol_depth_step0", // $symbol E.g. Spot: btcusdt, Futures: e_btcusdt
        "cb_id": "1" // Business ID is not required
    }
}
```

> response example

```json
{
    "channel": "market_btcusdt_depth_step0",
    "ts": 1506584998239,
    "tick": {
        "asks": [ //asks
            [
                10000.19,
                0.93
            ],
            [
                10001.21,
                0.2
            ],
            [
                10002.22,
                0.34
            ]
        ],
        "bids": [ //buys
            [
                9999.53,
                0.93
            ],
            [
                9998.2,
                0.2
            ],
            [
                9997.19,
                0.21
            ]
        ]
    }
}
```

### Subscribe to Real Time Trade

> sub example

```json
{
    "event": "sub",
    "params": {
        "channel": "market_$symbol_trade_ticker", // $symbol E.g. Spot: btcusdt, Futures: e_btcusdt
        "cb_id": "1" // Business ID is not required
    }
}
```

> response example

```json
{
    "channel": "market_$symbol_trade_ticker",
    "ts": 1506584998239, // request time
    "tick": {
        "id": 12121, // maximum trade ID in the data
        "ts": 1506584998239, // maximum time in the data
        "data": [
            {
                "side": "buy", // trade side
                "price": 32.233, // price
                "vol": 232, // volume
                "amount": 323, // total amount
                "ds": "2017-09-1023: 12: 21"
            }
        ]
    }
}
```
### Subscribe to K-Line Market

> sub example

```json
{
    "event": "sub",
    "params": {
        "channel": "market_$symbol_kline_[1min/5min/15min/30min/60min/1day/1week/1month]", // $symbol E.g. btcusdt
        "cb_id": "1" // Business ID is not required
    }
}
```

> response example

```json
{
    "channel": "market_$symbol_kline_1min", // 1min is for 1 minute
    "ts": 1506584998239, // request time
    "tick": {
        "id": 1506602880, // k-line start time
        "vol": 1212.12211, // trade volume
        "open": 2233.22, // open price
        "close": 1221.11, // close price
        "high": 22322.22, // high price
        "low": 2321.22 // low price
    }
}
```

### Subscribe to Market Tickers

> sub example

```json
{
    "event": "sub",
    "params": {
        "channel": "market_$symbol_ticker", // $symbol E.g. Spot: btcusdt, Futures: e_btcusdt
        "cb_id": "1" // Business ID is not required
    }
}
```

> response example

```json
{
    "channel": "market_$symbol_ticker",
    "ts": 1506584998239, // request time
    "tick": {
        "amount": 123.1221, // trade amount
        "vol": 1212.12211, // trade volume
        "open": 2233.22, // open price
        "close": 1221.11, // close price
        "high": 22322.22, // high price
        "low": 2321.22, // low price
        "rose": -0.2922, // increase
    }
}
```

### Request K-Line History Data

> sub example

```json
{
    "event": "req",
    "params": {
        "channel": "market_$symbol_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",
        "cb_id": "1",
        "endIdx": "1506602880", // Return pageSize data before endIdx Not required
        "pageSize": 100 // Not required
    }
}
```

> response example

```json
{
    "event_rep": "rep",
    "channel": "market_$symbol_kline_5min",
    "cb_id": "return along the same path",
    "ts": 1506584998239, // request time
    "data": [ // up to 300 pieces
        {
            "id": 1506602880, // k-line start time
            "amount": 123.1221, // trade amount
            "vol": 1212.12211, // trade volume
            "open": 2233.22, // open price
            "close": 1221.11, // close price
            "high": 22322.22, // high price
            "low": 2321.22 // low price
        },
        {
            "id": 1506602880, // k-line start time
            "amount": 123.1221, // trade amount
            "vol": 1212.12211, // trade volume
            "open": 2233.22, // open price
            "close": 1221.11, // close price
            "high": 22322.22, // high price
            "low": 2321.22 // low price
        }
    ]
}
```

### Request History Trade

> sub example

```json
{
    "event": "req",
    "params": {
        "channel": "market_$symbol_trade_ticker", // $symbol E.g. Spot: btcusdt, Futures: e_btcusdt
        "cb_id": "1" // Business ID is not required
    }
}
```

> response example

```json
{
    "event_rep": "rep",
    "channel": "market_$symbol_trade_ticker",
    "cb_id": "return along the same path",
    "ts": 1506584998239,
    "status": "ok",
    "data": [
        {
            "side": "buy", // trade side, buy or sell
            "price": 32.233, // price
            "vol": 232, // volume
            "amount": 323 // total amount
        },
        {
            "side": "buy", // trade side, buy or sell
            "price": 32.233, // price
            "vol": 232, // volume
            "amount": 323 // total amount
        }
    ]
}
```