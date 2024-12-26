# 文档说明

## 接口示例

下方接口为获取服务器时间的接口，以该接口作为示例，展示接口格式、访问链接和参数说明描述。

### 获取服务器时间

`GET https://openapi.xxx.com/sapi/v1/time`

获取服务器时间

> 请求示例

```http
GET https://openapi.xxx.com/sapi/v1/time
```

> 返回示例

```json
{
    "timezone": "China Standard Time",
    "serverTime": 1705039779880
}
```

**返回参数**

| 参数名     | 类型   | 示例                  | 描述         |
| :----------| :------| :---------------------| :------------|
| timezone   | string | `China Standard Time` | 服务器时区   |
| serverTime | long   | `1705039779880`       | 服务器时间戳 |

## 文档入参规范

入参名称的带有红色<font color="red">\*</font>号，说明该参数为必传，反之为非必传。

接口对入参字符大小写敏感，在接口中会明确指明。如写明需要输入大写的币对名称，则需要输入BTCUSDT，输入小写的btcusdt是不被允许的。

文档中的入参有明确的类型说明，需要按照指定的类型输入。如integer类型只能输入数字类型，3的输入是正确的，但"3"的输入是不被允许的。

## 接口是否需要签名验证

接口类型分为：公共、行情、交易、账户。

