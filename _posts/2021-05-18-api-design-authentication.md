---
title: API Design - Authentication
tags: api, python
---

In this post we will go through the OAuth 2 protocol and see how we can implement authentication in a Python wrapper. OAuth 2 defines several types of authorization flows, but for simplicity we will focus on the **Client Credentials** flow.

# Client Credentials

We assume a User wants to programmatically access a protected Resource via a Client.

* Without OAuth, the Resource server would need to handle the User authentication and the User would likely have to send its username and password with each request
* With OAuth, the process is decomposed between an Auth server and a Resource server

Some time before, the User asks the Resource owner (for instance, the team who developped the API) for access to specific scopes. We can assume the creators of ETPS API have created a trades.read scope protecting the GET method and a trades.write scope protecting the POST and DELETE methods. This is a one-off process.

![Oauth2 Diagram 1](/assets/images/api-design-authentication-oauth-1.png)

In the diagrams I put a GET /resource just for clarity. It does not have to be a GET, it can be any REST verb method depending on the API design.

From now on, for each request, the client will need to authenticate itself directly with the Auth server by communicating the username, the password and the desired scope(s). The Auth server is aware of the grant and validates the access by creating an Access token with limited duration.

The User then queries the Resource along with the Access token only (usually in the header of the request), the Resource server checks with the Auth server that the token is valid (since the Auth server just emitted the token, it can validate it). Upon validation, the Resource server sends back the protected Resource to the User (in our case, the JSON response to a GET /trades request for instance).

![Oauth2 Diagram 2](/assets/images/api-design-authentication-oauth-2.png)

At some point the Access token will cease to be valid. It can be as short-lived as a few minutes. At this point, when the Resource server will ask the Auth server to validate the token, it will be rejected. The ETPS API will send back a 403 and the User will know it has to request a new token by authenticating again.

Security-wise, the standard is to encrypt all requests in HTTPS. And even if the Access token is sniffed, it only has a limited lifespan.

# How to handle authentication

As far as our ETPS wrapper is concerned, we have two alternatives:

- we can handle the authentication within the wrapper itself
- we can delegate the authentication to an external class responsible for getting the tokens

The choice will depend on the overall architecture: is the Auth server specific to our API or is it a generic Auth server shared by several APIs?

For the former, we will likely bind the authentication code to our wrapper, but for the latter we will likely create a specific class for it that we can inject in our wrapper.

In our example we will assume that a generic Auth server handles the generation and validation of Access tokens, so we will delegate the authentication to a separate class.

# Python implementation

Coding the Auth class if out of the scope of our series of posts, but below is what a simple implementation could look like.

We instantiate the class with our client_id, client_secret and desired scopes. When we need to refresh the token, we simply call get_token() which will ask the TOKEN_URL for a new token given our credentials.

```python
import requests


class Auth:
    TOKEN_URL = 'https://auth/token'

    def __init__(client_id: str, client_secret: str, scope_list: List[str]):
        self.client_id = client_id
        self.client_secret = client_secret
        self.scope_list = scope_list
    
    def get_token() -> str:
        url = self.TOKEN_URL
        params = {
            'type': 'client_credentials',
            'scope': ' '.join(self.scope_list),
            'client-id: self.client_id,
            'password': self.client_secret
        }
        res = requests.get(url, params=params)
        
        # we assume the response from the Auth server is a JSON dictionary of format
        # {'access_token': str, 'token_type': 'Bearer', 'expiresIn': number}

        if res.status == 200:
            res_dict = json.loads(res.content)
            return res_dict['access_token']

```

To make a better Auth class, we also need to properly handle errors and raise Exceptions in case of invalid combination of client_id, client_secret and scope. We could also implement a token validity mechanism based on the expiresIn response from the Auth server in order to only ask for a new token when we know the current one is invalid, which reduces the charge on the Auth server.

As far as our API wrapper itself is concerned, all we need to do is to get a valid token before we request a resource.

A standard practice is to inject the Auth class into the wrapper at instantiation, then query it before each request.

For instance, we can enrich our get_trades() method as follows, leveraging on self.auth:

```python
# wrapper.py

class ETPSWrapper:
    
    def __init__(self, auth: Auth):
        # existing code
        self.auth = auth

    def get_trade(self, trade_id: str) -> TradeDTO:
        url = '/'.join([self.ROOT, self.TRADES_PATH, trade_id])
        token = self.auth.get_token()
        headers = {'Authorization': f'Bearer {token}'}
        res = requests.get(url, headers=headers)
        self.logger.debug(response_formatter(res))
```

Finally, the code calling our wrapper would need to import and instantiate the Auth class as follows:

```python
import datetime as dt
from etps import ETPSWrapper, TradeToBookDTO
from generic_auth import Auth

# Authorization
auth = Auth('0bfc2fff-2d73-4637-9265-a71ad3258e5a', 'x_f7xqrqbceh6ksg2j52twn9pk1lt6', ['trades.read'])

# Instantiation
etps = ETPSWrapper(auth=auth)

# Retrieve a trade knowing trade_id
trade_id = ...
trade = etps.get_trade(trade_id)
```

Obviously, the final application using our library would not expose client_id and client_secret in the code. These credentials would likely be safely stored in a Vault.


Using Environmnent variables.


Last but not least, it's best practice to keep the keys as environment variables. Do not include them in your code. The classic library for this is `python-dotenv` which can read key-value pairs from a `.env` file located at the root of the project.

