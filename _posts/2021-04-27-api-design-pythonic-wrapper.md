---
title: API Design - Python wrapper
tags: api python
---

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