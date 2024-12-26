# Futures Trading API

## Public

### Security: None

<aside class="notice">Public section can be accessed freely without requiring any API-key or signatures.</aside>

### Test Connectivity

`GET https://futuresopenapi.xxx.com/fapi/v1/ping`

This interface checks connectivity to the host.

### Check Server Time

`GET https://futuresopenapi.xxx.com/fapi/v1/time`

> return example

```json
{
    "timezone": "China Standard Time",
    "serverTime": 1704962055664
}
```

**Request Parameter**

| Parameter  | Type   | Example               | Description      |
| :----------| :------| :---------------------| :----------------|
| timezone   | string | `China Standard Time` | Server time zone |
| serverTime | long   | `1607702400000`       | Server timestamp |

### Futures List

`GET https://futuresopenapi.xxx.com/fapi/v1/contracts`

> request example

```http
GET https://futuresopenapi.xxx.com/fapi/v1/contracts
```

> response example

```json
[
    {
        "symbol": "E-ETC-USDT",
        "pricePrecision": 3,
        "side": 1,
        "maxMarketVolume": 200000,
        "multiplier": 1.0000000000000000,
        "minOrderVolume": 1,
        "maxMarketMoney": 500000.0000000000000000,
        "type": "E",
        "maxLimitVolume": 300000,
        "maxValidOrder": 10,
        "multiplierCoin": "ETC",
        "minOrderMoney": 1.0000000000000000,
        "maxLimitMoney": 500000.0000000000000000,
        "status": 1
    },
    {
        "symbol": "E-ATOM-USDT",
        "pricePrecision": 3,
        "side": 1,
        "maxMarketVolume": 100000,
        "multiplier": 1.0000000000000000,
        "minOrderVolume": 1,
        "maxMarketMoney": 200000.0000000000000000,
        "type": "E",
        "maxLimitVolume": 200000,
        "maxValidOrder": 10,
        "multiplierCoin": "ATOM",
        "minOrderMoney": 20.0000000000000000,
        "maxLimitMoney": 2000000.0000000000000000,
        "status": 1
    }
]
```

**Response Parameter**

| Parameter       | Type   | Example                   | Description                                                                                   |
| :---------------| :------| :-------------------------| :---------------------------------------------------------------------------------------------|
| symbol          | string | `E-BTC-USDT`              | `Uppercase` Contract name                                                                     |
| pricePrecision  | number | `3`                       | Price accuracy                                                                                |
| status          | number | `1`                       | Contract status(0:`not tradable`, 1:`tradable`)                                               |
| type            | string | `E`                       | Contract type(E: `Perpetual contract`, S: `Simulated contract`, others are `mixed contracts`) |
| side            | number | `1`                       | Contract direction(0: `reverse`, 1: `forward`)                                                |
| multiplier      | number | `1.0000000000000000`      | Contract value                                                                                |
| minOrderVolume  | number | `1`                       | Minimum order quantity                                                                        |
| minOrderMoney   | number | `1.0000000000000000`      | Minimum order amount                                                                          |
| maxMarketVolume | number | `200000`                  | Maximum order quantity for market orders                                                      |
| maxMarketMoney  | number | `500000.0000000000000000` | Maximum order amount at market price                                                          |
| maxLimitVolume  | number | `300000`                  | Maximum order quantity for limit orders                                                       |
| maxLimitMoney   | number | `500000.0000000000000000` | Maximum order amount at limit price                                                           |
| maxValidOrder   | number | `10`                      | The maximum number of valid orders                                                            |

## Market

### Security: None

<aside class="notice">Market section can be accessed freely without requiring any API-key or signatures.</aside>

### Depth

`GET https://futuresopenapi.xxx.com/fapi/v1/depth`

Market detpth data

> request example

```http
GET https://futuresopenapi.xxx.com/fapi/v1/depth?contractName=E-BTC-USDT&limit=100
```

**Request Parameters**

| Parameter    | Type    | Description                                 |
| :------------| :-------| :-------------------------------------------|
| contractName | string  | `Uppercase` contract name E.g. `E-BTC-USDT` |
| limit        | integer | default: 100; maximum: 100                  |

> response example

