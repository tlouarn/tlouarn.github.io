---
title: API Design - Pagination
tags: api, python
---

Sometimes the results returned by the API need to be split into several blocks. Enters pagination.

Pagination needs to be handled both on the server side and on the client side.

In this article we will look at a `search_trades()` method. This method retrieves all the trades done in a given day and offers the option to filter the results by stock.

# Server side

We imagine a server endpoint working as follows:

Endpoint 

```curl
GET /trades




# Client side

Here again, we have a design decision to make: if no trade is found, should the wrapper return an empty list or should it raise an exception?

To answer this question, let’s think about the use cases.

Contrary to the get_trade() method where it is reasonable to assume that calling this method implies you know the trade with this trade_id exists, calling the search method may well return no result if it so happens that no trade was done that day/for that given stock. Therefore we choose to catch the 404 and return an empty list instead of raising an Exception.

If some trades are found, the API paginates the results. This is typically something our wrapper will abstract away from the client code by taking care of loading all the results from the API before returning a single list of TradeDTO objects.

Below is an implementation of the search_trades() method:

```python
# wrapper.py
class ETPSWrapper:

    # existing code

    def search_trades(self, trade_date: dt.date, stock: str = None) -> List[TradeDTO]:
        url = '/'.join([self.ROOT, self.TRADES_PATH])
        params = {'trade-date': trade_date.strftime('%Y-%m-%d')}
        
        if stock:
            params['stock'] = stock
        
        res = requests.get(url, params=params)
        
        if res.status_code == 200:
            trades_list = []
            res_dict = json.loads(res.text)
            for api_dict in res_dict['results']:
                dto_dict = TradeMapper().to_dto(api_dict)
                trades_list.append(TradeDTO(**dto_dict))
        
            # If more results, loop through the results
            while res_dict['total'] > res_dict['offset'] + res_dict['limit']:
                params['offset'] = res_dict['offset'] + res_dict['limit']
                params['limit'] = res_dict['limit']
                res = requests.get(url, params=params)
        
                res_dict = json.loads(res.text)
                for api_dict in res_dict['results']:
                    dto_dict = TradeMapper().to_dto(api_dict)
                    trades_list.append(TradeDTO(**dto_dict))
        
            return trades_list
        
        elif res.status_code == 404:
            return []
        
        else:
            raise HTTPError(f'HTTP Error {res.status_code}: {res.reason}')
```

Our wrapper class will contain four methods, each one representing an API functionality. 

```python
class Wrapper:
    ROOT = 'https://api.etps.world/'
    TRADES_PATH = 'trades'

    def __init__(self):
        pass
    
    def get_trade(self, trade_id: str) -> Trade:
        pass
    
    def post_trade(self, trade: Trade) -> str:
        pass
    
    def delete_trade(self, trade_id: str) -> None:
        pass
    
    def search_trades(self, trade_date: dt.date, stock: str = None) -> List[Trade]:
        pass
```

Note that we are using class variables to store the root URL and the resource path of the API since they are common to all instances of the class. Alternatively, we could have used an external configuration file, or a constant somewhere else in the code. But I personally prefer to attach these to the wrapper class itself for readability.

# The GET method

For the GET method of the API, we define a `get_trade()` method that returns a valid `Trade` object or raises an exception.

There is a design choice here: we assume that when requesting a trade by its `id`, the trade exists. So the normal behaviour of the method should be to return a valid `Trade` object by default and to raise an exception otherwise. 

To keep things simple, we simply pass-on the custom exceptions coming from the `requests` library. Alternatively, we could have renamed these exceptions into custom exceptions with, for instance, a `TradeNotFoundError` in case of a 404 and an `InfrastructureError` in case of another HTTP status code.

The method calls the API endpoint and deserializes the response into a valid `Trade` object using our custom TradeMapper:

```python
# wrapper.py
import datetime as dt
import json
import requests

from requests.exceptions import HTTPError
from typing import List

from model import Trade
from mapper import TradeMapper


class ETPSWrapper:
    ROOT = 'https://api.etps.world/'
    TRADES_PATH = 'trades'

    def __init__(self):
        pass

    def get_trade(self, trade_id: str) -> TradeDTO:
        url = '/'.join([self.ROOT, self.TRADES_PATH, trade_id])
        res = requests.get(url)

        if res.status_code == 200:
            api_dict = json.loads(res.text)
            dto_dict = TradeMapper().to_dto(api_dict)
            return TradeDTO(**dto_dict)

        else:
            raise HTTPError(f'HTTP Error {res.status_code}: {res.reason}')
```

The wrapper can be called as follows:

```cmd
>>> from etps import ETPSWrapper
>>> trade = ETPSWrapper().get_trade('d76f36f0-cbdc-4481-9a46-36837d1847da')
>>> trade
<__main__.Trade object at 0x0000020407926CA0>
```