# 合约交易

## 公共

### 安全类型: None

<aside class="notice">公共下方的接口不需要API-key或者签名就能自由访问。</aside>

### 测试连接

`GET https://futuresopenapi.xxx.com/fapi/v1/ping`

测试REST API的连通性

### 获取服务器时间

`GET https://futuresopenapi.xxx.com/fapi/v1/time`

> 返回示例

```json
{
    "timezone": "China Standard Time",
    "serverTime": 1704962055664
}
```

**返回参数**

| 参数名     | 类型   | 示例                  | 描述         |
| :----------| :------| :---------------------| :------------|
| timezone   | string | `China Standard Time` | 服务器时区   |
| serverTime | long   | `1607702400000`       | 服务器时间戳 |

### 合约列表

`GET https://futuresopenapi.xxx.com/fapi/v1/contracts`

> 请求示例

```http
GET https://futuresopenapi.xxx.com/fapi/v1/contracts
```

> 返回示例

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

**返回参数**

| 参数名          | 类型   | 示例                      | 描述                                                      |
| :---------------|-:------| :-------------------------| :---------------------------------------------------------|
| symbol          | string | `E-BTC-USDT`              | `大写`合约名称                                            |
| pricePrecision  | number | `3`                       | 价格精度                                                  |
| status          | number | `1`                       | 合约状态（0：`不可交易`，1：`可交易`）                      |
| type            | string | `E`                       | 合约类型（E：`永续合约`，S：`模拟合约`，其他为`混合合约`） |
| side            | number | `1`                       | 合约方向（0：`反向`，1：`正向`）                           |
| multiplier      | number | `1.0000000000000000`      | 合约面值                                                  |
| minOrderVolume  | number | `1`                       | 最小下单量                                                |
| minOrderMoney   | number | `1.0000000000000000`      | 最小下单金额                                              |
| maxMarketVolume | number | `200000`                  | 市价单最大下单数量                                        |
| maxMarketMoney  | number | `500000.0000000000000000` | 市价最大下单金额                                          |
| maxLimitVolume  | number | `300000`                  | 限价单最大下单数量                                        |
| maxLimitMoney   | number | `500000.0000000000000000` | 限价最大下单金额                                          |
| maxValidOrder   | number | `10`                      | 最大有效委托的订单数量                                    |

## 行情

### 安全类型: None

<aside class="notice">行情下方的接口不需要API-Key或者签名就能自由访问</aside>

### 订单薄

`GET https://futuresopenapi.xxx.com/fapi/v1/depth`

市场订单薄深度信息

> 请求示例

```http
GET https://futuresopenapi.xxx.com/fapi/v1/depth?contractName=E-BTC-USDT&limit=100
```

**请求参数**

| 参数名                                  | 类型    | 描述                               |
| :---------------------------------------| :-------| :----------------------------------|
| contractName<font color="red">\*</font> | string  | `大写`合约名称，例如：`E-BTC-USDT` |
| limit                                   | integer | 默认：100；最大：100               |

> 返回示例

```json
{
    "time": 1704962463000,
    "bids": [
        [
            3.90000000,     //价格
            16.10000000     //数量
        ],
        [
            4.00000000,
            29.30000000
        ]
    ],
    "asks": [
        [
            4.00000200,     //价格
            12.00000000     //数量
        ],
        [
            5.10000000,
            28.00000000
        ]
    ]
}
```

**返回参数**

| 参数名 | 类型 | 示例                             | 描述                                                                                   |
| :------| :----| :--------------------------------| :--------------------------------------------------------------------------------------|
| time   | long | `1595563624731`                  | 当前时间戳                                                                             |
| bids   | list | `[[3.9,16.1],[4.0,29.3]]`        | 订单薄买盘信息，数组第一位为价格，类型为float；第二位为当前价格对应的数量，类型为float |
| asks   | list | `[[4.00000200,12.0],[5.1,28.0]]` | 订单薄卖盘信息，数组第一位为价格，类型为float；第二位为当前价格对应的数量，类型为float |

