# Spot Trading API

## Public

### Security: None

<aside class='notice'>Public section can be accessed freely without requiring any API-key or signatures.</aside>

### Test Connectivity

`GET https://openapi.xxx.com/api/v1/ping`

Test the connectivity of the REST API.

> request example

```http
GET https://openapi.xxx.com/api/v1/ping
```

### Check Server Time

`GET https://openapi.xxx.com/api/v1/time`

Check Server Time

> request example

```http
GET https://openapi.xxx.com/api/v1/time
```

> response example

```json
{
    "timezone": "GMT+08:00",
    "serverTime": 1595563624731
}
```

**Response Parameter**

| Parameter  | Type   | Example               | Description      |
| :----------| :------| :---------------------| :----------------|
| timezone   | string | `China Standard Time` | Server time zone |
| serverTime | long   | `1705039779880`       | Server timestamp |

### Symbol Pair List 

`GET https://openapi.xxx.com/api/v1/symbols`

The supported symbol pair collection which in the exchange.

> request example

```http
GET https://openapi.xxx.com/api/v1/symbols
```

> response example

```json
{
    "symbols": [
        {
            "quantityPrecision": 3,
            "limitVolumeMin": 0.0001,
            "symbol": "sccadai",
            "pricePrecision": 6,
            "marketBuyMin": 0.0001,
            "marketSellMin": 0.0001,
            "baseAsset": "SCCA",
            "limitPriceMin": 0.001,
            "quoteAsset": "DAI"
        },
        {
            "quantityPrecision": 8,
            "limitVolumeMin": 0.0001,
            "symbol": "btcusdt",
            "pricePrecision": 2,
            "marketBuyMin": 0.0001,
            "marketSellMin": 0.0001,
            "baseAsset": "BTC",
            "limitPriceMin": 0.001,
            "quoteAsset": "USDT"
        },
        {
            "quantityPrecision": 3,
            "limitVolumeMin": 0.0001,
            "symbol": "bchusdt",
            "pricePrecision": 2,
            "marketBuyMin": 0.0001,
            "marketSellMin": 0.0001,
            "baseAsset": "BCH",
            "limitPriceMin": 0.001,
            "quoteAsset": "USDT"
        },
        {
            "quantityPrecision": 2,
            "limitVolumeMin": 0.0001,
            "symbol": "etcusdt",
            "pricePrecision": 2,
            "marketBuyMin": 0.0001,
            "marketSellMin": 0.0001,
            "baseAsset": "ETC",
            "limitPriceMin": 0.001,
            "quoteAsset": "USDT"
        },
        {
            "quantityPrecision": 2,
            "limitVolumeMin": 0.0001,
            "symbol": "ltcbtc",
            "pricePrecision": 6,
            "marketBuyMin": 0.0001,
            "marketSellMin": 0.0001,
            "baseAsset": "LTC",
            "limitPriceMin": 0.001,
            "quoteAsset": "BTC"
        }
    ]
}
```

**Response Parameter**

| Parameter         | Type       | Example   | Description                                |
| :-----------------| :----------| :---------| :------------------------------------------|
| symbol            | string     | `btcusdt` | `Lowercase` symbol name                    |
| baseAsset         | string     | `BTC`     | `Base` asset for the symbol                |
| quoteAsset        | string     | `USDT`    | `Quote` asset for the symbol               |
| pricePrecision    | integer    | `3`       | Price accuracy                             |
| quantityPrecision | integer    | `8`       | Quantity accuracy                          |
| limitVolumeMin    | BigDecimal | `0.0001`  | Minimum quantity limit for limit order     |
| marketBuyMin      | BigDecimal | `0.0001`  | Minimum purchase quantity for market order |
| marketSellMin     | BigDecimal | `0.0001`  | Minimum selling quantity for market orders |
| limitPriceMin     | BigDecimal | `0.001`   | Minimum price limit for limit orders       |

## Market

### Security: None

<aside class='notice'>Market section can be accessed freely without requiring any API-key or signatures.</aside>

### Depth

`GET https://openapi.xxx.com/api/v1/depth`

Market detpth data

> request example

```http
GET https://openapi.xxx.com/api/v1/depth?symbol=BTCUSDT&limit=100
```

**Request Parameter**