```json
{
    "time": 1704962463000,
    "bids": [
        [
            3.90000000,     //price
            16.10000000     //volume
        ],
        [
            4.00000000,
            29.30000000
        ]
    ],
    "asks": [
        [
            4.00000200,     //price
            12.00000000     //volume
        ],
        [
            5.10000000,
            28.00000000
        ]
    ]
}
```

**Response Parameters**

| Parameter | Type | Example                          | Description                                                                                                                                                                       |
| :---------| :----| :--------------------------------| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| time      | long | `1595563624731`                  | Current time                                                                                                                                                                      |
| bids      | list | `[[3.9,16.1],[4.0,29.3]]`        | Order book buying information, the array length is 2, subscript one is the price, type is float; subscript two is the quantity corresponding to the current price, type is float  |
| asks      | list | `[[4.00000200,12.0],[5.1,28.0]]` | Order book selling information, the array length is 2, subscript one is the price, type is float; subscript two is the quantity corresponding to the current price, type is float |

The bid and ask information represents all the prices in the order book along with the corresponding quantities, listed from the best price to the rest in descending order.

### 24hrs Ticker

`GET https://futuresopenapi.xxx.com/fapi/v1/ticker`

24-hour price change data

> request example

```http
GET https://futuresopenapi.xxx.com/fapi/v1/ticker?contractName=E-BTC-USDT
```

**Request Parameters**

| Parameter                               | Type   | Description                                 |
| :---------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT` |


> response example

```json
{
    "high": 56120.22,
    "vol": 51.21,
    "last": 55989.93,
    "low": 55982.24,
    "buy": 55988.10,
    "sell": 55990.10,
    "rose": "+0.05",
    "time": 1704966225000
}
```

**Response Parameters**

| Parameter | Type   | Example         | Description                                                                                      |
| :---------| :------| :---------------| :------------------------------------------------------------------------------------------------|
| time      | long   | `1595563624731` | Timestamp                                                                                        |
| high      | float  | `56120.22`      | Highest price                                                                                    |
| low       | float  | `55982.24`      | Lowest price                                                                                     |
| last      | float  | `55989.93`      | Last price                                                                                       |
| vol       | float  | `51.21`         | Trade volume                                                                                     |
| rose      | string | `+0.05`         | Range of increase and decrease, `+` is increase, `-` is decrease, `+0.05` means increase by `5%` |
| buy       | float  | `55988.10`      | Best bid price                                                                                   |
| sell      | float  | `55990.10`      | Best ask price                                                                                   |

## Obtain Index/Mark Price

`GET` `https://futuresopenapi.xxx.com/fapi/v1/index`

> request example

```http
GET https://futuresopenapi.xxx.com/fapi/v1/index?contractName=E-BTC-USDT
```

**Request Parameters**

| Name                                    | Type   | Description                                 |
| :---------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT` |
| limit                                   | string | Default 100; maximum 1000                   |

> response example

```json
{
    "indexPrice": 27905.9800000000000000,
    "tagPrice": 27824.4422146875000000,
    "currentFundRate": -0.0037500000000000,
    "nextFundRate": -0.0037500000000000
}
```

**Response Parameters**

| Parameter       | Type  | Example                  | Description                                                                     |
| :---------------| :-----| :------------------------| :-------------------------------------------------------------------------------|
| indexPrice      | float | `27905.9800000000000000` | Index price                                                                     |
| tagPrice        | float | `27824.4422146875000000` | Mark price                                                                      |
| currentFundRate | float | `-0.0037500000000000`    | Funding rate price for the previous period (used for settlement in this period) |
| nextFundRate    | float | `-0.0037500000000000`    | Funding rate price                                                              |

### K-Line/Candlestick Data

`GET https://futuresopenapi.xxx.com/fapi/v1/klines`

> request example

```http
GET https://futuresopenapi.xxx.com/fapi/v1/klines?contractName=E-BTC-USDT&interval=1min&limit=100&startTime=1111111100000&endTime=222222222000000
```

**Request Parameter**

| Parameter                               | Type    | Description                                                                                                           |
| :---------------------------------------| :-------| :---------------------------------------------------------------------------------------------------------------------|
| contractName<font color="red">\*</font> | string  | `Uppercase` contract name E.g. `E-BTC-USDT`                                                                           |
| interval<font color="red">\*</font>     | string  | Interval of the candlesticks. Possible values include:  `1min`,`5min`,`15min`,`30min`,`60min`,`1day`,`1week`,`1month` |
| limit                                   | integer | default: 100; maximum: 300                                                                                            |
| startTime                               | long    | Timestamp                                                                                                             |
| endTime                                 | long    | Timestamp                                                                                                             |