bids和asks所对应的信息代表了订单薄的所有价格以及价格对应的数量的信息, 由最优价格从上到下排列

### 行情Ticker

`GET https://futuresopenapi.xxx.com/fapi/v1/ticker`

24小时价格变化数据

> 请求示例

```http
GET https://futuresopenapi.xxx.com/fapi/v1/ticker?contractName=E-BTC-USDT
```

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |


> 返回示例

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

**返回参数**

| 参数名 | 类型   | 示例            | 描述                                      |
| :------| :------| :---------------| :-----------------------------------------|
| time   | long   | `1595563624731` | 时间戳                                    |
| high   | float  | `56120.22`      | 最高价                                    |
| low    | float  | `55982.24`      | 最低价                                    |
| last   | float  | `55989.93`      | 最新价                                    |
| vol    | float  | `51.21`         | 交易量                                    |
| rose   | string | `+0.05`         | 涨跌幅，`+`为涨，`-`为跌，`+0.05`为涨`5%` |
| buy    | float  | `55988.10`      | 买一价格                                  |
| sell   | float  | `55990.10`      | 卖一价格                                  |

### K线/蜡烛图数据

`GET https://futuresopenapi.xxx.com/fapi/v1/klines`

> 请求示例

```http
GET https://futuresopenapi.xxx.com/fapi/v1/klines?contractName=E-BTC-USDT&interval=1min&limit=100&startTime=1111111100000&endTime=222222222000000
```

**请求参数**

| 参数名                                  | 类型    | 描述                                                                                                                                            |
| :---------------------------------------| :-------| :-----------------------------------------------------------------------------------------------------------------------------------------------|
| contractName<font color="red">\*</font> | string  | `大写`合约名称，例如：`E-BTC-USDT`                                                                                                              |
| interval<font color="red">\*</font>     | string  | K线图区间，可识别的参数值为：`1min`，`5min`，`15min`，`30min`，`1h`，`1day`，`1week`，`1month`（min=分钟，h=小时，day=天，week=星期，month=月） |
| limit                                   | integer | 默认：100；最大：300                                                                                                                            |
| startTime                               | long    | 开始时间戳                                                                                                                                      |
| endTime                                 | long    | 结束时间戳                                                                                                                                      |

> 返回示例

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

**返回参数**

| 参数名 | 类型  | 示例             | 描述       |
| :------| :-----| :----------------| :----------|
| idx    | long  | `15946403400000` | 开始时间戳 |
| open   | float | `6195.80`        | 开盘价     |
| close  | float | `6210.51`        | 收盘价     |
| high   | float | `6228.77`        | 最高价     |
| low    | float | `6190.48`        | 最低价     |
| vol    | float | `111`            | 成交量     |

## 交易

### 安全类型: TRADE

<aside class="notice">交易下方的接口都需要签名和API-key验证</aside>

### 创建订单

`POST https://futuresopenapi.xxx.com/fapi/v1/order`

创建单个新订单

**请求头**

| 参数名                                 | 类型   | 描述        |
| :--------------------------------------| :------| :-----------|
| X-CH-TS<font color="red">\*</font>     | string | 时间戳      |
| X-CH-APIKEY<font color="red">\*</font> | string | 您的API-KEY |
| X-CH-SIGN<font color="red">\*</font>   | string | 签名        |

> 请求示例

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

**请求参数**

