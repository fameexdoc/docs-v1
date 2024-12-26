# Documentation Description

## Example of Interface

The interface below is the interface for obtaining server time. This interface is used as an example to show the interface format, access link, and parameter description.

### Check Server Time

`GET https://openapi.xxx.com/sapi/v1/time`

Check Server Time

> request example

```http
GET https://openapi.xxx.com/sapi/v1/time
```

> response example

```json
{
    "timezone": "China Standard Time",
    "serverTime": 1705039779880
}
```

**Response Parameter**

| Parameter  | Type   | Example               | Description      |
| :----------| :------| :---------------------| :----------------|
| timezone   | string | `China Standard Time` | Server time zone |
| serverTime | long   | `1705039779880`       | Server timestamp |

## Document Parameter Specifications

The red <font color="red">\*</font> in the upper right corner of the input parameter name indicates that the parameter must be inputted, otherwise it is not required.

The interface is case-sensitive to input parameter characters, which will be clearly specified in the interface. If the currency pair name is in uppercase, you need to enter BTCUSDT, but btcusdt is not allowed.

The input parameters in the document have clear type descriptions and need to be entered according to the specified type. For example, the integer type can only input numeric types. The input of 3 is correct, but the input of "3" is not allowed.

## Does the Interface Require Signature Verification?

Interface types are divided into: Public, Market, Trade, Account.

*   The interfaces under the "Public" and "Market" categories can be accessed without requiring an API key or signature.
*   "Trade" and "Account" security require API-Key and signature verification before access.
*   The signature content is related to the parameters. If the parameters are entered incorrectly, the parameter error or null value will be returned.
*   For interfaces that require signature verification, X-CH-SIGN, X-CH-APIKEY, and X-CH-TS must be added to the header for signature verification. For signature rules and examples, please refer to: [Signature Example](https://fameexdoc.github.io/docs-v1/?http#09957702bc) (apiKey and secretKey in this document are virtual values. The real content needs to be obtained by the user in the API management of the front page.)

## Return Code Type

Please refer to the document: [Return Enums](https://fameexdoc.github.io/docs-v1/?http#8ec82582cd)

## API Basic Information

*   baseurl `https://openapi.xxx.com`
*   All endpoints return either a JSON object or array. 
*   Data is returned in Reverse order. newest first, oldest last.
*   All time and timestamp related fields are in **milliseconds**.

HTTP Error Codes

*   HTTP `4XX` return codes are used for malformed requests; the issue is on the sender's side.
*   HTTP `429` return code is used when breaking a request rate limit.
*   HTTP `418` return code is used when an IP has been auto-banned for continuing to send requests after receiving the 429 code.
*   HTTP `5XX` return codes are used for internal errors.
*   HTTP `504` return code is used when the API successfully sent the message but not get a response within the timeout period. It is important to NOT treat this as a failure operation; the execution status is UNKNOWN and could have been a success.
*   All endpoints can possibly return an ERROR, the error payload is as follows:

> response example

```json
{
    "code": -1121,
    "msg": "Invalid symbol."
}
```

## General Interface Information

*   All requests are based on the Https protocol, and the `Content-Type` in the request header information needs to be uniformly set to: `'application/json'`
*   For the interface of the `GET` method, the parameters must be sent in the `query string`.
*   For the interface of the `POST` method, the parameters must be sent in the `request body`.
*   Parameters may be sent in any order.

## Request Limits

*   There will be a limited frequency description below each interface.
*   The `429` code will be returned when either rate limit is violated.
*   When the `429` code is returned, the caller should reduce the access frequency or stop accessing.

## Endpoint Security Type

*   Each endpoint has a security type that determines how you will interact with it.
*   API-keys are passed into the Rest API via the `X-CH-APIKEY` header.
*   API-keys and secret-keys are case-sensitive.
*   In the web user center, the permissions associated with the API key can be modified, such as for reading account information, sending trade instructions, or initiating withdrawal requests.

| Security Type | Description                                              |
| :-------------| :--------------------------------------------------------|
| NONE          | Endpoint can be accessed freely.                         |
| TRADE         | Endpoint requires sending a valid API-Key and signature. |
| USER\_DATA    | Endpoint requires sending a valid API-Key and signature. |
| USER\_STREAM  | Endpoint requires sending a valid API-Key.               |
| MARKET\_DATA  | Endpoint requires sending a valid API-Key.               |

## SIGNED (TRADE and USER\_DATA) Endpoint Security

*   When calling the `TRADE` or `USER_DATA` interface, the signature parameter should be passed in the `X-CH-SIGN` field in the HTTP header.
*   The signature uses the `HMAC SHA256` algorithm. The API-Secret corresponding to the API-KEY is used as the `HMAC SHA256` key.
*   The request header of `X-CH-SIGN` is based on timestamp + method + requestPath + body string  (+ means string connection) as the operation object.
*   The value of timestamp is the same as the `X-CH-TS` request header, method is the request method, and the letters are all uppercase: GET/POST.
*   requestPath is the request interface path. For example: `sapi/v1/order?symbol=ethusdt&orderID=111000111`
*   `body` is the string of the request body (post only).
*   The signature is not case-sensitive.

## Timing Security

*   The signature interface needs to pass the timestamp in the `X-CH-TS` field in the HTTP header, and its value should be the unix timestamp (ms) of the request sending time. E.g. 1528394129373
*   An additional parameter, recvWindow, may be sent to specify the number of milliseconds after timestamp the request is valid for. If recvWindow is not sent, it defaults to 5000.
*   In addition, if the server calculates that the client's timestamp is more than one second ‘in the future’ of the server’s time, it will also reject the request.
*   The logic is as follows:

```java
if (timestamp < (serverTime + 1000) && (serverTime - timestamp) <= recvWindow) {
  // process request
} else {
  // reject request
}
```

<aside class="notice">Networks may be unstable and unreliable, which can lead to requests taking varying amounts of time to reach the servers. With <code>recvWindow</code>, you can specify that the request must be processed within a certain number of milliseconds or be rejected by the server. It is recommended to use a small <code>recvWindow</code> of 5000 or less!</aside>

## Endpoint Example for POST /sapi/v1/order/test

Here is a step-by-step example of how to send a valid signed payload from the Linux command line using echo, openssl, and curl.

| Key       | Value                            |
| :---------| :--------------------------------|
| apiKey    | vmPUZE6mv9SD5V5e14y7Ju91duEh8A   |
| secretKey | 902ae3cb34ecee2779aa4d3e1d226686 |

| Parameter | Value   |
| :---------| :-------|
| symbol    | BTCUSDT |
| side      | BUY     |
| type      | LIMIT   |
| volume    | 1       |
| price     | 9300    |

> Signature Example

> body

```json
{"symbol":"BTCUSDT","price":"9300","volume":"1","side":"BUY","type":"LIMIT"}
```

> HMAC-SHA256 signature

```shell
echo -n "1588591856950POST/sapi/v1/order/test{\"symbol\":\"BTCUSDT\",\"price\":\"9300\",\"volume\":\"1\",\"side\":\"BUY\",\"type\":\"LIMIT\"}" | openssl dgst -sha256 -hmac "902ae3cb34ecee2779aa4d3e1d226686"
(stdin)= c50d0a74bb9427a9a03933d0eded03af9bf50115dc5b706882a4fcf07a26b761
```

> curl

```shell
curl -H "X-CH-APIKEY: c3b165fd5218cdd2c2874c65da468b1e" -H "X-CH-SIGN: c50d0a74bb9427a9a03933d0eded03af9bf50115dc5b706882a4fcf07a26b761
```