> response example

```json
[
    {
        "high": 6228.77,
        "vol": 111,
        "low": 6190.48,
        "idx": 15946403400000,
        "close": 6210.51,
        "open": 6195.80
    },
    {
        "high": 6228.77,
        "vol": 222,
        "low": 6228.77,
        "idx": 15876321600000,
        "close": 6228.77,
        "open": 6228.77
    },
    {
        "high": 6228.77,
        "vol": 333,
        "low": 6228.77,
        "idx": 15876321000000,
        "close": 6228.77,
        "open": 6228.77
    }
]
```

**Response Parameter**

| Parameter | Type  | Example          | Description     |
| :---------| :-----| :----------------| :---------------|
| idx       | long  | `15946403400000` | Start timestamp |
| open      | float | `6195.80`        | Open price      |
| close     | float | `6210.51`        | Close price     |
| high      | float | `6228.77`        | Highest price   |
| low       | float | `6190.48`        | Lowest price    |
| vol       | float | `111`            | Trade volume    |

## Trade

### Security: TRADE

<aside class="notice">The interface in Trade requires API-key and signature verification for access.</aside>

### Create New Order

`POST https://futuresopenapi.xxx.com/fapi/v1/order`

Create a new single order.

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://futuresopenapi.xxx.com/fapi/v1/order

body
{
    "contractName": "E-BTC-USDT",
    "price": 65000.00,
    "volume": 1.00,
    "type": "LIMIT",
    "side": "BUY",
    "open": "OPEN",
    "positionType": 1,
    "clientOrderId": "111000111",
    "timeInForce": "IOC"
}
```

**Request Parameter**

| Parameter                               | Type   | Description                                                                                                                                                    |
| :---------------------------------------| :------| :--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT`                                                                                                                    |
| price                                   | number | The number of orders placed has a precision limit. The precision is set by the administrator. When opening a position at market price, the unit here is value. |
| volume<font color="red">\*</font>       | number | Order price. This field is required for limit orders. There is a precision limit. The precision is set by the administrator.                                   |
| type<font color="red">\*</font>         | string | Order type : `LIMIT/MARKET`                                                                                                                                    |
| side<font color="red">\*</font>         | string | Order side: `BUY/SELL`                                                                                                                                         |
| open<font color="red">\*</font>         | string | Position side: `OPEN/CLOSE`                                                                                                                                    |
| positionType<font color="red">\*</font> | number | Position type: `1:Cross Margin/2:Isolated Margin`                                                                                                              |
| timeInForce                             | string | `IOC, FOK, POST_ONLY`                                                                                                                                          |
| clientOrderId                           | string | Client order ID, a string less than 32 characters in length                                                                                                    |

> response example

```json
{
    "orderId": 256609229205684228
}
```

**Response Parameter**

| Parameter | Type   | Example              | Description |
| :---------| :------| :--------------------| :-----------|
| orderId   | string | `256609229205684228` | Order ID    |

### Create Conditional Order

`POST https://futuresopenapi.xxx.com/fapi/v1/conditionOrder`

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

> request example

```http
POST https://futuresopenapi.xxx.com/fapi/v1/conditionOrder

body
{
    "contractName": "E-BTC-USDT"
}
```

**Request Parameter**

| Parameter     | Type   | Description                                                                                                                                                 |
| :-------------| :------| :-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| triggerType   | string | Type of conditions, 1: `stop loss`, 2: `take profit`, 3: `chase the rise`, 4: `kill the fall`                                                               |
| triggerPrice  | string | Trigger price                                                                                                                                               |
| clientOrderId | string | Client order identification, a string less than 32 characters in length                                                                                     |
| positionType  | number | Position type: 1. `Cross Margin`, 2. `Isolated Margin`                                                                                                      |
| open          | string | Position side: `OPEN/CLOSE`                                                                                                                                 |
| side          | string | Orde side: `BUY/SELL`                                                                                                                                       |
| type          | string | Order type: `LIMIT/MARKET`                                                                                                                                  |
| contractName  | string | `Uppercase` contract name E.g. `E-BTC-USDT`                                                                                                                 |
| price         | number | The order price has precision limits, and the precision is set by the administrator.                                                                        |
| volume        | number | The order quantity. When opening a position at market price, the unit here is value. There is a precision limit. The precision is set by the administrator. |