| Parameter                         | Type    | Description                            |
| :---------------------------------| :-------| :--------------------------------------|
| symbol<font color="red">\*</font> | string  | `Uppercase` symbol name E.g. `BTCUSDT` |
| limit<font color="red">\*</font>  | integer | default: 100; maximum: 100             |

> response example

```json
{
    "time": 1701658276000,
    "bids": [
        [
            3.90000000,     // price
            43.10000000    // volume
        ],
        [
            4.00000000,
            19.20000000
        ]
    ],
    "asks": [
        [
            4.00000200,     // price
            12.00000000     // volume
        ],
        [
            5.10000000,
            28.00000000
        ]
    ]
}
```

**Response Parameter**

| Paramter | Type  | Example                   | Description                                                                                                                                                                        |
| :--------| :-----| :-------------------------| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| time     | long  | `1595563624731`           | Timestamp                                                                                                                                                                          |
| bids     | array | `[[3.9,43.1],[4.0,19.2]]` | Order book buying information, the array length is 2, subscript one is the price, type is float; subscript two is the quantity corresponding to the current price, type is float.  |
| asks     | array | `[[4.0,12.0],[5.1,28.0]]` | Order book selling information, the array length is 2, subscript one is the price, type is float; subscript two is the quantity corresponding to the current price, type is float. |

The bid and ask information represents all the prices in the order book along with the corresponding quantities, listed from the best price to the rest in descending order.

### 24hrs Ticker

`GET https://openapi.xxx.com/api/v1/ticker`

24-hour price change data

> request example

```http
GET https://openapi.xxx.com/api/v1/ticker?symbol=BTCUSDT
```

**Request Parameter**

| Parameter                         | Type   | Description                            |
| :---------------------------------| :------| :--------------------------------------|
| symbol<font color="red">\*</font> | string | `Uppercase` symbol name E.g. `BTCUSDT` |

> response example

```json
{
    "amount": 22400.0,
    "high": 9900.51,
    "vol": 4691.0,
    "last": 9211.60,
    "low": 9100.34,
    "buy": 9210.0,
    "sell": 9213.0,
    "rose": "+0.05",
    "time": 1595563624731
}
```

**Response Parameter**

| Parameter | Type   | Example         | Description                                                                                      |
| :---------| :------| :---------------| :------------------------------------------------------------------------------------------------|
| time      | long   | `1595563624731` | Timestamp                                                                                        |
| high      | float  | `9900.51`       | Highest price                                                                                    |
| low       | float  | `9100.34`       | Lowest price                                                                                     |
| last      | float  | `9211.60`       | Latest trade price                                                                               |
| vol       | float  | `4691.0`        | Trading volume                                                                                   |
| amount    | float  | `22400.0`       | Trading amount                                                                                   |
| buy       | float  | `9210.0`        | Best bid price                                                                                   |
| sell      | float  | `9213.0`        | Best ask price                                                                                   |
| rose      | string | `+0.05`         | Range of increase and decrease, `+` is increase, `-` is decrease, `+0.05` means increase by `5%` |

### Recent Trade List

`GET https://openapi.xxx.com/api/v1/trades`

Market recent trade

> request example

```http
GET https://openapi.xxx.com/api/v1/trades?symbol=BTCUSDT&limit=100
```

**Request Parameter**

| Parameter                         | Type   | Description                            |
| :---------------------------------| :------| :--------------------------------------|
| symbol<font color="red">\*</font> | string | `Uppercase` symbol name E.g. `BTCUSDT` |
| limit<font color="red">\*</font>  | string | default: 100; maximum: 1000            |

> response example

```json
[
    {
        "side": "buy",
        "price": 131.0000000000000000,
        "qty": 0.1000000000000000,
        "time": 1704788645416
    },
    {
        "side": "sell",
        "price": 132.0000000000000000,
        "qty": 0.2000000000000000,
        "time": 1704788282332
    }
]
```

**Response Parameter**

| Parameter | Type   | Example                | Description         |
| :---------| :------| :----------------------| :-------------------|
| price     | float  | `131.0000000000000000` | Trade price         |
| time      | long   | `1704788645416`        | Timestamp           |
| qty       | float  | `0.1000000000000000`   | The quantity traded |
| side      | string | `buy/sell`             | Taker side          |

### K-Line/Candlestick Data

