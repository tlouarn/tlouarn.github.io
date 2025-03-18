---
title: API Design - Data Transfer Objects
tags: api, python
---

Python offers a wide choice of data structures. We could for instance have our wrapper return the trade information from the API as a `dictionary` or a `namedtuple`. This approach works for simple scripts, but for enterprise grade applications we prefer to build custom objects.

Custom objects are an efficient way to **communicate expectations** to the readers of the code. With dictionaries for example, we can’t specify which fields are required and which are not, we can’t assign default values, we can’t perform data validation and we can’t type-hint. On the other hand, a custom object gives us the possibility to do just all that.

# Introducing the DTO

The main resource of our API being the `trade`, we will create a Python object that will have a one-to-one mapping with the fields from the API’s JSON object.

At this stage, we need to think about the data types of our Python object. If we take the `tradeDate` for instance, we see that the API encodes it as a %Y-%m-%d formatted string. In our Python script, we are likely to convert string to a proper datetime.date sooner or later.

The question is: should we do it inside our wrapper or should we do it outside of our wrapper?

There is no clear answer to this question since it can vary with each project. But I would recommend keeping the data as communicated by the API: sure, we don’t want the wrapper to return plain dictionaries, but we don’t necessarily want it to try and convert the values to proper Domain objects neither.

Turns out there is already a name for what we are looking to build: DTO (Data Transfer Object). DTOs are well-known in the Java and C# worlds but are rarely seeen in Python codebases, or at least are not clearly defined as such.

There are multiple usages to a DTO:

* reduce expensive remote calls: in the definition given by Martin Fowler, DTOs are here to reduce the number of remote calls by grouping the requested data in a single call
* remove sensitive data from the Domain object: in our example, the actual trade resource on the server-side may contain other fields that should not be exposed via the API, so what we see coming from the API is likely to be a minimal representation of the actual Domain trade object

# DTO implementationin Python

Let’s implement our `Trade` class. You will note that:

* type hints are always welcome
* we define a special `__slots__` attribute which brings clarity and improves performance
* we added a `to_dict()` method to convert the object to a Dictionary (thanks to the usage of the `__slots__` magic declaration, the code is reduced to a one-liner)
* we do not need a `from_dict()` method since we can already construct the object from a Dictionary using unpacking.
* since `id()` is a built-in Python function, we renamed the `id` attribute to `id_` as per [PEP 8](https://peps.python.org/pep-0008/#descriptive-naming-styles)

```python
# model.py
import datetime as dt
from decimal import Decimal
from typing import Dict


class Trade:
    __slots__ = ['id_', 'way', 'stock', 'quantity', 'price', 'currency', 'trade_date', 'value_date']

    def __init__(
        self,
        id_: str,
        way: str,
        stock: str,
        quantity: int,
        price: str,
        currency: str,
        trade_date: str,
        value_date: str
    ) -> None:

        self.id_ = id_
        self.way = way
        self.stock = stock
        self.quantity = quantity
        self.price = Decimal(price)
        self.currency = currency
        self.trade_date = dt.strptime(trade_date, "%Y-%m-%d").date()
        self.value_date = dt.strptime(value_date, "%Y-%m-%d").date()

    def to_dict(self) -> Dict:
        return {attr: getattr(self, attr) for attr in self.__slots__}

```

Assuming we have the following dictionary ready to be fed to our object:

```python
trade_dict = {
    'id_': 'd76f36f0-cbdc-4481-9a46-36837d1847da',
    'way': 'BUY',
    'stock': 'AAPL US',
    'quantity': 54123,
    'price': '127.79',
    'currency': 'USD',
    'trade_date': '2021-03-01',
    'value_date': '2021-03-03'
}
```

Let's test our `Trade object`:

```python
>>> trade = TradeDTO(**trade_dict)
>>> trade
<__main__.TradeDTO object at 0x000001D8EA745160>
>>> trade.to_dict()
{'id_': 'd76f36f0-cbdc-4481-9a46-36837d1847da', 'way': 'BUY', 'stock': 'AAPL US', 'quantity': 54123, 'price': '127.79', 'currency': 'USD', 'trade_date': '2021-03-01', 'value_date': '2021-03-03'}
>>> trade.to_dict() == trade_dict
True

```