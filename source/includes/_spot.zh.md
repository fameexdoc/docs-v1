# 现货交易

## 公共

### 安全类型: None

<aside class='notice'>公共类型接口不需要API-key或者签名就能自由访问</aside>

### 测试连接

`GET https://openapi.xxx.com/api/v1/ping`

测试REST API的连通性

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/ping
```

### 服务器时间

`GET https://openapi.xxx.com/api/v1/time`

获取服务器时间

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/time
```

> 返回示例

```json
{
    "timezone": "GMT+08:00",
    "serverTime": 1595563624731
}
```

**返回参数**

| 参数名     | 类型   | 示例                  | 描述         |
| :----------| :------| :---------------------| :------------|
| timezone   | string | `China Standard Time` | 服务器时区   |
| serverTime | long   | `1705039779880`       | 服务器时间戳 |

### 币对列表

`GET https://openapi.xxx.com/api/v1/symbols`

获取市场支持的币对集合

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/symbols
```

> 返回示例

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

**返回参数**

| 参数名            | 类型       | 示例      | 描述               |
| :-----------------| :----------| :---------| :------------------|
| symbol            | string     | `btcusdt` | `小写`币对名称     |
| baseAsset         | string     | `BTC`     | `基准货币`         |
| quoteAsset        | string     | `USDT`    | `计价货币`         |
| pricePrecision    | integer    | `3`       | 价格精度           |
| quantityPrecision | integer    | `8`       | 数量精度           |
| limitVolumeMin    | BigDecimal | `0.0001`  | 限价单最小数量限制 |
| marketBuyMin      | BigDecimal | `0.0001`  | 市价单最小购买数量 |
| marketSellMin     | BigDecimal | `0.0001`  | 市价单最小卖出数量 |
| limitPriceMin     | BigDecimal | `0.001`   | 限价单最小价格限制 |

## 行情

### 安全类型: None

<aside class='notice'>行情下方的接口不需要API-Key或者签名就能自由访问</aside>

### 订单薄

`GET https://openapi.xxx.com/api/v1/depth`

获取市场订单薄深度信息

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/depth?symbol=BTCUSDT&limit=100
```

**请求参数**

| 参数名                            | 类型    | 描述                            |
| :---------------------------------| :-------| :-------------------------------|
| symbol<font color="red">\*</font> | string  | `大写`币对名称，例如：`BTCUSDT` |
| limit<font color="red">\*</font>  | integer | 默认：100；最大：100            |

> 返回示例

```json
{
    "time": 1701658276000,
    "bids": [
        [
            3.90000000,     // 价格
            43.10000000    // 数量
        ],
        [
            4.00000000,
            19.20000000
        ]
    ],
    "asks": [
        [
            4.00000200,     // 价格
            12.00000000     // 数量
        ],
        [
            5.10000000,
            28.00000000
        ]
    ]
}
```

**返回参数**

| 参数名 | 类型  | 示例                      | 描述                                                                                          |
| :------| :-----| :-------------------------| :---------------------------------------------------------------------------------------------|
| time   | long  | `1595563624731`           | 当前时间戳                                                                                    |
| bids   | array | `[[3.9,43.1],[4.0,19.2]]` | 订单薄买盘信息，数组长度为2，角标1为价格，类型为float；角标2为当前价格对应的数量，类型为float |
| asks   | array | `[[4.0,12.0],[5.1,28.0]]` | 订单薄卖盘信息，数组长度为2，角标1为价格，类型为float；角标2为当前价格对应的数量，类型为float |


bids和asks所对应的信息代表了订单薄的所有价格以及价格对应的数量的信息, 由最优价格从上到下排列

### 行情Ticker

`GET https://openapi.xxx.com/api/v1/ticker`

获取24小时价格变化数据

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/ticker?symbol=BTCUSDT
```

**请求参数**

| 参数名                            | 类型   | 描述                          |
| :---------------------------------| :------| :-----------------------------|
| symbol<font color="red">\*</font> | string | 大写币对名称，例如：`BTCUSDT` |

> 返回示例

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

**返回参数**

| 参数名 | 类型   | 示例            | 描述                                      |
| :------| :------| :---------------| :-----------------------------------------|
| time   | long   | `1595563624731` | 当前时间戳                                |
| high   | float  | `9900.51`       | 最高价                                    |
| low    | float  | `9100.34`       | 最低价                                    |
| last   | float  | `9211.60`       | 最新成交价                                |
| vol    | float  | `4691.0`        | 交易量                                    |
| amount | float  | `22400.0`       | 交易额                                    |
| buy    | float  | `9210.0`        | 买一价格                                  |
| sell   | float  | `9213.0`        | 卖一价格                                  |
| rose   | string | `+0.05`         | 涨跌幅，`+`为涨，`-`为跌，`+0.05`为涨`5%` |

### 最近成交

`GET https://openapi.xxx.com/api/v1/trades`