`GET https://openapi.xxx.com/api/v1/klines`

K-Line/Candlestick data

> request example

```http
GET https://openapi.xxx.com/api/v1/klines?symbol=BTCUSDT&interval=1min&limit=100
```

**Request Parameter**

| Parameter                           | Type    | Description                                                                                                           |
| :-----------------------------------| :-------| :---------------------------------------------------------------------------------------------------------------------|
| symbol<font color="red">\*</font>   | string  | `Uppercase` symbol name E.g. `BTCUSDT`                                                                                |
| interval<font color="red">\*</font> | string  | Interval of the candlesticks. Possible values include:  `1min`,`5min`,`15min`,`30min`,`60min`,`1day`,`1week`,`1month` |
| limit                               | integer | default: 100; maximum: 300                                                                                            |

> response example

```json
[
    {
        "high": 6228.77,
        "vol": 2456.11,
        "low": 6220.13,
        "idx": 1594640340000,
        "close": 6225.63,
        "open": 6129.41
    },
    {
        "high": 6228.77,
        "vol": 1700.21,
        "low": 6228.77,
        "idx": 1587632160000,
        "close": 6228.77,
        "open": 6228.77
    },
    {
        "high": 6228.77,
        "vol": 1908.52,
        "low": 6228.77,
        "idx": 1587632100000,
        "close": 6228.77,
        "open": 6228.77
    }
]
```

**Response Parameter**

| Parameter | Type  | Example         | Description     |
| :---------| :-----| :---------------| :---------------|
| idx       | long  | `1538728740000` | Start timestamp |
| open      | float | `6129.41`       | Open price      |
| close     | float | `6225.63`       | Close price     |
| high      | float | `6228.77`       | Highest price   |
| low       | float | `6220.13`       | Lowest price    |
| vol       | float | `2456.11`       | Trading volume  |

## Trade

### Security: TRADE

<aside class='notice'>The interface in Trade requires API-key and signature verification for access.</aside>

### Create New Order

`POST https://openapi.xxx.com/api/v1/order`

**Rate Limit: 100 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://openapi.xxx.com/api/v1/order

body
{
    "symbol": "BTCUSDT",
    "volume": 1.00,
    "side": "BUY",
    "type": "LIMIT",
    "price": 65000.00,
    "newClientOrderId": "111000000111"
}
```

**Request Parameter**

| Parameter                         | Type   | Description                                                                                                        |
| :---------------------------------| :------| :------------------------------------------------------------------------------------------------------------------|
| symbol<font color="red">\*</font> | string | `Uppercase` symbol name E.g. `BTCUSDT`                                                                             |
| volume<font color="red">\*</font> | number | Order quantity, there is a precision limit, which is configured by the administrator.                              |
| side<font color="red">\*</font>   | string | Order side, `BUY/SELL`                                                                                             |
| type<font color="red">\*</font>   | string | Order type, `LIMIT/MARKET`                                                                                         |
| price                             | number | Order price, fo `LIMIT` orders must be sent, there is a precision limit, which is configured by the administrator. |
| newClientOrderId                  | string | Client order ID                                                                                                    |

> response example

```json
{
    "symbol": "ETHUSDT",
    "side": "BUY",
    "executedQty": 0,
    "orderId": [
        "2012274607240433332"
    ],
    "price": 47651.29,
    "origQty": 0.01,
    "clientOrderId": "213443",
    "transactTime": 1704959985403,
    "type": "MARKET",
    "status": "NEW"
}
```

**Response Parameter**

| Parameter     | Type    | Example               | Description                                                                                                      |
| :-------------| :-------| :---------------------| :----------------------------------------------------------------------------------------------------------------|
| orderId       | long    | `2012274607240433332` | Order ID(system generated)                                                                                       |
| clientOrderId | string  | `213443`              | Order ID(sent by user)                                                                                           |
| symbol        | string  | `BTCUSDT`             | `Uppercase` symbol name                                                                                          |
| transactTime  | integer | `1704959985403`       | The time of order placed                                                                                         |
| price         | float   | `47651.29`            | Order price                                                                                                      |
| origQty       | float   | `0.01`                | Order volume                                                                                                     |
| executedQty   | float   | `0`                   | The number of orders already executed                                                                            |
| type          | string  | `LIMIT`               | Type of the order: `LIMIT/MARKET`                                                                                |
| side          | string  | `BUY`                 | Side of the order: `BUY/ SELL`                                                                                   |
| status        | string  | `NEW`                 | Order status: New Order (new order, no execution), Partially Filled (partial execution), Filled (fully executed) |

### Test New Order

`POST https://openapi.xxx.com/api/v1/order/test`

