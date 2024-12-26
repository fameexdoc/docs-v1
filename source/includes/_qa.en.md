# FAQ

## What is the maximum difference between the timestamp parameter of the request interface and the arrival time of the server?

When the server receives the request, it will judge the timestamp in the request. If it is sent before 5000 milliseconds, the request will be considered invalid. This time window value can be customized by sending the optional parameter `recvWindow`.

## The request header "X-CH-TS" cannot be empty. How to solve it?

First, it is recommended to print out the `X-CH-TS`, and check whether the `X-CH-TS` is empty when there is an exception, and it is recommended to optimize your code and judge whether the `X-CH-TS` is empty before each request.

## Why does authentication always return invalid signatures?

You can print out the request header information and the string before the signature, with the following points:

* Compare your request header with the following request header example one by one.

```json
Content-Type: application/json
X-CH-APIKEY: 44c541a1-****-****-****-10fe390df2
X-CH-SIGN: ssseLeefrffraoEQ3yI9qEtI1CZ82ikZ4xSG5Kj8gnl3uw=
X-CH-TS: 1574327555669
```

* Confirm whether the API-key is configured correctly in the program.
* Ensure the string before signing follows the standard format, with all elements in the correct order. You can use the example below to compare with your string before signing:

> GET example

```http
1588591856950GET/sapi/v1/account
```

> POST example

```http
1588591856950POST/sapi/v1/order/test{"symbol":"BTCUSDT","price":"9300","volume":"1","side":"BUY","type":"LIMIT"}
```

## The calling interface prompts** ILLEGAL_CONTENT_TYPE (-1017). What is the reason?

We recommend attaching Content-Type to all request headers and setting it to application/json.

## Is there a limit to the frequency of API calls per second?

There are restrictions. For details, see the access frequency restrictions for each interface in the document.

## What is the limit on API access frequency？

Personal data is restricted according to **API-key**, and public data is restricted according to **ip**. It should be noted that if the user requests public data and passes in valid personal information, it is restricted according to **API-key**.

## What is the cause of HTTP status code 429?

The request interface exceeds the access frequency limit. It is recommended to reduce the access frequency.

## Will the IP be blocked if the API call interface reports that the access frequency is exceeded? How long is it blocked?

Normally not. If it is blocked, please reduce the frequency of access.

## Why is WebSocker disconnected?

* Without adding a heartbeat, the WebSocket connection requires the client to return to pong to ensure the stability of the connection.
* The pong message sent by the client is caused by network reasons, but the server does not receive it, or other network reasons may also cause automatic disconnection.
* It is recommended that you have a robust WebSocket disconnect and reconnect mechanism to ensure automatic reconnection if the heartbeat (ping/pong) connection is unexpectedly lost.

## Why does the user request interface report an error Time Out?

The network cannot connect to the server. It is recommended that you check whether the network is smooth.

## How to get all the currency pairs on the platform?

You obtain them via coin's `/sapi/v1/symbols` interface.

## Is there a limit on the number of batch orders/cancellations?

Yes, the batch interface will limit 10 orders.

## What is newClientOrderId and what does it do?

* newClientOrderId is your customized order number, which can be used to identify your order. After the order is placed, you can make newClientOrderId call the "Order Information" interface to view the order status.
* You need to ensure that this ID is not repeated, and we will not prompt for re-relocation. If there is a repetition, you can only cancel or query the latest piece of data when canceling and querying the order.

## How to get the latest transaction price?

You can get ticker information, last is the latest transaction price.

## Will there be a negative growth in the 24-hour trading volume in the ticker interface?

Yes, it is possible. Since the 24-hour trading volume is based on a rolling 24-hour window ( (the size of the translation window is 24 hours), the accumulated volume in the next window may be smaller than the previous one, resulting in negative growth.