获取最近成交数据

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/trades?symbol=BTCUSDT&limit=100
```

**请求参数**

| 参数名                            | 类型   | 描述                            |
| :---------------------------------| :------| :-------------------------------|
| symbol<font color="red">\*</font> | string | `大写`币对名称，例如：`BTCUSDT` |
| limit<font color="red">\*</font>  | string | 默认：100; 最大：1000               |

> 返回示例

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

**返回参数**

| 参数名 | 类型   | 示例                   | 描述         |
| :------| :------| :----------------------| :------------|
| price  | float  | `131.0000000000000000` | 交易价格     |
| time   | long   | `1704788645416`        | 当前时间戳   |
| qty    | float  | `0.1000000000000000`   | 数量（张数） |
| side   | string | `buy/sell`             | 主动单方向   |

### K线/蜡烛图数据

`GET https://openapi.xxx.com/api/v1/klines`

获取K线数据

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/klines?symbol=BTCUSDT&interval=1min&limit=100
```

**请求参数**

| 参数名                              | 类型    | 描述                                                                                                                                 |
| :-----------------------------------| :-------| :------------------------------------------------------------------------------------------------------------------------------------|
| symbol<font color="red">\*</font>   | string  | `大写`币对名称，例如：`BTCUSDT`                                                                                                      |
| interval<font color="red">\*</font> | string  | K线图区间, 可识别发送的值为： `1min`,`5min`,`15min`,`30min`,`60min`,`1day`,`1week`,`1month`（min=分钟，day=天，week=星期，month=月） |
| limit                               | integer | 默认：100；最大：300                                                                                                                     |

> 返回示例

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

**返回参数**

| 参数名 | 类型  | 示例            | 描述       |
| :------| :-----| :---------------| :----------|
| idx    | long  | `1538728740000` | 开始时间戳 |
| open   | float | `6129.41`       | 开盘价     |
| close  | float | `6225.63`       | 收盘价     |
| high   | float | `6228.77`       | 最高价     |
| low    | float | `6220.13`       | 最低价     |
| vol    | float | `2456.11`       | 成交量     |

## 交易

### 安全类型: TRADE

<aside class='notice'>交易下方的接口都需要签名和API-key验证</aside>

### 创建新订单

`POST https://openapi.xxx.com/api/v1/order`

**限速规则: 100次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

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

**请求参数**

| 参数名                            | 类型   | 描述                                                            |
| :---------------------------------| :------| :---------------------------------------------------------------|
| symbol<font color="red">\*</font> | string | `大写`币对名称，例如：`BTCUSDT`                                 |
| volume<font color="red">\*</font> | number | 订单数量，有精度限制，精度由管理员配置                          |
| side<font color="red">\*</font>   | string | 订单方向, `BUY/SELL`                                            |
| type<font color="red">\*</font>   | string | 订单类型, `LIMIT/MARKET`                                        |
| price                             | number | 订单价格, 对于`LIMIT`订单必须发送，有精度限制，精度由管理员配置 |
| newClientOrderId                  | string | 客户端订单标识                                                  |

> 返回示例

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

**返回参数**

| 参数名           | 类型      | 示例                    | 描述                                                                         |
| :-------------| :-------| :---------------------| :--------------------------------------------------------------------------|
| orderId       | long    | `2012274607240433332` | 订单ID（系统生成）                                                                 |
| clientOrderId | string  | `213443`              | 订单ID（自己发送的）                                                                |
| symbol        | string  | `BTCUSDT`             | `大写`币对名称                                                                   |
| transactTime  | integer | `1704959985403`       | 订单创建时间戳                                                                    |
| price         | float   | `47651.29`            | 订单价格                                                                       |
| origQty       | float   | `0.01`                | 订单数量                                                                       |
| executedQty   | float   | `0`                   | 已经成交订单数量                                                                   |
| type          | string  | `LIMIT`               | 订单类型`LIMIT`(限价)`MARKET`（市价）                                                |
| side          | string  | `BUY`                 | 订单方向。可能出现的值只能为：`BUY`（买入做多） 和 `SELL`（卖出做空）                                  |
| status        | string  | `NEW`                 | 订单状态。可能出现的值为：`New Order`(新订单，无成交)、`Partially Filled`（部分成交）、`Filled`（全部成交）、 |

### 创建测试订单

`POST https://openapi.xxx.com/api/v1/order/test`

创建和验证新订单, 但不会送入撮合引擎

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

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
**请求参数**

