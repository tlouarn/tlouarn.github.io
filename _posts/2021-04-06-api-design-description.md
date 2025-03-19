---
title: API Design - Description
tags: api python
---

API stands for **A**pplication **P**rogramming **I**nterface. It's basically words on paper describing how systems should interact together. The API is then implemented with code that tries to behave like described in the Interface as best as possible.

There is no standard for APIs but there is a dominant style named **REST**. REST specifies architectural constraints around the usage of HTTP verbs (`GET`, `POST`, `PUT`, `DELETE`). Nowadays, most APIs follow the REST style and transfer data using JSON.

APIs are usually versioned. This way, changes to request or response format can be integrated to a new version of the API so that the existing API consumers do not break.

# REST APIs

If we had to build a REST API around a trade booking system, we could use the HTTP verbs as follows:

| Verb |  Action | 
| --- |  --- |
| `GET` | Retrieve trade details for a given trade id |
| `POST` | Book a new trade |
| `DELETE` | Delete a trade | 
| `PATCH` | Update an existing trade |

# The JSON format

JSON defines two data structures: objects and arrays.
JSON defines only two data structures: objects and arrays. An object is a set of name-value pairs, and an array is a list of values. JSON defines seven value types: string, number, object, array, true, false, and null.

# How to describe an endpoint

An endpoint description should contains:
* the endpoint url
* the query parameters
* the possible status codes
* the response format
* an example

In the following example, we assume a REST API endpoing returning the details of a specific trade.

## Endpoint url:

```curl
GET /trades/:id
```

## Path parameters:

| Name | Type | Description |
| --- | --- | --- |
|id  | string | Unique identifier of the trade to request. Format: UUID v4. Required |

## Status codes:

| Name | Description |
| --- | --- |
|200 | OK |
|401 | Unauthenticated|
|403 | Forbidden|
|404 | Not found|
|500 | Internal server error|

## Response fields:

| Name | Type | Description |
| --- | --- | --- |
|id | string | Unique trade identifier. Format: UUID v4|
|way | string | Client way. Possibles values: “BUY” or “SELL”|
|stock | string | Stock name|
|quantity |	number | Number of stocks traded|
|price | string | Price at which the stocks traded. Contains up to 8 decimals|
|currency |	string | Name of the currency. Format: ISO 4217|
|tradeDate | string | Trade Date. Format: YYYY-MM-DD|
|valueDate | string | Value Date. Format: YYYY-MM-DD|

You may wonder why we choose to return the trade quantity as a `number` but the price as a `string`. The reason is simple: a quantity of stocks has to be an integer, whereas the price can contain decimals. Forcing a `number` type for decimals would open the door to floating-point arithmetics on the client side.

## Query example:

```curl
GET /trades/d76f36f0-cbdc-4481-9a46-36837d1847da
```
Response body example (JSON):

```json
{
    "id": "d76f36f0-cbdc-4481-9a46-36837d1847da",
    "way": "BUY",
    "stock": "AAPL US",
    "quantity": 54123,
    "price": "127.79",
    "currency": "USD",
    "tradeDate": "2021-03-01",
    "valueDate": "2021-03-03"
}
```