---
title: A Pythonic API wrapper - Introduction
tags: python, api
---

In order to programmatically extract data from a system, we need an API of some sort. It can be as simple as a flat file with well-defined column names dumped by an overnight batch, or as sophisticated as an on-demand web service with specific access methods.

And in order to leverage on an existing API, it is usually good practice to abstract away the HTTP implementation behind a custom class that we can reuse throughout our projects. This is exactly what we are going to do in this series of posts.

All the source code is available on [my GitHub](https://github.com/tlouarn/blog-pythonic-api-wrapper) for reference, and tests will be fully working thanks to mock requests.


# Introducing ETPS

In order to show practical examples, we will imagine a legacy trade booking system named ETPS (for Equity Trades Processing System). This system has a newly released REST API exposing the trades resource and we will be in charge of writing the Python wrapper around this API.

# A lightweight wrapper

We basically want our wrapper to:
- handle network requests
- handle pagination
- require a unique import
- be used via simple one-liners

On the other hand, we don’t want our wrapper to differ too much from the original API by, for example, mixing calls to different endpoints behind the scenes or converting the data types.

The end code should look like the below and our wrapper should let us interact with the API through pure Python objects (i.e. no HTTP and no JSON):

```python
import datetime as dt
from etps import ETPSWrapper, TradeToBookDTO

# Instantiation
etps = ETPSWrapper()

# Retrieve a trade knowing trade_id
trade_id = "XXX"
trade = etps.get_trade(trade_id)

# Book a new trade
trade = TradeToBookDTO(...)
etps.post_trade(trade)

# Delete a trade
etps.delete_trade(trade_id)

# Retrieve trades by criteria and loop through them
trades = etps.search_trades(trade_date=dt.date.today(), stock='AAPL US')
for trade in trades:
    print(f'quantity: {trade.quantity}')

```

# Issues and reflexions

During the process, we will face a number of issues ranging from the more benign to the more conceptual ones. We will do our best to come up with elegant and scalable solutions:
- how to handle different naming conventions
- how to handle the logging
- when to throw exceptions
- how to be more Pythonic

Last, we want this code to act as a template for future endpoints or wrappers altogether.