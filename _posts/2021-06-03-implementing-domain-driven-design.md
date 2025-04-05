---
title: Implementing Domain-Driven Design
tags: ddd python books
toc: true
---

![Implementing Domain-Driven Design](/assets/images/2021-06-03-implementing-domain-driven-design.jpg)

Commonly referred to as the **Red Book of DDD**, Implementing Domain-Driven Design tackles practical implementations of the Domain-Driven approach to software engineering. Since the best way to explain coding principles is through actual code, and DDD code is especially clear, the book provides an excellent reading experience.

The programming language used throughout the book is Java. This could have been a barrier to entry given my Python background, but it turned out to be relatively easy to grasp. It also made for a very refreshing experience to be reading well-organized code in a statically-typed language.

This post gathers some reading notes.

* TOC
{:toc}

# Strategic Modeling vs Tactical Modeling

* **Strategic Modeling** is an abstraction exercise where software engineers and domain experts gather and study the terminology used in a given Domain in order to uncover different Bounded Contexts, each Context having its own Ubiquitous Language.

* In contrast, **Tactical Modeling** relies on a series of common coding recipes that could prove useful in implementing DDD. Typical recipes include Entities, Value Objects, Aggregates, Repositories etc. Using these coding patterns does not guarantee that the overall project will be properly modeled, but properly modeling the project will likely result in using these coding patterns.

# Terminology matters

* In Philosophy, a good starting point when writing an essay is to look at the etymology of the very words used in the subject and, from there, explore the spectrum of their meanings and gain insight on the relationships between concepts.

* DDD follows the same approach: by focusing extensively on the words used in a given Domain, the goal is to discover how the concepts interact and use this understanding as a foundation for writing the actual code. In this respect, one can see software engineers as modern-days Socrates engaging with domain experts to extract and model their knowledge.

# Bounded Contexts

It’s okay to model the same concept using different objects as part of different Bounded Contexts.

Consider the example of a book:

* In the Bounded Context of a creative writing software, a book could be modeled as a collection of ideas and a temporary title
* In the Bounded Context of a publisher’s database, a book could be modeled as a structured object with different types of content including a preface and a back cover as well as references to the people who helped finalize it
* In the Bounded Context of an online retailer, the emphasis would be put on the price, the shipping cost, the number of units in stock and a collection of customer reviews

In all three cases it’s still a book, but seen through different Contexts and therefore modeled differently.

# Immutability of Value Objects

Value Objects are immutable. Their attributes are protected by private setters and are validated and set by the constructor once and for all. Therefore the very existence of a Value Object guarantees that it is valid, which makes it much easier to rely on and to manipulate within the Application Layer.

# When to use Domain Services

The very existence of Domain Services with logic in them could be the sign that Entities are Anemic, i.e. simple data storage objects with no logic in it, which is generally a bad practice.

That said, Domain services can be useful in specific cases:
* When an action/computation requires several Entities: instead of choosing which Entity to place the logic in and which Entity to inject, we can choose to write the logic in a separate Domain Service in which to inject both Entities.
* When an action requires to look into a Repository: it is generally not recommended to access a Repository from an Aggregate, but it is acceptable to do so from a Domain Service.

# Two types of Repositories

In his original book, Eric Evans introduces the concept of Repositories as follows:

> For each type of object that needs global access, create an object that can provide the illusion of an in-memory collection of all objects of that type.
>
> Eric Evans, Domain-Driven Design: Tackling Complexity in the Heart of Software

This definition, although simple and elegant, left me with an long-standing question: since Repositories are part of the Domain Layer and behave as a in-memory collection, but also simulate Persistence, to what extent should we give the Repositories a peak into the Infrastructure Layer?

The answer lies in this chapter when Vaughn Vernon takes a more practical approach and distinguishes between two types of Repositories:
* Collection-oriented Repositories containing the following starter methods: `add()`, `addAll()`, `remove()` and `removeAll()`
* Persistence-oriented Repositories where the `add()` and `addAll()` methods are respectively replaced by `save()` and `saveAll()` methods

# Unique identity generated from Persistence Layer

The theory says that Domain objects should not be aware of anything Infrastructure-related. So how do we create Domain objects in the Application Layer when their `id` is dictated by the Infrastructure Layer (typically, an autoincrement id)?

The answer suggested by Vaugh Vernon is to equip the abstract Repository with a special method called `nextIdentity()` in charge of providing the id of the next Entity to be added to the Repository:
* if the identity is decided by the Application Layer (for instance a GUID), then the implementation of this method will return a GUID
* if the identity is decided by the database (autoincrement id), then the implementation of this method will run a quick query to the database to get the next autoincrement id

# Final thoughts

The above notes are only a very partial and personal summary of all the concepts explained in the book. There are for instance entire chapters on things like Event Sourcing or RESTful integration between Bounded Contexts, which go at length into implementation details, and which I haven’t even mentionned.

Also, the Author adds detailed testing strategies along with most explanations and implementations of concepts, which will prove quite useful to the practitioner (e.g. how to test Value Objects, Repositories etc.).

Overall, the book was a good read and I definitely recommend it to intermediate software developers, Java or not.