| 参数名                                  | 类型   | 描述                                                             |
| :---------------------------------------| :------| :----------------------------------------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT`                               |
| price                                   | number | 下单价格，限价单时该字段为必传，有精度限制，精度由管理员设置       |
| volume<font color="red">\*</font>       | number | 下单张数，有精度限制，精度由管理员设置，市价开仓时这里单位是价值 |
| type<font color="red">\*</font>         | string | 订单类型，`LIMIT/MARKET`                                         |
| side<font color="red">\*</font>         | string | 买卖方向，`BUY/SELL`                                             |
| open<font color="red">\*</font>         | string | 开平仓方向，`OPEN/CLOSE`                                         |
| positionType<font color="red">\*</font> | number | 持仓类型，`1:全仓/2:逐仓`                                        |
| timeInForce                             | string | `IOC, FOK, POST_ONLY`                                            |
| clientOrderId                           | string | 客户端下单标识，长度小于32位的字符串                             |

> 返回示例

```json
{
    "orderId": 256609229205684228
}
```

**返回参数**

| 参数名  | 类型   | 示例                 | 描述   |
| :-------| :------| :--------------------| :------|
| orderId | string | `256609229205684228` | 订单ID |

### 创建条件单

`POST https://futuresopenapi.xxx.com/fapi/v1/conditionOrder`

**请求头**

| 参数名                                 | 类型   | 描述        |
| :--------------------------------------| :------| :-----------|
| X-CH-TS<font color="red">\*</font>     | string | 时间戳      |
| X-CH-APIKEY<font color="red">\*</font> | string | 您的API-KEY |
| X-CH-SIGN<font color="red">\*</font>   | string | 签名        |

> 请求示例

```http
POST https://futuresopenapi.xxx.com/fapi/v1/conditionOrder

body
{
    "contractName": "E-BTC-USDT"
}
```

**请求参数**

| 参数名                                  | 类型   | 描述                                                             |
| :---------------------------------------| :------| :----------------------------------------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT`                               |
| price<font color="red">\*</font>        | number | 下单价格，有精度限制，精度由管理员设置                           |
| volume<font color="red">\*</font>       | number | 下单数量，市价开仓时这里单位是价值，有精度限制，精度由管理员设置 |
| type<font color="red">\*</font>         | string | 订单类型，`LIMIT`/`MARKET`                                      |
| side<font color="red">\*</font>         | string | 买卖方向，`BUY`/`SELL`                                           |
| positionType<font color="red">\*</font> | number | 持仓类型，1：`全仓`，2：`逐仓`                                   |
| open<font color="red">\*</font>         | string | 开平仓方向，`OPEN`/`CLOSE`                                       |
| triggerType<font color="red">\*</font>  | string | 条件的类型，1：`止损`，2：`止盈`，3：`追涨`，4：`杀跌`           |
| triggerPrice<font color="red">\*</font> | string | 触发价                                                           |
| clientOrderId                           | string | 客户端下单标识，长度小于32位的字符串                             |

> 返回示例

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

如果该接口返回与期望不符，请联系技术团队，我们会为您提供相关帮助

### 取消订单

`POST https://futuresopenapi.xxx.com/fapi/v1/cancel`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |
| orderId<font color="red">\*</font>      | string | 订单ID                             |

> 返回示例

```json
{
    "orderId": "256609229205684228"
}
```

### 取消条件单

`POST https://futuresopenapi.xxx.com/fapi/v1/cancel_trigger_order`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |
| orderId<font color="red">\*</font>      | string | 订单ID                             |
| clientOrderId                           | string | 客户端唯一标识，默认：0            |

> 返回示例

```json
{
    "orderId": "256609229205684228"
}
```

### 订单详情

`GET https://futuresopenapi.xxx.com/fapi/v1/order`

**请求参数**

| 参数名                                   | 类型   | 描述                               |
| :----------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font>  | string | `大写`合约名称，例如：`E-BTC-USDT` |
| orderId<font color="red">\*</font>       | string | 订单ID                             |
| clientOrderId<font color="red">\*</font> | string | 客户端唯一标识，默认：0            |

> 返回示例

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

**返回参数**