| 参数名                            | 类型   | 描述                                                        |
| :---------------------------------| :------| :-----------------------------------------------------------|
| symbol<font color="red">\*</font> | string | `大写`币对名称，如`BTCUSDT`                                 |
| volume<font color="red">\*</font> | number | 订单数量，有精度限制，由管理员配置                          |
| side<font color="red">\*</font>   | string | 订单方向, `BUY/SELL`                                        |
| type<font color="red">\*</font>   | string | 订单类型, `LIMIT/MARKET`                                    |
| price                             | number | 订单价格, 对于`LIMIT`订单必须发送，有精度限制，由管理员配置 |
| newClientOrderId                  | string | 客户端订单标识                                              |

> 返回示例

```json
{}
```

### 批量下单

`POST https://openapi.xxx.com/api/v1/batchOrders`

**限速规则: 50次/2s 一个批量最多10个订单**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

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

**请求参数**

| 参数名    | 类型   | 示例           | 描述     |
| :---------| :------| :--------------| :--------|
| symbol    | string | `ETHUSDT`      | 币对名称 |
| price     | float  | `2100.00`      | 价格     |
| volume    | float  | `1.00`         | 数量     |
| side      | string | `BUY/SELL`     | 方向     |
| batchType | string | `LIMIT/MARKET` | 类型     |

> 返回示例

```json
{
    "ids": [
        165964665990709251,
        165964665990709252,
        165964665990709253
    ]
}
```

**返回参数**

| 参数名 | 类型  | 示例 | 描述       |
| :------| :-----| :----| :----------|
| ids    | array |      | 订单id数组 |


### 订单查询

`GET https://openapi.xxx.com/api/v1/order`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/order?symbol=ethusdt&orderID=111000111
```

**请求参数**

| 参数名                             | 类型   | 描述                            |
| :----------------------------------| :------| :-------------------------------|
| orderId<font color="red">\*</font> | string | 订单id                          |
| symbol<font color="red">\*</font>  | string | `小写`币对名称，例如：`ethusdt` |

> 返回示例

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

**返回参数**

| 参数名        | 类型   | 示例                 | 描述                                                                                                          |
| :-------------| :------| :--------------------| :-------------------------------------------------------------------------------------------------------------|
| orderId       | long   | `150695552109032492` | 订单ID（系统生成）                                                                                            |
| clientOrderId | string | `213443`             | 订单ID（用户发送的）                                                                                          |
| symbol        | string | `ethusdt`            | `小写`币对名称                                                                                                |
| price         | float  | `4765.29`            | 订单价格                                                                                                      |
| origQty       | float  | `1.01`               | 订单数量                                                                                                      |
| executedQty   | float  | `0`                  | 已经成交订单数量                                                                                              |
| avgPrice      | float  | `4754.24`            | 订单已经成交的平均价格                                                                                        |
| type          | string | `LIMIT`              | 订单类型。可能出现的值只能为:`LIMIT`(限价)和`MARKET`（市价）                                                  |
| transactTime  | long   | `1672274311107`      | 时间戳                                                                                                        |
| side          | string | `BUY`                | 订单方向。可能出现的值只能为：`BUY`（买入做多） 和 `SELL`（卖出做空）                                         |
| status        | string | `New Order`          | 订单状态。可能出现的值为：`New Order`(新订单，无成交)、`Partially Filled`（部分成交）、`Filled`（全部成交）、 |

### 撤销订单

`POST https://openapi.xxx.com/api/v1/cancel`

**限速规则: 100次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

```http
POST https://openapi.xxx.com/api/v1/cancel
body
{
    "symbol": "ethusdt",
    "orderId": "111000111"
}
```

**请求参数**

| 参数名                             | 类型   | 描述                          |
| :----------------------------------| :------| :-----------------------------|
| orderId<font color="red">\*</font> | string | 订单id                        |
| symbol<font color="red">\*</font>  | string | `小写`币对名称，例如：ethusdt |

> 返回示例

```json
{
    "symbol": "ethusdt",
    "orderId": [
        "1938321163093079425"
    ],
    "status": "PENDING_CANCEL"
}
```

**返回参数**

| 参数名  | 类型   | 示例                  | 描述                       |
| :-------| :------| :---------------------| :--------------------------|
| orderId | long   | `1938321163093079425` | 订单ID（系统生成）         |
| symbol  | string | `ethusdt`             | 币对名称                   |
| status  | string | `PENDING_CANCEL`      | 订单状态：`PENDING_CANCEL` |

### 批量撤销订单

`POST https://openapi.xxx.com/api/v1/batchCancel`

**限速规则: 50次/2s 一次批量最多10个订单**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

```http
POST https://openapi.xxx.com/api/v1/batchCancel
body
{
    "symbol": "BTCUSDT",
    "oderIds": [111000111, 111000112]
}
```

