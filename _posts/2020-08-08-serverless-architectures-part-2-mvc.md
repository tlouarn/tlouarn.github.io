---
title: Serverless architectures (Part 2) - MVC
subtitle: How to design a serverless architecture on AWS to host a dynamic website
tags: aws architecture
---

# Introduction

**Model-View-Controller** (MVC) is a software **design pattern** which consists in separating data (the Model) from display (the View). The **Model** is the dynamic data structure. Models typically interact with databases through an ORM. The **View** is the final representation of the information. It could be a webpage, a chart, a PDF, an Excel file etc. The **Controller** is the conductor processing the request, handling the Model and rendering the View.

This pattern introduces flexibility both in data sources (it's easy to change the database source as long as the models themselves remain unchanged) and in rendering (it's easy to change the views and render JSON data instead of an HTML document for instance).

Popular server-side MVC frameworks include [Rails](https://rubyonrails.org/) (written in Ruby), [Django](https://www.djangoproject.com/) (Python), [Symfony](https://symfony.com/) (PHP) and my old-time favorite [CodeIgniter](https://codeigniter.com/) (PHP as well).

# Implementation

Here is how to implement this solution on AWS:

![Diagram 1: server-side MVC lambda](/assets/images/tlouarn-serverless-architectures-part-2-mvc.svg)

* Route53 redirects the request to API Gateway
* An API is deployed that redirects all requests to a single Lambda (using greedy paths[^1] and Lambda proxy integration[^2])  
* The Lambda implements MVC pattern and returns a response which body is an HTML document
* On API Gateway, the API is tweaked to return text/html instead of the default application/json

The Lambda can be organized in any way you like, but why not structure the code as MVC?

Let's take a simple example for a blog post and see how that would work out:

1. `GET example.com/blog/2020/06/01/title-of-blog-post`
2. The Lambda fires up a Router
3. The Router reads the path, assigns the request to a BlogController
4. The BlogController instantiates a BlogModel which connects to a database to retrieve the data
5. The BlogController then instantiates a BlogView, fills the template with the data from BlogModel
6. The BlogController sends back the resulting HTML in response body

# Pros & Cons

Pros | Cons
--- | ---
**Compartmentalization**: each layer is clearly separated and the project is scalable. Following the example above: simply add another post in the database and the link `GET example.com/blog/2020/06/03/title-of-another-blog-post` will work just fine. | **Resource intensive**: rendering a simple HTML page requires to go through the whole process of routing the request, instantiating the correct controller, loading up the data, rendering the view and sending back the HTML document, so each request effectively comes with a computational overhead.
**Maintainability**: MVC allows different teams to focus on different parts of the application: frontend designers would focus on the views (relying on template tags) while the backend engineers would work on the Models and the database connections. | **Complexity**: although simple on paper, MVC design will inevitably encounter limits. One of them is the difficulty for the View to directly access the Model: the communication must always go through the Controller.
**SEO friendly**: URLs can be used as deep links[^3] since even if the webpage is generated on the fly, it will remain linked to that specific URL path and will be served as a full HTML document when requested. | **Bandwidth and latency**: this architecture will consume more bandwidth than a single-page application since a full HTML document needs to be sent back at each request. Also, it can create bottlenecks in database access that would slow down the request processing[^4].

# Use cases

Typical use cases include:

* An active blog with comments: each page refresh will include the latest comments including the ones posted since the page was last refreshed
* An online store: new articles simply have to be inserted in the database for the website to start displaying them

---

# References

[^1]: Greedy paths are path variables written as `{proxy+}` that will capture any path.

[^2]: A **Lambda proxy integration** essentially means that the API Gateway will direct the original request directly to the lambda without any modification, therefore all controls and variable mappings will have to be implemented within the Lambda.

[^3]: **Deep links** point directly and all the way to a specific resource: `https://example.com/blog/blog-post-title-1` is a deep link, but `/blog/blog-post-title-1` is not.

[^4]: For instance, if each page contains a blog post as well as the titles of the last 10 posts in the sidebar, this solution would need to lookup the last 10 blog posts **for each page request**, as opposed to a [single-page application](serverless-architectures-aws-part-1-single-page-application) that would load the last 10 posts only once and after that only load the requested blog post.

[SQLAlchemy](https://www.sqlalchemy.org/) in Python