| 参数名       | 类型    | 示例                     | 描述                                                                                                                                                  |
| :------------| :-------| :------------------------| :-----------------------------------------------------------------------------------------------------------------------------------------------------|
| orderId      | long    | `2006628907041292645`    | 订单ID（系统生成                                                                                                                                      |
| contractName | string  | `E-BTC-USDT`             | `大写`合约名称                                                                                                                                        |
| price        | float   | `67000.0000000000000000` | 委托价格                                                                                                                                              |
| origQty      | float   | `2.0000000000000000`     | 委托数量                                                                                                                                              |
| executedQty  | float   | `0`                      | 成交数量                                                                                                                                              |
| avgPrice     | float   | `0`                      | 成交均价                                                                                                                                              |
| status       | string  | `NEW`                    | 订单状态。可能出现的值为：`NEW`（新订单，无成交）、`PARTIALLY_FILLED`（部分成交）、`FILLED`（全部成交）、`CANCELED`（已取消）和`REJECTED`（订单被拒绝） |
| side         | string  | `BUY`                    | 订单方向。可能出现的值只能为：`BUY`（买入做多）和`SELL`（卖出做空）                                                                                   |
| action       | string  | `OPEN`                   | `OPEN/CLOSE`                                                                                                                                          |
| transactTime | long    | `1704967622000`          | 订单创建时间                                                                                                                                          |
| type         | string  | `LIMIT`                  | 订单类型`LIMIT`/`MARKET`                                                                                                                              |
| timeInForce  | integer | `1`                      | 条件单有效方式，1：`limit`，2：`market`，3：`IOC`，4：`FOK`，5： `POST\_ONLY`                                                                         |
| fills        | array   |                          | 成交记录                                                                                                                                              |

### 当前订单

`GET https://futuresopenapi.xxx.com/fapi/v1/openOrders`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型   | 描述        |
| :--------------------------------------| :------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | string | 时间戳      |

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |

> 返回示例

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

返回参数

| 参数名       | 类型   | 示例                     | 描述                                                                                                                                                    |
| :------------| :------| :------------------------| :-------------------------------------------------------------------------------------------------------------------------------------------------------|
| orderId      | long   | `259396989397942275`     | 订单ID（系统生成）                                                                                                                                      |
| contractName | string | `E-BTC-USDT`             | `大写`合约名称                                                                                                                                          |
| price        | float  | `72000.0000000000000000` | 订单价格                                                                                                                                                |
| origQty      | float  | `1.0000000000000000`     | 订单数量                                                                                                                                                |
| executedQty  | float  | `0.5`                    | 已经成交订单数量                                                                                                                                        |
| avgPrice     | float  | `71990.0`                | 订单已经成交的平均价格                                                                                                                                  |
| type         | string | `LIMIT`                  | 订单类型。可能出现的值只能为：`LIMIT`（限价）和`MARKET`（市价）                                                                                         |
| side         | string | `BUY`                    | 订单方向。可能出现的值只能为：`BUY`（买入做多）和 `SELL`（卖出做空）                                                                                    |
| status       | string | `NEW`                    | 订单状态。可能出现的值为：`NEW`（新订单，无成交）、`PARTIALLY_FILLED`（部分成交）、`FILLED`（全部成交）、`CANCELED`（已取消）和`REJECTED`（订单被拒绝） |
| action       | string | `OPEN`                   | `OPEN/CLOSE`                                                                                                                                            |
| transactTime | long   | `1607702400000`          | 订单创建时间戳                                                                                                                                          |

### 历史委托

`POST https://futuresopenapi.xxx.com/fapi/v1/orderHistorical`

**请求头**

| 参数名                                 | 类型   | 描述        |
| :--------------------------------------| :------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | string | 时间戳      |

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |
| limit                                   | string | 分页条数，默认：100；最大：1000    |
| fromId                                  | long   | 从这条记录开始检索                 |

> 返回示例

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

如果该接口返回与期望不符，请联系技术团队，我们会为您提供相关帮助

### 盈亏记录

