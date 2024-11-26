# Return Code Type

Descriptions and reasons of exception codes

<aside class="warning">The following return content is basic parameter verification. If the return code is not included in the return code types listed below, it is an error message returned from outside the business layer and needs to be docked with technical personnel.</aside>

## 10XX - General Server or Network Issues

### Code:-1000 UNKNOWN

msg: An unknown error occurred while processing the request.

Reason: An unknown error occurred while processing the request.

### Code:-1001 DISCONNECTED

msg: Internal error; unable to process your request. Please try again.

Reason: Internal error; unable to process your request.

### Code:-1002 UNAUTHORIZED

msg: You are not authorized to execute this request. The request needs to send an API Key, and we recommend appending the X-CH-APIKEY to all request headers.

Reason: The X\_CH\_APIKEY required for signature is not added to the header.

### Code:-1003 TOO\_MANY\_REQUESTS

msg: Requests exceed the limit too frequently.

Reason: Requests exceed the limit too frequently.

### Code:-1004 NO\_THIS\_COMPANY

msg: You are not authorized to execute this request. User does not exist.

Reason: You are not authorized to execute this request. User does not exist.

### Code:-1006 UNEXPECTED\_RESP

msg: An unexpected response was received from the message bus. Execution status unknown.

Reason: An unexpected response was received from the message bus. Execution status unknown. The OPEN API server finds some exceptions in executing the request. Please report to our Customer Service.

### Code:-1007 TIMEOUT

msg: Timeout waiting for response from backend server. Send status unknown; execution status unknown.

Reason: Request timed out.

### Code:-1014 UNKNOWN\_ORDER\_COMPOSITION

msg: Unsupported order combination.

Reason: The order combination does not exist or an incorrect order combination was entered.

### Code:-1015 TOO\_MANY\_ORDERS

msg: Too many orders. Please reduce the number of your orders.

Reason: The order quantity exceeds the maximum quantity limit.

### Code:-1016 SERVICE\_SHUTTING\_DOWN

msg: This server is no longer available.

Reason: This server is no longer available and the interface cannot be accessed.

### Code:-1017 NO\_CONTENT\_TYPE

msg: We recommend attaching Content-Type to all request headers and setting it to application/json.

Reason: The Content-Type is missing in the request header.

### Code:-1020 UNSUPPORTED\_OPERATION

msg: This operation is not supported.

Reason: An incorrect request operation was made, and coordination with the technical team is required to resolve the issue.

### Code:-1021 INVALID\_TIMESTAMP

msg: Invalid timestamp with too large time offset.

Reason: The timestamp offset is too large. Timestamp for this request was 1000ms ahead of the server's time. Please check the difference between your local time and server time.

### Code:-1022 INVALID\_SIGNATURE

msg: Invalid signature.

Reason: Signature verification failed.

### Code:-1023 UNAUTHORIZED

msg: You are not authorized to execute this request. The request need to send timestamps, and we recommend appending X-CH-TS to all request headers.

Reason: The X-CH-TS required for signature is not added to the header.

### Code:-1024 UNAUTHORIZED

msg: You are not authorized to execute this request. The request needs to send sign, and we recommend appending X-CH-SIGN to all request headers.

Reason: The X-CH-SIGN required for signature is not added to the header.

## 11XX - Request Issues

### Code:-1100 ILLEGAL\_CHARS

msg: Illegal characters found in a parameter.

Reason: Illegal characters exist in the request parameters.

### Code:-1101 TOO\_MANY\_PARAMETERS

msg: Too many parameters sent for this endpoint.

Reason: The parameter content is too large or duplicate parameter values were detected.

### Code:-1102 MANDATORY\_PARAM\_EMPTY\_OR\_MALFORMED

msg: Forced parameter XXX not sent, empty or incorrect format.

Reason: The parameter is null,  a required parameter is missing, or the input parameters are in an incorrect format.

### Code:-1103 UNKNOWN\_PARAM

msg: An unknown parameter was sent.

Reason: The parameter content or format in the request is incorrect. Please check if the fields contain any spaces.

### Code:-1104 UNREAD\_PARAMETERS

msg: Not all sent parameters were read.

Reason: Not all sent parameters were read; read '%s' parameter(s) but was sent '%s'.

### Code:-1105

msg: The parameter XXX is empty.

Reason: The required parameter is empty.

### Code:-1106 PARAM\_NOT\_REQUIRED

msg: This parameter is not required.

Reason: Parameter '%s' sent when not required.

### Code:-1111 BAD\_PRECISION

msg: Precision is over the maximum defined for this asset.

Reason: Precision is over the maximum defined for this asset.

### Code:-1112

msg: There are no pending orders for trading pairs.

Reason: The order that needs to be canceled does not exist.

### Code:-1116 INVALID\_ORDER\_TYPE

msg: Invalid order type.

Reason: Invalid order type.

### Code:-1117 INVALID\_SIDE

msg: Invalid buying or selling direction.

Reason: Invalid side.

### Code:-1121

msg: Invalid contract.

Reason: Invalid symbol.

### Code:-1136 ORDER\_QUANTITY\_TOO\_SMALL

msg: The order quantity is less than the minimum value.

Reason: Order quantity lower than the minimum.

### Code:-1138 ORDER\_PRICE\_WAVE\_EXCEED

msg: The order price is outside the allowable range.

Reason: Order price exceeds the permissible range.

### Code:-1139 ORDER\_NOT\_SUPPORT\_MARKET

msg: The pair does not support market trading.

Reason: This trading pair does not support market trading.

### Code:-1145

msg: The order status does not allow for cancellation.

Reason: The order cannot be canceled.

### Code:-1147 PRICE\_VOLUME\_PRESION\_ERROR

msg: The price or quantity precision exceeds the maximum limit.

Reason: Order price or quantity exceeds the maximum limit.

## 2XXX - Other Issues

### Code:-2013 NO\_SUCH\_ORDER

msg: The order does not exist.

Reason: The order does not exist.

### Code:-2015

msg: Invalid API key, IP, or operation permission.

Reason: Signature or IP is incorrect.

### Code:-2016

msg: Transactions are frozen.

Reason: The user's transaction is frozen.

### Code:-2017 balance\_not\_enough

msg: Insufficient balance.

Reason: Insufficient balance in the user's account.

### Code:-2100

msg: Parameter error.

Reason: Input wrong parameter or exist block in the parameter.

### Code:-2200

msg: Illegal IP.

Reason: Not a trusted IP.

### Code:35

msg: Forbidden to order.

Reason: User's transactions may be restricted.