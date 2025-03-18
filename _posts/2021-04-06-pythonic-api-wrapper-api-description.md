---
title: A Pythonic API wrapper - API description
tags: api, python
---

# REST assured, our API will be clean

> An API is forever.
> – Somewhere on the web

API stands for Application Programming Interface. And this is really what it is: an Interface, i.e. words. It’s a way of describing on paper how systems should interact together. The API is then implemented with servers and server-side code that tries as best as possible to behave like described in the Interface.

There is no standard for APIs but there is a dominant style - REST - which specifies architectural constraints around the usage of HTTP verbs (GET, POST, PUT, DELETE). Nowadays, most APIs follow the REST style and transfer data in the JSON format.

As of version 1, we assume the API is contains a single trades endpoint which can be queried in the following way:

- a GET by :id to retrieve information about a specific trade
- a POST to book a new trade
- a DELETE to delete a trade from the system
- a GET with queried parameters which returns a list of trades matching the criteria (if any)

This post will go through the endpoint URL, the parameters, the status codes and the response fields and will be used as a reference for the upcoming posts of the series. We will try to be as exhaustive as possible in describing the behaviour of the API and the data fields.

# API description

This post shows how an API should be described.

## GET method - Retrieve a trade by id

The GET method is pretty straightforward: knowing the `id` of a given trade, it returns the trade information as a JSON string.

Endpoint url:

```curl
GET /trades/:id
```

Path parameters:


| Name | Type | Description |
| --- | --- | --- |
|id  | string | Unique identifier of the trade to request. Format: UUID v4. Required |

Status codes:

| Name | Description |
| --- | --- |
|200 | OK |
|401 | Unauthenticated|
|403 | Forbidden|
|404| Not found|
|500| Internal server error|

Response fields:

| Name | Type | Description |
| --- | --- | --- |
|id| 	string |	Unique trade identifier. Format: UUID v4|
|way| 	string 	|Client way. Possibles values: “BUY” or “SELL”|
|stock| 	string |	Stock name|
|quantity |	number |	Number of stocks traded|
|price| 	string |	Price at which the stocks traded. Contains up to 8 decimals|
|currency |	string |	Name of the currency. Format: ISO 4217|
|tradeDate |	string |	Trade Date. Format: YYYY-MM-DD|
|valueDate |	string |	Value Date. Format: YYYY-MM-DD|