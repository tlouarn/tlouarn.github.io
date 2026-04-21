---
title: Serverless architectures (Part 1) - SPA
subtitle: How to design a serverless architecture on AWS to support a SPA
tags: aws architecture
---

# Introduction

A **Single Page Application** (SPA) is based on a unique document that dynamically loads and renders data via AJAX requests. First, the user downloads the HTML document with all associated CSS and JavaScript. Once loaded, JavaScript takes over and handles both data requests to the server and dynamic display on the web page. A SPA is therefore made of **states** rather than **pages**. Several JavaScript frameworks have popularized this architecture, such as [Angular](https://angular.io/), [React](https://reactjs.org) and [Vue](https://vuejs.org).

# Implementation

Here is how to implement this solution on AWS:

![Diagram 1: single-page web application on AWS](/assets/images/tlouarn-serverless-architectures-part-1-spa.svg)

graph TD
    A[User] -->|Request https://example.com| B[Route 53]
    B -->|DNS Resolution| C[S3 Bucket]
    C -->|Return HTML/CSS/JS| A
    A -->|AJAX Request| D[API Gateway]
    D -->|Route Request| E[Lambda]
    E -->|Read/Write Data| F[DynamoDB]
    F -->|Return Data| E
    E -->|Return JSON| D
    D -->|Return JSON| A


* The HTML document is stored on a public S3 bucket
* The link between a custom domain name and the S3 bucket is done directly in Route 53 via a hosted zone
* When the user GET the address, AWS returns a single HTML document with associated CSS and JS
* Once opened in the browser, JavaScript kicks in and takes over from the client side
* Subsequent AJAX requests originating from the SPA call specific endpoints deployed via API Gateway
* Behind each endpoint, lambdas handle the requests and prepare the responses (e.g. getting data from a DynamoDB)
* Once the data is received on the client side, JavaScript renders it
* The page itself does not reload

# Pros & Cons

Pros | Cons
--- | ---
**Reusable API endpoints**: no need to develop specific endpoints to serve HTML documents, as it is possible to use and reuse JSON data from existing API endpoints. | **No deep-linking**: because all the content is generated on-the-fly after the initial HTML document is loaded, there is no unique link pointing to specific resources, so the "pages" you visit will not be kept as such in your browser history and you won't be able to go back and forth[^1].
**Flexible development**: although the initial learning curve is steep, existing SPA web frameworks make it easy to code, deploy and manage complex applications in a relatively short time. | **Weak SEO**: bots crawling and indexing the web typically struggle with SPAs and the JavaScript overhead necessary to render the pages. Also, it's harder to dedicate specific keywords to specific pages and to analyze visitors traffic[^2].
**Reduced bandwidth usage**: even if the initial page load is longer, subsequent requests are typically much faster since only a handful of JSON data is exchanged between the client and the server, as opposed to full HTML documents. | **Security issues**: single-page web applications are highly sensitive to XSS attacks.
**Seamless experience**: since the web page is never fully refreshed, the UI appears more responsive. | **JavaScript dependency**: turn off JavaScript from your browser and the webpage will remain an empty document!

# Use cases

Typical use cases include:

* A webmail client
* A chat application
* Any proprietary application running on an intranet where SEO does not matter

---

# References

[^1]: This is now addressed in the biggest frameworks with **Routing** modules, e.g. [Angular Router](https://angular.io/guide/router) or [React Router](https://reactrouter.com/)

[^2]: There are techniques to try and remediate this issue, such as pre-rendering the pages.


