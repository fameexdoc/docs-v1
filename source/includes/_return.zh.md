# 返回码类型

异常码和错误码的描述和原因

<aside class="warning">以下返回内容均为基本参数校验，若返回码不包含在以下列出的返回码类型中，则为业务层以外的错误提示返回，需要联系技术人员进行处理</aside>

## 10XX - 通用服务器和网络错误

### Code:-1000 UNKNOWN

msg：An unknown error occurred while processing the request

原因：处理请求时发生未知错误

### Code:-1001 DISCONNECTED

msg：内部错误; 无法处理您的请求。 请再试一次

原因：内部错误; 无法处理您的请求

### Code:-1002 UNAUTHORIZED

msg：You are not authorized to execute this request. The request needs to send an API Key, and we recommend appending the X-CH-APIKEY to all request headers

原因：header中未添加签名所需要的X\_CH\_APIKEY

### Code:-1003 TOO\_MANY\_REQUESTS

msg：请求过于频繁超过限制

原因：请求过于频繁超过限制

### Code:-1004 NO\_THIS\_COMPANY

msg：您无权执行此请求 user not exit

原因：您无权执行此请求 user not exit

### Code:-1006 UNEXPECTED\_RESP

msg：接收到了不符合预设格式的消息，下单状态未知

原因：接收到了不符合预设格式的消息，下单状态未知

### Code:-1007 TIMEOUT

msg：等待后端服务器响应超时。 发送状态未知，执行状态未知

原因：请求超时

### Code:-1014 UNKNOWN\_ORDER\_COMPOSITION

msg：不支持的订单组合

原因：订单组合不存在或输入了错误的订单组合

### Code:-1015 TOO\_MANY\_ORDERS

msg：Too many orders. Please reduce the number of your orders

原因：下单数量超过最大数量限制

### Code:-1016 SERVICE\_SHUTTING\_DOWN

msg：服务器下线

原因：服务器已下线，无法访问该接口

### Code:-1017 NO\_CONTENT\_TYPE

msg：我们建议在所有的请求头附加Content-Type, 并设置成application/json

原因：请求头中缺少Content-Type

### Code:-1020 UNSUPPORTED\_OPERATION

msg：不支持此操作

原因：进行了错误的请求操作，需要同技术团队进行对接解决

### Code:-1021 INVALID\_TIMESTAMP

msg：Invalid timestamp with too large time offset

原因：时间戳偏移偏大，服务器根据请求中的时间戳判定客户端时间比服务器时间提前了1秒钟以上.

### Code:-1022 INVALID\_SIGNATURE

msg：Invalid signature

原因：签名验证失败

### Code:-1023 UNAUTHORIZED

msg：You are not authorized to execute this request. The request need to send timestamps, and we recommend appending X-CH-TS to all request headers

原因：header中未添加签名所需要的X-CH-TS

### Code:-1024 UNAUTHORIZED

msg：You are not authorized to execute this request. The request needs to send sign, and we recommend appending X-CH-SIGN to all request headers

原因：header中未添加签名所需要的X-CH-SIGN

## 11XX - 请求内容中的问题

### Code:-1100 ILLEGAL\_CHARS

msg：请求中存在非法字符

原因：请求参数中存在非法字符

### Code:-1101 TOO\_MANY\_PARAMETERS

msg：发送的参数太多

原因：参数内容过多或检测到的参数值重复

### Code:-1102 MANDATORY\_PARAM\_EMPTY\_OR\_MALFORMED

msg: Forced parameter XXX not sent, empty or incorrect format

原因：参数为空，必传参数未传或不正确的入参格式

### Code:-1103 UNKNOWN\_PARAM

msg：发送了未知参数

原因：请求参数中的参数内容或者格式错误，请检查是否字段中包含空格

### Code:-1104 UNREAD\_PARAMETERS

msg：并非所有发送的参数都被读取

原因：并非所有发送的参数都被读取； 读取了'％s'参数，但被发送了'％s'

### Code:-1105

msg：The parameter XXX is empty

原因：必传参数为空

### Code:-1106 PARAM\_NOT\_REQUIRED

msg：不需要发送此参数

原因：不需要时发送参数'％s'

### Code:-1111 BAD\_PRECISION

msg：精度超过此资产定义的最大值

原因：精度超过此资产定义的最大值

### Code:-1112

msg：There are no pending orders for trading pairs

原因：需要取消的该订单不存在

### Code:-1116 INVALID\_ORDER\_TYPE

msg：Invalid order type

原因：无效订单类型

### Code:-1117 INVALID\_SIDE

msg：Invalid buying or selling direction

原因：无效买卖方向

### Code:-1121

msg: Invalid contract

原因：币对名称输入错误或合约名称输入错误

### Code:-1136 ORDER\_QUANTITY\_TOO\_SMALL

msg：订单数量小于最小值

原因：订单quantity小于最小值

### Code:-1138 ORDER\_PRICE\_WAVE\_EXCEED

msg：The order price is outside the allowable range

原因：订单价格超出允许范围

### Code:-1139 ORDER\_NOT\_SUPPORT\_MARKET

msg：The pair does not support market trading

原因：该交易对不支持市价交易

### Code:-1145

msg：The order status does not allow for cancellation

原因：订单不能够被取消

### Code:-1147 PRICE\_VOLUME\_PRESION\_ERROR

msg：价格或数量精度超过最大限制

原因：订单的价格或数量超过最大限制

## 2XXX - 其他相关返回码

### Code:-2013 NO\_SUCH\_ORDER

msg：订单不存在

原因：订单不存在

### Code:-2015

msg：Invalid API key, IP or operation permission

原因：签名或IP不通过

### Code:-2016

msg：Transactions are frozen

原因：该用户交易被冻结

### Code:-2017 balance\_not\_enough

msg：余额不足

原因：用户该账户中余额不足

### Code:-2100

msg：Parameter error

原因：参数输入错误

### Code:-2200

msg：Illegal IP

原因：不是信任的IP

### Code:35

msg：禁止下单

原因：用户交易可能被限制