> response example

```json
{
    "code": "0",
    "msg": "Success",
    "data": {
        "triggerIds": [
            "1322738336974712847"
        ],
        "ids": [
            
        ],
        "cancelIds": [
            
        ]
    },
    "succ": true
}
```

If this interface (/fapi/v1/conditionOrder) return does not meet expectations, please contact the technical team and we will provide you with relevant assistance.

### Cancel Order

`POST https://futuresopenapi.xxx.com/fapi/v1/cancel`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                               | Type   | Description                                 |
| :---------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT` |
| orderId<font color="red">\*</font>      | string | Order ID                                    |

> response example

```json
{
    "orderId": "256609229205684228"
}
```

### Cancel Trigger Order

`POST https://futuresopenapi.xxx.com/fapi/v1/cancel_trigger_order`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                               | Type   | Description                                 |
| :---------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT` |
| orderId<font color="red">\*</font>      | string | Order ID                                    |
| clientOrderId                           | string | Client unique identifier, default is 0      |

> response example

```json
{
    "orderId": "256609229205684228"
}
```

### Order Details

`GET https://futuresopenapi.xxx.com/fapi/v1/order`

**Request Parameter**

| Parameter                                | Type   | Description                                 |
| :----------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font>  | string | `Uppercase` contract name E.g. `E-BTC-USDT` |
| orderId<font color="red">\*</font>       | string | Order ID                                    |
| clientOrderId<font color="red">\*</font> | string | Client unique identifier, default is 0      |

> response example

```json
{
    "side": "BUY",
    "executedQty": 0,
    "orderId": 2006628907041292645,
    "price": 67000.0000000000000000,
    "origQty": 2.0000000000000000,
    "avgPrice": 0,
    "transactTime": 1704967622000,
    "action": "OPEN",
    "contractName": "E-BTC-USDT",
    "type": "LIMIT",
    "timeInForce": "1",
    "status": "NEW",
    "fills": [

    ]
}
```

**Response Parameter**

| Parameter    | Type    | Example                  | Description                                                                                                                                                                                 |
| :------------| :-------| :------------------------| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| orderId      | long    | `2006628907041292645`    | Order ID (system generated)                                                                                                                                                                 |
| contractName | string  | `E-BTC-USDT`             | `Uppercase` contract name E.g. `E-BTC-USDT`                                                                                                                                                 |
| price        | float   | `67000.0000000000000000` | Order price                                                                                                                                                                                 |
| origQty      | float   | `2.0000000000000000`     | Order volume                                                                                                                                                                                |
| executedQty  | float   | `0`                      | Filled volume                                                                                                                                                                               |
| avgPrice     | float   | `0`                      | Average filled price                                                                                                                                                                        |
| status       | string  | `NEW`                    | Order status. Possible values are`NEW` (new order, no execution), `PARTIALLY_FILLED` (partial execution), `FILLED` (fully executed), `CANCELED` (canceled), and `REJECTED` (order rejected) |
| side         | string  | `BUY`                    | Order side: `BUY` (buy long) and `SELL` (sell short)                                                                                                                                        |
| action       | string  | `OPEN`                   | `OPEN/CLOSE`                                                                                                                                                                                |
| transactTime | long    | `1704967622000`          | Order creation time                                                                                                                                                                         |
| type         | string  | `LIMIT`                  | Order type: `LIMIT/MARKET`                                                                                                                                                                  |
| timeInForce  | integer | `1`                      | Effective method of conditional orders: 1: `limit`, 2: `market`, 3: `IOC`, 4: `FOK`, 5: `POST\_ONLY`                                                                                        |
| fills        | array   |                          | Transaction record                                                                                                                                                                          |

### Current Order Information