`POST https://futuresopenapi.xxx.com/fapi/v1/profitHistorical`

如果该接口返回报错，请联系技术团队，我们会为您提供相关帮助

**请求头**

| 参数名                                 | 类型   | 描述        |
| :--------------------------------------| :------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | string | 时间戳      |

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |
| limit                                   | string | 分页条数，默认：100；最大：1000    |
| fromId                                  | long   | 从这条记录开始检索                 |

> 返回示例

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

如果该接口返回与期望不符，请联系技术团队，我们会为您提供相关帮助

### 交易记录

`GET https://futuresopenapi.xxx.com/fapi/v1/myTrades`

**限速规则: 20次/2s**

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |

**请求参数**

| 参数名                                  | 类型   | 描述                               |
| :---------------------------------------| :------| :----------------------------------|
| contractName<font color="red">\*</font> | string | `大写`合约名称，例如：`E-BTC-USDT` |
| limit                                   | string | 分页条数，默认：100；最大：1000    |
| fromId                                  | long   | 从这个tradeId开始检索              |

> 返回示例

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

**返回参数**

| 参数名       | 类型    | 示例                  | 描述                                    |
| :------------| :-------| :---------------------| :---------------------------------------|
| tradeId      | number  | `1528`                | 交易ID                                  |
| bidId        | long    | `1874564572563538130` | 买方订单ID                              |
| askId        | long    | `1954072405852309104` | 卖方订单ID                              |
| bidUserId    | integer | `10034`               | 买方用户ID                              |
| askUserId    | integer | `10378`               | 卖方用户ID                              |
| price        | float   | `10.0`                | 成交价格                                |
| qty          | float   | `3`                   | 交易数量                                |
| amount       | float   | `30.0`                | 成交金额                                |
| time         | number  | `1499865549590`       | 交易时间戳                              |
| fee          | number  | `0.001`               | 交易手续费                              |
| side         | string  | `BUY`                 | 当前订单方向，`BUY`：买入，`SELL`：卖出 |
| contractName | string  | `E-BTC-USDT`          | `大写`合约名称                          |
| isMaker      | boolean | `true`                | 是否是maker                             |
| isBuyer      | boolean | `true`                | 是否买方                                |

### 更改持仓模式

`POST https://futuresopenapi.xxx.com/fapi/v1/edit_user_position_model`

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |

**请求参数**

| 参数名                                   | 类型    | 描述                                 |
| :----------------------------------------| :-------| :------------------------------------|
| contractName<font color="red">\*</font>  | string  | 合约名称，例如：`E-BTC-USDT`         |
| positionModel<font color="red">\*</font> | integer | 持仓模式，1：`净持仓`，2：`双向持仓` |

> 返回示例

```json
{
    "code": "0",
    "msg": "成功",
    "data": null
}
```

### 更改保证金模式

`POST https://futuresopenapi.xxx.com/fapi/v1/edit_user_margin_model`

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |

**请求参数**

| 参数名                                  | 类型    | 描述                                 |
| :---------------------------------------| :-------| :------------------------------------|
| contractName<font color="red">\*</font> | string  | 合约名称，例如：`E-BTC-USDT`         |
| marginModel<font color="red">\*</font>  | integer | 持仓模式，1：`净持仓`，2：`双向持仓` |

> 返回示例

```json
{ 
    "code": "0", 
    "msg": "成功", 
    "data": null 
}
```

### 更改杠杆倍数

`POST` `https://futuresopenapi.xxx.com/fapi/v1/edit_lever`

**请求头**

| 参数名                                 | 类型    | 描述        |
| :--------------------------------------| :-------| :-----------|
| X-CH-TS<font color="red">\*</font>     | integer | 时间戳      |
| X-CH-APIKEY<font color="red">\*</font> | string  | 您的API-key |
| X-CH-SIGN<font color="red">\*</font>   | string  | 签名        |