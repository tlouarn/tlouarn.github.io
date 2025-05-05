---
title: Building a live order book using Python
subtitle: How to translate a WebSocket feed into a responsive UI
tags: python ui
---

This article examines design considerations for implementing a lightweight live order book GUI in Python using PySide6. PySide is the official Python port of the popular Qt framework. For the data source, we will use a free Bitcoin order book feed provided by Bitfinex.

# Bitfinex API v2

WebSockets is a communication protocol that operates over a single TCP connection and allows real-time data exchange between a client and a server. Unlike traditional HTTP requests, which are one-way and require constant polling for updates, WebSockets provide a persistent connection where both the client and the server can send data at any time.

In Python, the most commonly used package for WebSocket communication is **websockets**. This package is an asyncio-based library that supports both client and server WebSocket implementations. In this article we will use it to build a client.

The Bitfinex API v2 gives access to a live Bitcoin order book via a WebSocket connection. The documentation of the **book** channel can be found [here](https://docs.bitfinex.com/reference/ws-public-books). Once connected to the WebSocket API, the client receives messages in the following order: first a snapshot of the current order book, then some updates to the order book (changes in prices and quantities of bids and asks). Once every 15 seconds, the client receives a heartbeat message to confirm that the connection is still open.

It is therefore the client's responsibility to maintain an internal representation of the order book and to update it accordingly with the messages received.

# Design decisions

I have broken down the program into several key components:
* `BitfinexHandler`: interacts with the Bitfinex WebSockets API
* `OrderBookModel`: maintains an up-to-date internal representation of the order book 
* `MainWindow`: ties the handler and the model to the UI

First thing first, we don't want our UI to freeze while we are handling market data, therefore we will use asynchronous programming with the **asyncio** framework as well as Qt's own **signals** mechanism. We showcase the use of signals by updating the status bar of the main window whenever the connection is (asynchronously) opened or closed by the handler.

`BitfinexHandler` is our primary client for the Bitfinex Websockets API. We implement it as a subclass of `QObject` in order to be able to emit signals. When it opens or close a connection, `BitfinexHandler` emits a specific signal that can be captured by the main window. When listening to updates of the **book** channel for Bitcoin (tBTCUSD), it calls back a custom `handle_message` method from `MainWindow`.

We could choose to maintain our data model directly as it is represented (i.e. a 5x4 table), or we could maintain it in a dedicated structure and implement standard methods by which our view can represent it as a table. Since it is generally good practice to  separate data from UI, we choose the second option. To do so, instead of relying on a `QTableWidget` component, we choose to use a custom `QTableView` together with a custom `QAbstractTableModel`. This way, instead of directly updating the table cells, we update an internal model that is then translated by the view into a 5x4 table.

The main window controls the show. It builds the interface using a button, a table and a status bar. It initiates our client using its own `handle_message` method as a callback, meaning this method is called everytime the client receives a message. The message can either be informative (like a heartbeat) or market-related. In the latter case, the message would either be a 50-long list (25 limits on either side of bids and asks) or a 1-long list (update of a single limit). The `handle_message` method passes on the messages received from the handler into our order book model and notifies the view that the model has been updated.

Finally, we use **qasync** in order to integrate Qt's own event loop with asyncio's separate event loop. 

# UI considerations

In terms of UI, we want the user to start and stop the connection with buttons. A button should always display an action rather than a status. We could imagine two buttons: **Start** and **Stop**. Since we do not want the user to start a system already started nor to stop a system already stopped, only one of these two buttons should be enabled at all times. In fact, we could probably use a single button which action changes depending on the state of the system. This is what we implemented using our own `ToggleButton` widget. One last subtlety: while the system is connecting or disconnecting, we temporarily disable the button and enable it again once the system is either fully connected or fully disconnected. This avoids clicking twice while the previous operation is still running.

Qt provides a default design for its components but we want some degree of customization linked to the type of application we are building and the type of data we are handling. Among the questions to be asked are: What font should be used? What font size is optimal? How much spacing inside the cells? Should we right-align all numbers? How many decimals should be displayed? How to remove double borders (cell borders and table borders)? Thankfully, the Qt framework is customizable via its own CSS so we have provided our custom components with their own style sheets. In order to not make `main_window.py` too big, we have placed the button and the table classes in their own files, still in the `views` folder.

# The result

The result is a responsive, asynchronous and (hopefully) good-looking UI that shows a live Bitcoin order book. The user can start and stop the feed with a toggle button. Most importantly, the interface remains fully responsive in any possible state of the WebSocket connection. This allows for instance the user to resize the window while, in the background, the client is connecting to the feed or the view is updating the table.

![Live order book using Python](/assets/images/tlouarn-building-a-live-order-book-using-python.gif)

The final project can be found on my [GitHub](https://github.com/tlouarn/order-book/).