`GET https://futuresopenapi.xxx.com/fapi/v1/openOrders`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                               | Type   | Description                                 |
| :---------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT` |

> response example

```json
[
    {
        "side": "BUY",
        "executedQty": 0.5,
        "orderId": 259396989397942275,
        "price": 72000.0000000000000000,
        "origQty": 1.0000000000000000,
        "avgPrice": 71990.0,
        "transactTime": 1607702400000,
        "action": "OPEN",
        "contractName": "E-BTC-USDT",
        "type": "LIMIT",
        "status": "NEW"
    }
]
```

Response Parameter

| Parameter       | Type   | Example                     | Description                                                                                                                                                   |
| :------------| :------| :------------------------| :------------------------------------------------------------------------------------------------------------------------------------------------------|
| orderId      | long   | `259396989397942275`     | Order ID(system generated)                                                                                                                                     |
| contractName | string | `E-BTC-USDT`             | `Uppercase` contract name E.g. `E-BTC-USDT`                                                                                                                                         |
| price        | float  | `72000.0000000000000000` | Order price                                                                                                                                               |
| origQty      | float  | `1.0000000000000000`     | Order volume                                                                                                                                               |
| executedQty  | float  | `0.5`                    | The number of orders already executed                                                                                                                                       |
| avgPrice     | float  | `71990.0`                | The average price of executed orders                                                                                                                                 |
| type         | string | `LIMIT`                  | Order type: `LIMIT` and `MARKET`                                                                                           |
| side         | string | `BUY`                    | Order side: `BUY` and `SELL`                                                                                  |
| status       | string | `NEW`                    | Order status. Possible values are`NEW` (new order, no execution), `PARTIALLY_FILLED` (partial execution), `FILLED` (fully executed), `CANCELED` (canceled), and `REJECTED` (order rejected) |
| action       | string | `OPEN`                   | `OPEN/CLOSE`                                                                                                                                           |
| transactTime | long   | `1607702400000`          | Order creation time                                                                                                                                         |

### Historical Order Records

`POST https://futuresopenapi.xxx.com/fapi/v1/orderHistorical`

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                               | Type   | Description                               |
| :---------------------------------------| :------| :-----------------------------------------|
| contractName<font color="red">\*</font> | string | Uppercase contract name E.g. `E-BTC-USDT` |
| limit                                   | string | default: 100; maximum: 100                |
| fromId                                  | long   | Start searching from this record          |

> response example

```json
[
    {
        "side": "BUY",
        "clientId": "0",
        "ctimeMs": 1632903411000,
        "positionType": 2,
        "orderId": 777293886968070157,
        "avgPrice": 41000,
        "openOrClose": "OPEN",
        "leverageLevel": 26,
        "type": 4,
        "closeTakerFeeRate": 0.00065,
        "volume": 2,
        "openMakerFeeRate": 0.00025,
        "dealVolume": 1,
        "price": 41000,
        "closeMakerFeeRate": 0.00025,
        "contractId": 1,
        "ctime": "2021-09-29T16:16:51",
        "contractName": "E-BTC-USDT",
        "openTakerFeeRate": 0.00065,
        "dealMoney": 4.1,
        "status": 4
    }
]
```

If this interface (/fapi/v1/orderHistorical) return does not meet expectations, please contact the technical team and we will provide you with relevant assistance.

### PnL Records

`POST https://futuresopenapi.xxx.com/fapi/v1/profitHistorical`

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                               | Type   | Description                               |
| :---------------------------------------| :------| :-----------------------------------------|
| contractName<font color="red">\*</font> | string | Uppercase contract name E.g. `E-BTC-USDT` |
| limit                                   | string | default: 100; maximum: 100                |
| fromId                                  | long   | Start searching from this record          |

> response example

```json
[
    {
        "side": "SELL",
        "positionType": 2,
        "tradeFee": -5.23575,
        "realizedAmount": 0,
        "leverageLevel": 26,
        "openPrice": 44500,
        "settleProfit": 0,
        "mtime": 1632882739000,
        "shareAmount": 0,
        "openEndPrice": 44500,
        "closeProfit": -45,
        "volume": 900,
        "contractId": 1,
        "historyRealizedAmount": -50.23575,
        "ctime": 1632882691000,
        "id": 8764,
        "capitalFee": 0
    }
]
```

If this interface (/fapi/v1/orderHistorical) return does not meet expectations, please contact the technical team and we will provide you with relevant assistance.

### Trading Record

`GET https://futuresopenapi.xxx.com/fapi/v1/myTrades`