Create and validate a new order but do not send the order into the matching engine.

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://openapi.xxx.com/api/v1/order/test

body
{
    "symbol": "BTCUSDT",
    "volume": 1.00,
    "side": "BUY",
    "type": "LIMIT",
    "price": 65000.00,
    "newClientOrderId": "111000001111"
}
```
**Request Parameter**

| Parameter                         | Type   | Description                                                                                               |
| :---------------------------------| :------| :---------------------------------------------------------------------------------------------------------|
| symbol<font color="red">\*</font> | string | `Uppercase` symbol name E.g. `BTCUSDT`                                                                    |
| volume<font color="red">\*</font> | number | Order quantity, with precision limits, configured by the administrator                                    |
| side<font color="red">\*</font>   | string | Order side, `BUY/SELL`                                                                                    |
| type<font color="red">\*</font>   | string | Order type, `LIMIT/MARKET`                                                                                |
| price                             | number | Order price, for `LIMIT` orders must be sent, there is a precision limit, configured by the administrator |
| newClientOrderId                  | string | Client order ID                                                                                           |

> response example

```json
{}
```

### Batch Orders

`POST https://openapi.xxx.com/api/v1/batchOrders`

**Rate Limit: 50 times/2s Up to 10 orders in one batch order request**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://openapi.xxx.com/api/v1/batchOrders

body
{
    "symbol": "ETHUSDT",
    "orders": [
        {
            "price": 2100.00,
            "volume": 1.00,
            "side": "BUY",
            "batchType": "LIMIT"
        },
        {
            "price": 2200.00,
            "volume": 2.00,
            "side": "SELL",
            "batchType": "LIMIT"
        }
    ]
}
```

**Request Parameter**

| Parameter | Type   | Example        | Description             |
| :---------| :------| :--------------| :-----------------------|
| symbol    | string | `ETHUSDT`      | `Uppercase` symbol name |
| price     | float  | `2100.00`      | Order price             |
| volume    | float  | `1.00`         | Order volume            |
| side      | string | `BUY/SELL`     | Order side              |
| batchType | string | `LIMIT/MARKET` | Order type              |

> response example

```json
{
    "ids": [
        165964665990709251,
        165964665990709252,
        165964665990709253
    ]
}
```

**Response Parameter**

| Parameter | Type  | Example | Description       |
| :---------| :-----| :-------| :-----------------|
| ids       | array |         | ids of Order List |


### Query Order

`GET https://openapi.xxx.com/api/v1/order`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
GET https://openapi.xxx.com/api/v1/order?symbol=ethusdt&orderID=111000111
```

**Request Parameter**

| Parameter                          | Type   | Description                            |
| :----------------------------------| :------| :--------------------------------------|
| orderId<font color="red">\*</font> | string | Order id                               |
| symbol<font color="red">\*</font>  | string | `Lowercase` symbol name E.g. `ethusdt` |

> response example

```json
{
    "symbol": "ethusdt",
    "side": "BUY",
    "executedQty": 0,
    "orderId": 150695552109032492,
    "price": 4765.29,
    "origQty": 1.01,
    "avgPrice": 4754.24,
    "transactTime": 1672274311107,
    "type": "LIMIT",
    "status": "New Order"
}
```

**Response Parameter**

| Parameter     | Type   | Example              | Description                                                                                                      |
| :-------------| :------| :--------------------| :----------------------------------------------------------------------------------------------------------------|
| orderId       | long   | `150695552109032492` | Order ID (system generated)                                                                                      |
| clientOrderId | string | `213443`             | Order ID (sent by user)                                                                                          |
| symbol        | string | `ethusdt`            | `Lowercase` symbol name                                                                                          |
| price         | float  | `4765.29`            | Order price                                                                                                      |
| origQty       | float  | `1.01`               | Order volume                                                                                                     |
| executedQty   | float  | `0`                  | The number of orders already executed                                                                            |
| avgPrice      | float  | `4754.24`            | The average price of executed orders                                                                             |
| type          | string | `LIMIT`              | Order type:`LIMIT`/`MARKET`                                                                                      |
| transactTime  | long   | `1672274311107`      | Timestamp                                                                                                        |
| side          | string | `BUY`                | Order side: `BUY`/`SELL`                                                                                         |
| status        | string | `New Order`          | Order status: New Order (new order, no execution), Partially Filled (partial execution), Filled (fully executed) |

### Cancel Order

`POST https://openapi.xxx.com/api/v1/cancel`