**请求参数**

| 参数名                              | 类型   | 描述                                              |
| :-----------------------------------| :------| :-------------------------------------------------|
| symbol<font color="red">\*</font>   | string | `大写`币对名称，例如：`BTCUSDT`                   |
| orderIds<font color="red">\*</font> | array  | 要取消的订单id集合, id值以数字格式输入`[123,456]` |

>成功返回数据

```json
{
    "success": [
        165964665990709251,
        165964665990709252,
        165964665990709253
    ],
    "failed": [ // 取消失败一般是因为订单不存在或订单状态已经到终态
        165964665990709250
    ]
}
```

> 失败返回数据

```json
{} //通常是因为订单号错误，需要检查orderIds里的内容是否正确
```

### 当前订单

`GET https://openapi.xxx.com/api/v1/openOrders`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

**请求参数**

| 参数名                            | 类型    | 描述                            |
| :---------------------------------| :-------| :-------------------------------|
| symbol<font color="red">\*</font> | string  | `小写`币对名称，例如：`ethusdt` |
| limit<font color="red">\*</font>  | integer | 最大1000                        |

> 返回示例

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

**返回参数**

| 参数名      | 类型   | 示例                 | 描述                                                                                                          |
| :-----------| :------| :--------------------| :-------------------------------------------------------------------------------------------------------------|
| orderId     | long   | `150695552109032492` | 订单ID（系统生成）                                                                                            |
| symbol      | string | `ETHUSDT`            | 币对名称                                                                                                      |
| price       | float  | `4765.29`            | 订单价格                                                                                                      |
| origQty     | float  | `1.01`               | 订单数量                                                                                                      |
| executedQty | float  | `1.01`               | 已经成交订单数量                                                                                              |
| avgPrice    | float  | `4754.24`            | 订单已经成交的平均价格                                                                                        |
| type        | string | `LIMIT`              | 订单类型。可能出现的值只能为:`LIMIT`(限价)和`MARKET`（市价）                                                  |
| time        | long   | `1701243281850`      | 时间戳                                                                                                        |
| side        | string | `BUY`                | 订单方向。可能出现的值只能为：`BUY`（买入做多）和 `SELL`（卖出做空）                                          |
| status      | string | `New Order`          | 订单状态。可能出现的值为：`New Order`(新订单，无成交)、`Partially Filled`（部分成交）、`Filled`（全部成交）、 |

### 交易记录

`GET https://openapi.xxx.com/api/v1/myTrades`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 请求示例

```http
GET https://openapi.xxx.com/api/v1/myTrades?symbol=BTCUSDT&limit=100
```

**请求参数**

| 参数名                            | 类型   | 描述                            |
| :---------------------------------| :------| :-------------------------------|
| symbol<font color="red">\*</font> | string | `大写`币对名称，例如：`BTCUSDT` |
| limit<font color="red">\*</font>  | string | 默认：100；最大：1000               |

> 返回示例

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

**返回参数**

| 参数名    | 类型    | 示例                  | 描述                                              |
| :---------| :-------| :---------------------| :-------------------------------------------------|
| symbol    | string  | `ETHBTC`              | `大写`币种名称                                    |
| id        | integer | `159`                 | 交易ID                                            |
| bidId     | long    | `1954603951049381893` | 买方订单ID                                        |
| askId     | long    | `1856176838352995447` | 卖方订单ID                                        |
| price     | integer | `2334`                | 交易价格                                          |
| qty       | float   | `0.00004284`          | 交易数量                                          |
| time      | number  | `1701165091964`       | 交易时间戳                                        |
| isBuyer   | boolean | `true`                | `true`=Buyer，`false`=Seller                      |
| isMaker   | boolean | `false`               | `true`=Maker，`false`=Taker                       |
| feeCoin   | string  | `ETH`                 | 交易手续费币种                                    |
| fee       | number  | `0.00000000428`       | 交易手续费                                        |
| bidUserId | integer | `10083`               | 买方用户uid                                       |
| askUserId | integer | `10671`               | 卖方用户uid                                       |
| isSelf    | boolean | `false`               | 是否为自成交，`true`=是自成交，`false`=不是自成交 |
| side      | string  | `BUY`                 | 主动单方向`BUY`/`SELL`                            |

## 账户

### 安全类型: USER\_DATA

<aside class="notice">账户下方的接口都需要签名和API-key验证</aside>

### 账户信息

`GET https://openapi.xxx.com/api/v1/account`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

> 返回示例

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

**返回参数**

| 参数名   | 类型   | 描述         |
| :--------| :------| :------------|
| balances | array  | 账户余额集合 |
| asset    | string | 交易对       |
| free     | string | 可用余额     |
| locked   | string | 冻结余额     |