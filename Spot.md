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

<h3 id="1-3">1-3.Place an order</h3>
