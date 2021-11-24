<div align="center">
  <img src="https://docs.kucoin.com/images/api-logo.svg">
</div>

[![Logo](https://img.shields.io/badge/kucoin--api--sop-yellowgreen?style=flat-square)](https://github.com/codewc/kucoin-api-sop)
[![GitHub stars](https://img.shields.io/github/stars/codewc/kucoin-api-sop.svg?label=Stars&style=flat-square)](https://github.com/codewc/kucoin-api-sop)
[![GitHub forks](https://img.shields.io/github/forks/codewc/kucoin-api-sop.svg?label=Fork&style=flat-square)](https://github.com/codewc/kucoin-api-sop)
[![GitHub issues](https://img.shields.io/github/issues/codewc/kucoin-api-sop.svg?label=Issue&style=flat-square)](https://github.com/codewc/kucoin-api-sop/issues)

# Spot

* [First steps](#a)

<h2 id="a">First steps</h2>

* [1-1.Create an API](#1-1)
* [1-2.Client Libraries](#1-2)
* [1-3.Place an order](#1-3)

<h3 id="1-1">1-1.Create an API</h3>

- Environment

|Environment   |Website    |REST API     |
|:-------:|:-------:|:-------|
| Sandbox|[https://sandbox.kucoin.com](#https://sandbox.kucoin.com)|https://openapi-sandbox.kucoin.com|
|Production |[https://www.kucoin.com](#https://www.kucoin.com)    |https://api.kucoin.com.|

Sandbox is the test environment, used for testing an API connection or web trading. It provides all the functionalities
of the live exchange. All funds and transactions there are simulated for testing purposes.

Log in to the web corresponding to the above environment, and then create the API according to the following guide
article

- How-to-Create-an-API

For how to create an API, please refer to the official special
introduction [How-to-Create-an-API](https://support.kucoin.plus/hc/en-us/articles/360015102174-How-to-Create-an-API)

<h3 id="1-2">1-2.Client Libraries</h3>
Client libraries can help you integrate with our API quickly.

|SDK       | github| install|
|:-------:|:------:|:------:|
|Java SDK  |https://github.com/Kucoin/kucoin-java-sdk|```<dependency><groupId>com.kucoin</groupId><artifactId>kucoin-java-sdk</artifactId><version>1.0.6</version></dependency>```|
|PHP SDK   |https://github.com/Kucoin/kucoin-php-sdk|```composer require "kucoin/kucoin-php-sdk:~1.1.0"```|
|Go SDK    |https://github.com/Kucoin/kucoin-go-sdk|```go get github.com/Kucoin/kucoin-go-sdk```|
|Python SDK|https://github.com/Kucoin/kucoin-python-sdk|```pip3 install kucoin-python```|
|Nodejs SDK|https://github.com/Kucoin/kucoin-node-sdk|```npm install kucoin-node-sdk``` or ```yarn add kucoin-node-sdk```|

CCXT is our authorized SDK provider and you may access the KuCoin API through CCXT. For more information, please
visit: https://ccxt.trade.

<h3 id="1-3">1-3 Place an order</h3>
For detailed instructions on placing an order, see API documentation https://docs.kucoin.com/#place-a-new-order.

Let's use the Python SDK to illustrate

<h4 id="1-3-1">1-3-1 Place Order Limit</h3>

```python
from kucoin.client import Trade

client = Trade(key='', secret='', passphrase='', is_sandbox=False)
order = client.create_limit_order(symbol='KCS-USDT', side='buy', size='0.01', price='0.001')
print(order)
```

There are some situations in this example, and the error indicates that the balance is insufficient

```json
{
  "code": "200004",
  "msg": "Balance insufficient!"
}
```

Let's first check the available balance of the account

<h4 id="1-3-2">1-3-2 Get a list of accounts</h3>

```python
from kucoin.client import User

client = User(key='', secret='', passphrase='', is_sandbox=False)
account_list = client.get_account_list(currency='USDT')
print(account_list)
```

- Return data sample:

```json
{
  "code": "200000",
  "data": [
    {
      "id": "60ff67b62abb0f0006ebb409",
      "currency": "USDT",
      "type": "main",
      "balance": "22.26577947",
      "available": "22.26577947",
      "holds": "0"
    },
    {
      "id": "61113016724a380006d06bf3",
      "currency": "USDT",
      "type": "margin",
      "balance": "0.6544733",
      "available": "0.6544733",
      "holds": "0"
    },
    {
      "id": "61025f527da40f0006db6c0c",
      "currency": "USDT",
      "type": "trade",
      "balance": "0.22227892",
      "available": "0.00000087",
      "holds": "0.22227804"
    }
  ]
}
```

we see that ```"available": "0.00000087"``` in trade account available balance,
so ```spendAmount = size * price = 0.01 * 0.001 = 0.00001```
.

It is understandable to prompt that the balance is insufficient.

The amount that needs to be transferred to the trading account
is ```amt = spendAmount - available = 0.00001 - 0.00000087 = 0.00000913```

```python
from kucoin.client import User

client = User(key='', secret='', passphrase='', is_sandbox=False)
transfer_ret = client.inner_transfer(currency='USDT', from_payer='main', to_payee='trade', amount='0.00000913')
print(transfer_ret)
```

This endpoint [POST /api/v2/accounts/inner-transfer](#https://docs.kucoin.com/#inner-transfer) returns that our transfer
was successful

```json
{
  "code": "200000",
  "data": {
    "orderId": "619cfd8ba93ed20001e6016f"
  }
}
```

Let's use the endpoint [GET /api/v1/accounts](#https://docs.kucoin.com/#list-accounts) to check trade account balance

```json
{
  "code": "200000",
  "data": [
    {
      "id": "60ff67b62abb0f0006ebb409",
      "currency": "USDT",
      "type": "main",
      "balance": "22.26577034",
      "available": "22.26577034",
      "holds": "0"
    },
    {
      "id": "61113016724a380006d06bf3",
      "currency": "USDT",
      "type": "margin",
      "balance": "0.6544733",
      "available": "0.6544733",
      "holds": "0"
    },
    {
      "id": "61025f527da40f0006db6c0c",
      "currency": "USDT",
      "type": "trade",
      "balance": "0.22228805",
      "available": "0.00001",
      "holds": "0.22227804"
    }
  ]
}
```

According to the above example, try again and place an order

```json
{
  "code": "200004",
  "msg": "Balance insufficient!"
}
```

When the order is placed above, it still prompts that the balance is insufficient, that is because we forgot to
calculate the trade fee. Let's check our own trade fee

<h4 id="1-3-3">1-3-3 Actual fee rate of the trading pair</h3>

```python
from kucoin.client import User

client = User(key='', secret='', passphrase='', is_sandbox=False)
ret = client.get_actual_fee(symbols='KCS-USDT')
print(ret)
```

```json
{
  "code": "200000",
  "data": [
    {
      "symbol": "KCS-USDT",
      "takerFeeRate": "0.002",
      "makerFeeRate": "0.002"
    }
  ]
}
```

```python
from kucoin.client import Market

client = Market(key='', secret='', passphrase='', is_sandbox=False)
ret = client.get_symbol_list()
print(ret)
```

```json
{
  "code": "200000",
  "data": [
    ...
    {
      "symbol": "KCS-USDT",
      "name": "KCS-USDT",
      "baseCurrency": "KCS",
      "quoteCurrency": "USDT",
      "feeCurrency": "USDT",
      "market": "USDS",
      "baseMinSize": "0.01",
      "quoteMinSize": "0.05",
      "baseMaxSize": "10000000000",
      "quoteMaxSize": "99999999",
      "baseIncrement": "0.0001",
      "quoteIncrement": "0.0001",
      "priceIncrement": "0.001",
      "priceLimitRate": "0.1",
      "isMarginEnabled": true,
      "enableTrading": true
    },
    ...
  ]
}
```

```actual_fee= takerFeeRate * spendAmount = 0.002 * 0.00001 = 0.00000002 ```

```python
from kucoin.client import User

client = User(key='', secret='', passphrase='', is_sandbox=False)
transfer_ret = client.inner_transfer(currency='USDT', from_payer='main', to_payee='trade', amount='0.00000002')

account_list = client.get_account_list(currency='USDT')
print(account_list)
```

```json
{
  "code": "200000",
  "data": [
    {
      "id": "60ff67b62abb0f0006ebb409",
      "currency": "USDT",
      "type": "main",
      "balance": "22.26577032",
      "available": "22.26577032",
      "holds": "0"
    },
    {
      "id": "61113016724a380006d06bf3",
      "currency": "USDT",
      "type": "margin",
      "balance": "0.6544733",
      "available": "0.6544733",
      "holds": "0"
    },
    {
      "id": "61025f527da40f0006db6c0c",
      "currency": "USDT",
      "type": "trade",
      "balance": "0.22228807",
      "available": "0.00001002",
      "holds": "0.22227804"
    }
  ]
}
```

Let's place an order again.

```python
from kucoin.client import Trade

client = Trade(key='', secret='', passphrase='', is_sandbox=False)
order = client.create_limit_order(symbol='KCS-USDT', side='buy', size='0.01', price='0.001')
print(order)
```

```json
{
  "code": "200000",
  "data": {
    "orderId": "619e52769419150001003840"
  }
}
```

So far we have successfully placed a limit order, but obviously we can’t make a deal.