**Rate Limit: 20 times/2s**

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                               | Type   | Description                                 |
| :---------------------------------------| :------| :-------------------------------------------|
| contractName<font color="red">\*</font> | string | `Uppercase` contract name E.g. `E-BTC-USDT` |
| limit                                   | string | default: 100; maximum: 100                  |
| fromId                                  | long   | Start retrieval from this tradeId           |

> response example

```json
[
    {
        "amount": 0.30000000000000000000000000000000,
        "side": "BUY",
        "fee": 0.001,
        "isMaker": true,
        "isBuyer": true,
        "bidId": 1874564572563538130,
        "bidUserId": 10034,
        "price": 10.0000000000000000,
        "qty": 3,
        "askId": 1954072405852309104,
        "contractName": "E-ETH-USDT",
        "time": 1701419186000,
        "tradeId": 1528,
        "askUserId": 10378
    },
    {
        "amount": 1.00000000000000000000000000000000,
        "side": "BUY",
        "fee": 0.00025,
        "isMaker": true,
        "isBuyer": true,
        "bidId": 1874564572563538059,
        "bidUserId": 10034,
        "price": 10.0000000000000000,
        "qty": 10,
        "askId": 1954072405852309104,
        "contractName": "E-ETH-USDT",
        "time": 1701419186000,
        "tradeId": 1527,
        "askUserId": 10378
    }
]
```

**Response Parameter**

| Parameter    | Type    | Example               | Description                                     |
| :------------| :-------| :---------------------| :-----------------------------------------------|
| tradeId      | number  | `1528`                | Trade ID                                        |
| bidId        | long    | `1874564572563538130` | Buy side order ID                               |
| askId        | long    | `1954072405852309104` | Sell side order ID                              |
| bidUserId    | integer | `10034`               | Buy side user ID                                |
| askUserId    | integer | `10378`               | Sell side user ID                               |
| price        | float   | `10.0`                | Filled price                                    |
| qty          | float   | `3`                   | Filled volume                                   |
| amount       | float   | `30.0`                | Filled amount                                   |
| time         | number  | `1499865549590`       | Trade timestamp                                 |
| fee          | number  | `0.001`               | Trade fee                                       |
| side         | string  | `BUY`                 | Order side:  `BUY/SELL`                         |
| contractName | string  | `E-BTC-USDT`          | `Uppercase` contract name E.g. `E-BTC-USDT`     |
| isMaker      | boolean | `true`                | Maker or Taker(true: `Maker`, false: `Taker`)   |
| isBuyer      | boolean | `true`                | Buyer or Seller(true: `buyer`, false: `seller`) |

### Change Position Mode

`POST https://futuresopenapi.xxx.com/fapi/v1/edit_user_position_model`

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                                | Type    | Description                                                   |
| :----------------------------------------| :-------| :-------------------------------------------------------------|
| contractName<font color="red">\*</font>  | string  | `Uppercase` contract name E.g. `E-BTC-USDT`                   |
| positionModel<font color="red">\*</font> | integer | The model of position(1: `Net position`, 2: `Hedge position`) |

> response example

```json
{
    "code": "0",
    "msg": "success",
    "data": null
}
```

### Change Margin Mode

`POST https://futuresopenapi.xxx.com/fapi/v1/edit_user_margin_model`

**Request Headers**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |

**Request Parameter**

| Parameter                                | Type    | Description                                                   |
| :----------------------------------------| :-------| :-------------------------------------------------------------|
| contractName<font color="red">\*</font>  | string  | `Uppercase` contract name E.g. `E-BTC-USDT`                   |
| positionModel<font color="red">\*</font> | integer | The model of position(1: `Net position`, 2: `Hedge position`) |

> response example

```json
{ 
    "code": "0", 
    "msg": "success", 
    "data": null 
}
```

### Change Leverage

`POST` `https://futuresopenapi.xxx.com/fapi/v1/edit_lever`

**Request Header**

| Parameter                              | Type    | Description    |
| :--------------------------------------| :-------| :--------------|
| X-CH-SIGN<font color="red">\*</font>   | string  | Signature      |
| X-CH-APIKEY<font color="red">\*</font> | string  | User's API-key |
| X-CH-TS<font color="red">\*</font>     | integer | Timestamp      |