**Rate Limit: 100 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://openapi.xxx.com/api/v1/cancel
body
{
    "symbol": "ethusdt",
    "orderId": "111000111"
}
```

**Request Parameter**

| Parameter                          | Type   | Description                            |
| :----------------------------------| :------| :--------------------------------------|
| orderId<font color="red">\*</font> | string | Order id                               |
| symbol<font color="red">\*</font>  | string | `Lowercase` symbol name E.g. `ethusdt` |

> response example

```json
{
    "symbol": "ethusdt",
    "orderId": [
        "1938321163093079425"
    ],
    "status": "PENDING_CANCEL"
}
```

**Response Parameter**

| Parameter | Type   | Example               | Description                     |
| :---------| :------| :---------------------| :-------------------------------|
| orderId   | long   | `1938321163093079425` | Order ID (system generated)     |
| symbol    | string | `ethusdt`             | `Lowercase` symbol name         |
| status    | string | `To be Cancelled`     | Order Status: `To be Cancelled` |

### Batch Order Cancellation

`POST https://openapi.xxx.com/api/v1/batchCancel`

**Rate Limit: 50 times/2s  Every batch cancellation request contains at most 10 orders**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://openapi.xxx.com/api/v1/batchCancel
body
{
    "symbol": "BTCUSDT",
    "oderIds": [111000111, 111000112]
}
```

**Request Parameter**

| Parameter                           | Type   | Description                                                                                                            |
| :-----------------------------------| :------| :----------------------------------------------------------------------------------------------------------------------|
| symbol<font color="red">\*</font>   | string | `Uppercase` symbol name E.g. `BTCUSDT`                                                                                 |
| orderIds<font color="red">\*</font> | array  | Collection of order IDs to be canceled. The order id must be of numeric type rather than String type. e.g. `[123,456]` |

>success response example

```json
{
    "success": [
        165964665990709251,
        165964665990709252,
        165964665990709253
    ],
    "failed": [ // Cancellation failure is usually because the order does not exist or the order status has reached the final state.
        165964665990709250
    ]
}
```

> failure response example

```json
{} //Usually it is because the order number is wrong and you need to check whether the content in orderIds is correct.
```

### Current Open Orders

`GET https://openapi.xxx.com/api/v1/openOrders`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                         | Type    | Description                            |
| :---------------------------------| :-------| :--------------------------------------|
| symbol<font color="red">\*</font> | string  | `Lowercase` symbol name E.g. `ethusdt` |
| limit<font color="red">\*</font>  | integer | Maximum: 1000                          |

> response example

```json
[
    {
        "symbol": "ETHUSDT",
        "side": "BUY",
        "executedQty": "0",
        "orderId": 1938321163093077686,
        "price": "0",
        "origQty": "0.10",
        "avgPrice": "0",
        "time": 1701240367864,
        "type": "MARKET",
        "status": "NEW_"
    },
    {
        "symbol": "ETHUSDT",
        "side": "BUY",
        "executedQty": "0",
        "orderId": 1938321163093078022,
        "price": "0",
        "origQty": "0.01",
        "avgPrice": "0",
        "time": 1701243281850,
        "type": "MARKET",
        "status": "NEW_"
    }
]
```

**Response Parameter**

