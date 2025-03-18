---
Title: A Pythonic API wrapper - API description
Tags: api, python
---

# REST assured, our API will be clean

Wiser people than me once said:

| An API is forever.
| – Somewhere on the web

API stands for Application Programming Interface. And this is really what it is: an Interface, i.e. words. It’s a way of describing on paper how systems should interact together. The API is then implemented with servers and server-side code that tries as best as possible to behave like described in the Interface.

There is no standard as such for APIs, but there is a dominant style: REST APIs. REST specifies architectural constraints around the usage of HTTP verbs (GET, POST, PUT, DELETE etc.). Nowadays, most APIs follow the REST style, coupled with JSON as the data transfer format, so this is how we will imagine our ETPS API for this series of posts.

As of version 1, we assume the API is contains a single trades endpoint which can be queried in the following way:

- a GET by :id to retrieve information about a specific trade
- a POST to book a new trade
- a DELETE to delete a trade from the system
- a GET with queried parameters which returns a list of trades matching the criteria (if any)

This post will go through the endpoint URL, the parameters, the status codes and the response fields and will be used as a reference for the upcoming posts of the series. We will try to be as exhaustive as possible in describing the behaviour of the API and the data fields.

