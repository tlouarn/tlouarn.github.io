---
title: Getting started with tweepy
tags: python, api
---

In this article I will get you setup and ready to go with the Twitter API using Python in 8 steps:

1. Create a Twitter account (if you don’t already have one)
2. Apply for a developer account
3. A word about credentials and versioning
4. Create your project and your app
5. Generate consumer keys
6. Generate access tokens
7. Install tweepy
8. Post your first automated tweet

# Step 1 - Create a Twitter account

If you don’t already have one, or if you don’t want to develop on your existing account, you can create a new Twitter account.

Bear in mind:
- You can share the same phone number with up to 10 accounts.
- If you have a Gmail address, you can reuse the same address over multiple accounts by adding “dots” in your email address (see [here](https://support.google.com/mail/answer/7436150?hl=en-GB)).

# Step 2 - Apply for a Developer account

Developer accounts are regular accounts on steroids.

Go to https://developer.twitter.com/en/apply-for-access and follow the steps to create a developer account attached to your main Twitter account.

You will have to answer a few questions such as the purpose of the applications you intend to develop as well as the type of analytics you are planning to do with Twitter data. Unless you are into really weird stuff, your developer account should be immediately validated.

# Step 3 - A word about credentials and versioning

The documentation can be confusing, so here is a quick summary regarding credentials and versioning.

Twitter uses an authentication protocol called OAuth (in fact, Twitter even helped to create it, but that’s another story).

OAuth exists in different versions:
- OAuth 1.0a: you will use this protocol to act on behalf of a Twitter account via the API (e.g. post tweets, like tweets, DM people etc.)
- OAuth 2.0: you may have the option of using this protocol for the application’s own read-only actions (e.g. lookup a user, load tweets etc.)

Furthermore, the API itself exists in two versions:
- the legacy API v1
- the newly baked API v2 which is only being released bit by bit.

Not all endpoints have been migrated yet, far from it (you can follow the changelog [here](https://docs.x.com/changelog)).

To summarize, depending on the endpoint you wish to call, you may use the API v1 or the API v2, and depending on the nature of the operation you wish to execute, you will be using OAuth 1.0a or you may have the option of using OAuth 2.0.

# Step 4 - Create your project and your app

The notion of “project” was introduced with Twitter API v2: in order to access endpoints from API v2, your app needs to be part of a project.

The rules are:
- Only 1 Standard Project per developer account (you can apply for an Academic Research Project as well)
- Only 1 app per project (Twitter is planning on allowing several apps per project)
- The app name needs to be unique

Once your developer account has been validated, you should see the prompts for a project name and an app name (remember: your app name needs to be unique). Let your creativity speak!

# Step 5 - Generate consumer keys

Right after you chose your app name, you will be presented with your keys and tokens like below:

[Keys and tokens](/assets/images/getting-started-with-tweepy-keys-and-tokens.png)

Please save them somewhere secure, and once that’s done, save them again. This is the only time you will ever see them. The API Key and API Secret Key (also known as Consumer Keys) will authenticate the app itself with OAuth 1.0a. The Bearer Token can be used with OAuth 2.0 but we will not use it in this tutorial.

# Step 6 - Generate an access token

Just before we generate the access tokens, we need to give Read and Write access to our app in order to be able to post tweets.

From the main Settings page, find the App permissions section and click on Edit:

[Edit app permissions](/assets/images/getting-started-with-tweepy-edit-app-permissions.png)

Then select “Read and Write”:

[Change pp permissions to Read and Write](/assets/images/getting-started-with-tweepy-app-permissions-read-and-write.png)

Once this is done, go back to the Keys and tokens page and generate your Access Token and Secret. You know the drill: save them somewhere secure, and once this is done, save them again. Since we gave Read and Write permissions to our app prior to generating the Access Token and Secret, we will be able to post tweets using the API.


# Step 7 - Install Tweepy

From the Terminal, install tweepy using pip.

```
pip install tweepy
```

# Step 8: Post your first automated tweet

Below is the Python code to post a tweet using tweepy.

Quick reminder on how to fill the XXXXXXXXXX:

    The consumer key and the consumer secret were created at the beginning of the tutorial: they will allow your script to be authenticated by the platform.

    The access token and access token secret were created in a second step after we changed the app’s permissions: they will allow your script to tweet on behalf of your main account.

```python
import tweepy

consumer_key = 'XXXXXXXXXX'
consumer_secret = 'XXXXXXXXXX'
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)

access_token = 'XXXXXXXXXX'
access_token_secret = 'XXXXXXXXXX'
auth.set_access_token(access_token, access_token_secret)

api = tweepy.API(auth)
res = api.update_status('Hello World!')
```

Run the code, then head over to your main Twitter account and voilà !