| Parameter   | Type   | Example              | Description                                                                                                            |
| :-----------| :------| :--------------------| :----------------------------------------------------------------------------------------------------------------------|
| orderId     | long   | `150695552109032492` | Order ID (system generated)                                                                                            |
| symbol      | string | `ETHUSDT`            | `Uppercase` symbol name                                                                                                |
| price       | float  | `4765.29`            | Order price                                                                                                            |
| origQty     | float  | `1.01`               | Order volume                                                                                                           |
| executedQty | float  | `1.01`               | The number of orders already executed                                                                                  |
| avgPrice    | float  | `4754.24`            | The average price of executed orders                                                                                   |
| type        | string | `LIMIT`              | Order type:`LIMIT`/`MARKET`                                                                                            |
| time        | long   | `1701243281850`      | Timestamp                                                                                                              |
| side        | string | `BUY`                | Order side: `BUY`/`SELL`                                                                                               |
| status      | string | `New Order`          | Order status: `New Order` (new order, no execution), `Partially Filled` (partial execution), `Filled` (fully executed) |

### Trading Records

`GET https://openapi.xxx.com/api/v1/myTrades`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |


> request example

```http
GET https://openapi.xxx.com/api/v1/myTrades?symbol=BTCUSDT&limit=100
```

**Request Parameter**

| Parameter                         | Type   | Description                            |
| :---------------------------------| :------| :--------------------------------------|
| symbol<font color="red">\*</font> | string | `Uppercase` symbol name E.g. `BTCUSDT` |
| limit<font color="red">\*</font>  | string | default: 100; maximum: 1000            |

> response example

```json
[
    {
        "symbol": "ETHUSDT",
        "side": "BUY",
        "fee": 0.00000000428,
        "isMaker": false,
        "isBuyer": true,
        "bidId": 1954603951049381893,
        "bidUserId": 10083,
        "feeCoin": "ETH",
        "price": 2334,
        "qty": 0.00000428,
        "askId": 1856176838352995447,
        "id": 159,
        "time": 1701623660989,
        "isSelf": false,
        "askUserId": 10671
    },
    {
        "symbol": "ETHUSDT",
        "side": "BUY",
        "fee": 0.00000004284,
        "isMaker": false,
        "isBuyer": true,
        "bidId": 1938321163093068889,
        "bidUserId": 10083,
        "feeCoin": "ETH",
        "price": 2334,
        "qty": 0.00004284,
        "askId": 1856176838352995447,
        "id": 158,
        "time": 1701165091964,
        "isSelf": false,
        "askUserId": 10671
    }
]
```

**Response Parameter**

| Parameter | Type    | Example               | Description                                                     |
| :---------| :-------| :---------------------| :---------------------------------------------------------------|
| symbol    | string  | `ETHBTC`              | `Uppercase` symbol name                                         |
| id        | integer | `159`                 | Trade ID                                                        |
| bidId     | long    | `1954603951049381893` | Buy side order ID                                               |
| askId     | long    | `1856176838352995447` | Sell side order ID                                              |
| price     | integer | `2334`                | Trade price                                                     |
| qty       | float   | `0.00004284`          | Trade volume                                                    |
| time      | number  | `1701165091964`       | Trade timestamp                                                 |
| isBuyer   | boolean | `true`                | `true`= Buyer, `false`= Seller                                  |
| isMaker   | boolean | `false`               | `true`= Maker, `false`= Taker                                   |
| feeCoin   | string  | `ETH`                 | Trade fee coin                                                  |
| fee       | number  | `0.00000000428`       | Trade fee                                                       |
| bidUserId | integer | `10083`               | Buy side user id                                                |
| askUserId | integer | `10671`               | Sell side user id                                               |
| isSelf    | boolean | `false`               | Self deal status `true` = is self deal, `false` = not self deal |
| side      | string  | `BUY`                 | Taker order side: `BUY`/`SELL`                                  |

## Account

### Security: USER\_DATA

<aside class="notice">The interface in Trade requires API-key and signature verification for access.</aside>

### Account Information

`GET https://openapi.xxx.com/api/v1/account`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> response example

```json
{
    "balances": [
        {
            "asset": "ABAT",
            "free": "10.00",
            "locked": "20.00"
        },
        {
            "asset": "DOT",
            "free": "10.00",
            "locked": "10.00"
        },
        {
            "asset": "TT",
            "free": "50.00",
            "locked": "50.00"
        }
    ]
}
```

**Response Parameter**

| Parameter | Type   | Description                       |
| :---------| :------| :---------------------------------|
| balances  | array  | The collection of account balance |
| asset     | string | Symbol                            |
| free      | string | Available balance                 |
| locked    | string | Frozen balance                    |