*   公共和行情类下的接口无需API-Key或者签名就可访问。
*   交易和账户安全需要API-Key和签名验证后才可访问。
*   签名内容和参数有关，若参数输入错误则会返回提示参数错误或空值。
*   需要签名验证的接口 要在Header中添加X-CH-SIGN，X-CH-APIKEY，X-CH-TS进行验签。签名规则和示例可参照：[签名示例](https://fameexdoc.github.io/docs-v1/?http#09957702bc)(文档中的apiKey，secretKey均为虚拟值；真实内容需要用户在前台页面的API管理中申请获取)。

## 返回码的类型

见 [返回码类型](https://fameexdoc.github.io/docs-v1/?http#8ec82582cd)

## API基本信息

*   本篇列出REST接口的baseurl`https://openapi.xxx.com`。
*   所有接口都会返回一个JSON object或者array。
*   响应中如有数组，数组元素以时间倒序排列，越早的数据越靠前。
*   所有时间、时间戳均为Unix时间，单位为**毫秒**。

HTTP返回代码

*   HTTP`4XX`错误码用于指示错误的请求内容、行为、格式。
*   HTTP`429`错误码表示警告访问频次超限，即将被封IP。
*   HTTP`418`表示收到429后继续访问，于是被封了。
*   HTTP`5XX`返回错误码是内部系统错误；这说明这个问题是在服务器这边。在对待这个错误时，**千万**不要把它当成一个失败的任务，因为执行状态未知，有可能是成功也有可能是失败。
*   HTTP`504`表示API服务端已经向业务核心提交了请求但未能获取响应，特别需要注意的是`504`代码不代表请求失败，而是未知。很可能已经得到了执行，也有可能执行失败，需要做进一步确认。
*   任何接口都可能返回ERROR（错误）；错误的返回payload如下：

> 返回示例

```json
{
    "code": -1121,
    "msg": "Invalid symbol."
}
```

## 接口通用信息

*   所有请求基于Https协议，请求头信息中`Content-Type`需要统一设置为：`'application/json'`。
*   `GET`方法的接口，参数必须在`query string`中发送。
*   `POST`方法的接口，参数必须在`request body`中发送。
*   对参数的顺序不做要求。

## 访问限制

*   在每个接口下面会有限频的说明。
*   违反频率限制都会收到`HTTP 429`，这是一个警告。
*   当收到`HTTP 429`告警时，调用者应当降低访问频率或者停止访问。

## 接口鉴权类型

*   每个接口都有自己的鉴权类型，鉴权类型决定了访问时应当进行何种鉴权。
*   如果需要API-key，应当在HTTP头中以`X-CH-APIKEY`字段传递。
*   API-key与API-secret是大小写敏感的。
*   可以在网页用户中心修改API-key所具有的权限，例如读取账户信息、发送交易指令、发送提现指令。

| 鉴权类型     | 描述                    |
| :------------| :-----------------------|
| NONE         | 不需要鉴权的接口        |
| TRADE        | 需要有效的API-KEY和签名 |
| USER\_DATA   | 需要有效的API-KEY和签名 |
| USER\_STREAM | 需要有效的API-KEY       |
| MARKET\_DATA | 需要有效的API-KEY       |

## 需要签名的接口 (TRADE 与 USER\_DATA)

*   调用`TRADE`或者`USER_DATA`接口时，应当在HTTP头中以`X-CH-SIGN`字段传递签名参数。
*   签名使用`HMAC SHA256`算法，API-KEY所对应的API-Secret作为 `HMAC SHA256` 的密钥。
*   `X-CH-SIGN`的请求头是以timestamp + method + requestPath + body字符串（+表示字符串连接）作为操作对象。
*   其中timestamp的值与`X-CH-TS`请求头相同，method是请求方法，字母全部大写：`GET`/`POST`。
*   requestPath是请求接口路径，例如:`sapi/v1/order?symbol=ethusdt&orderID=111000111`。
*   `body`是请求主体的字符串（post only），如果是`GET`请求则`body`可省略。
*   签名大小写不敏感。

## 时间同步安全

*   签名接口均需要在HTTP头中以`X-CH-TS`字段传递时间戳，其值应当是请求发送时刻的Unix时间戳（毫秒），E.g. `1528394129373`。
*   服务器收到请求时会判断请求中的时间戳，如果是5000毫秒之前发出的，则请求会被认为无效。这个时间窗口值可以通过发送可选参数`recvWindow`来自定义。
*   另外，如果服务器计算得出客户端时间戳在服务器时间的`未来`一秒以上，也会拒绝请求。
*   Java逻辑伪代码：

```java
if (timestamp < (serverTime + 1000) && (serverTime - timestamp) <= recvWindow) {
  // process request
} else {
  // reject request
}
```

<aside class="notice">关于交易时效性：互联网状况并不100%可靠，不可完全依赖，因此你的程序本地到交易所服务器的时延会有抖动。这是我们设置<code>recvWindow</code>的目的所在，如果你从事高频交易，对交易时效性有较高的要求，可以灵活设置<code>recvWindow</code>以达到你的要求。不推荐使用5秒以上的<code>recvWindow</code>。</aside>

## POST /sapi/v1/order/test 的示例

以下是在linux bash环境下使用echo，openssl和curl工具实现的一个调用接口下单的示例，其中apikey、secretKey仅供示范，请将其替换为您的真实`apiKey`和`secretKey`。

| Key       | Value                            |
| :---------| :--------------------------------|
| apiKey    | vmPUZE6mv9SD5V5e14y7Ju91duEh8A   |
| secretKey | 902ae3cb34ecee2779aa4d3e1d226686 |

| 参数名 | 取值    |
| :------| :-------|
| symbol | BTCUSDT |
| side   | BUY     |
| type   | LIMIT   |
| volume | 1       |
| price  | 9300    |

> 签名示例

> body

```json
{"symbol":"BTCUSDT","price":"9300","volume":"1","side":"BUY","type":"LIMIT"}
```

> HMAC-SHA256 签名

```shell
echo -n "1588591856950POST/sapi/v1/order/test{\"symbol\":\"BTCUSDT\",\"price\":\"9300\",\"volume\":\"1\",\"side\":\"BUY\",\"type\":\"LIMIT\"}" | openssl dgst -sha256 -hmac "902ae3cb34ecee2779aa4d3e1d226686"
(stdin)= c50d0a74bb9427a9a03933d0eded03af9bf50115dc5b706882a4fcf07a26b761
```

> curl

```shell
curl -H "X-CH-APIKEY: c3b165fd5218cdd2c2874c65da468b1e" -H "X-CH-SIGN: c50d0a74bb9427a9a03933d0eded03af9bf50115dc5b706882a4fcf07a26b761
```