---
title: Serverless architectures (Part 3) - SSG
subtitle: How to host a static site generator on AWS
tags: aws cloud
---

# Introduction

This last solution can look like a step back towards the old days of the Web when people were writing and posting static HTML pages, but it can still be relevant today in specific cases.

Here, pages are pre-rendered and stored, like a cache. No more on the fly computation (be it [client-side](serverless-architectures-aws-part-1-single-page-application) or [server-side](serverless-architectures-aws-part-2-server-side-mvc-lambda) with a MVC lambda): all the pages are already pre-rendered before the user request comes in.

Static site frameworks include [Hugo](https://gohugo.io/) (Go language), [Pelican](https://blog.getpelican.com/) (Python), [MkDocs](https://www.mkdocs.org/) (Python as well) and many more that you can find on [StaticGen](https://www.staticgen.com/).

# Implementation

Here is how to implement the solution on AWS:

![Diagram 1: static site generator](/assets/images/tlouarn-serverless-architectures-part-3-ssg.svg)

* A CRON job implemented via CloudWatch runs a Lambda at regular intervals (once a day for instance)
* This Lambda generates all the pages of the website (for instance by loading data from DynamoDB) and pre-renders the HTML pages
* The pre-rendered HTML pages are stored on S3 in a public bucket
* When the user GET a specific page, Route53 redirects towards the public S3 bucket and the user gets the pre-rendered HTML page

# Pros & Cons

Pros | Cons
--- | ---
**Reduced latency**: no need to instantiate half a dozen objects to generate some HTML, the webpages are already sitting here on the server ready to be sent back to the client. Zero computation needed! | **Read-only**: static websites are... read-only. There's no server ready to handle interactions with visitors (think search, comments or contact forms)[^1].
**SEO friendly**: URLs point to real resources, allowing crawling, deep linking and keywording. | **Less personalization**: what if you want to welcome the visitor in their native language? Suddenly personalization does not work anymore, since different users will speak different languages but there is only 1 version of the HTML page for everybody.
**Increased security**: it's much harder to hack a read-only resource! Less interactions with the server mean less vulnerabilities and fewer doors open for attacks. | **No reactivity**: impossible to display dynamic content, like fresh comments in a blog or new articles in an online store. These would have to wait for the next CRON batch in order to be integrated in the HTML.[^2]

# Use cases

Typical use cases include:

* A showcase website
* A technical documentation website

---

# References

[^1]: This can be mitigated with third-party plugins, such as [Disqus](https://disqus.com/) for comments or [Google Programmable Search Engine](https://programmablesearchengine.google.com/about/) for search.

[^2]: One way to overcome this is to implement a page-rendering process that kicks in after a new piece of data is added to the database, and would only pre-render that specific data (think a Lambda linked to a DynamoDB stream).