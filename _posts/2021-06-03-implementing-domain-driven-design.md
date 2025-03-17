---
title: Implementing Domain-Driven Design
tag: DDD, Python
toc: true
---

Commonly referred to as the “Red Book of DDD”, Implementing Domain-Driven Design tackles practical implementations of the Domain-Driven approach to software engineering. Since the clearest way to expain coding principles is actual code, and DDD code being particularly clear, it makes for an excellent reading experience.

The programming language used throughout the book is Java. This could have been a barrier to entry given my Python background, but it turned out to be relatively easy to grasp. Il also made for a very refreshing experience to be reading well-organized code in a statically-typed language.

This post gathers some reading notes.

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Book chapters

The book is organised as follows:

1. Getting started with DDD
2. Domains, Subdomains, and Bounded Contexts
3. Context Maps
4. Architecture
5. Entities
6. Value Objects
7. Services
8. Domain Events
9. Modules
10. Aggregates
11. Factories
12. Repositories
13. Integrating Bounded Contexts
14. Application

# Strategic Modeling vs Tactical Modeling

* **Strategic Modeling** is an abstraction exercise where software engineers and domain experts gather and study the terminology used in a given Domain in order to uncover different Bounded Contexts, each Context having its own Ubiquitous Language.

* By opposition, **Tactical Modeling** relies on a series of common coding recipes that could prove useful in implementing DDD. Typical recipes include Entities, Value Objects, Aggregates, Repositories etc. Using these coding patterns does not guarantee that the overall project will be properly modeled, but properly modelling the project will likely result in using these coding patterns.

# Terminology matters

* In Philosophy, a good starting point when writing an essay is to look at the etymology of the very words used in the topic and, from there, explore the spectrum of their meaning and gain insight on the relationships between concepts.

* DDD follows the same approach: by focusing extensively on the words used in a given Domain, the hope is to also discover how the concepts interact together and use this as a foundation on which to write the actual code.

* In this respect, one can see software engineers as modern-days Socrates asking questions to Domain experts in order to extract and model the knowledge.

# Bounded Contexts

It’s okay to model the same concept using different objects in different Bounded Contexts.

Taking the example of a book:

In the Bounded Context of a creative writing software, a book is barely more than a collection of Ideas and a TentativeTitle.
In the Bounded Context of a publisher’s database, a book could be modeled as a structured object with different types of Content including a Preface and a BackCover as well as references to the people who helped finalize it.
In the Bounded Context of an online retailer, the emphasis would be put on the Price, the ShippingCost, the number of units in Stock and potential Reviews.

In all three cases it’s still a book, but seen through different Contexts and therefore modeled differently.

# Immutability of Value Objects

Value Objects are immutable objects where the attributes are protected by private setters and are validated and set by the constructor once and for all. Therefore the very existence of a Value Object guarantees that it is valid, which makes it much easier to rely on and to manipulate.

Sidenote: in Python, Value Objects would be either built-in immutable data types (`bool`, `int`, `float`, `tuple`, `str` and `frozenset`) or custom objects. The issue with custom objets is that there is no such thing as true private methods in Python. All there is is the coding convention of prefixing the method name with an underscore to signal to other developers that the method *should not* be accessed directly. The other issue is that equality would default to a referene identity where what we want is an identity by value. To do so, you would need to overwrite the `__eq__` dunder method of the custom object in order to test the equality by only looking at the values of the attributes. 

# When to use Domain Services

* The very existence of Domain Services with logic in them could be the sign that Entities are Anemic, i.e. simple data storage objects with no logic in it, which is generally a bad practice.

* That being said, Domain services can be useful in specific cases:
- When an action/computation requires several Entities: instead of choosing which Entity to place the logic in and which Entity to inject, we can choose to write the logic in a separate Domain Service in which to inject both Entities.
- When an action requires to look into a Repository: it is generally not recommended to access a Repository from an Aggregate, but it’s okay to do so from a Domain Service.

Sidenote: in Python, a class with static methods is usually the implementation of choice for a Domain Service.

# Two types of Repositories

In his original book, Eric Evans introduces the concept of Repositories as follows:

    > For each type of object that needs global access, create an object that can provide the illusion of an in-memory collection of all objects of > > that type.
    >
    > Eric Evans, Domain-Driven Design: Tackling Complexity in the Heart of Software

This definition, although simple and elegant, left me with an long-standing question: since Repositories are part of the Domain Layer and behave as a in-memory collection, but also simulate Persistence, to what extent should we give the Repositories a peak into the Infrastructure Layer?

The answer lies in this chapter when Vaughn Vernon takes a more practical approach and distinguishes between two types of Repositories:

    * Collection-oriented Repositories containing the following starter methods: `add()`, `addAll()`, `remove()` and `removeAll()`
    * Persistence-oriented Repositories where the `add()` and `addAll()` methods are respectively replaced by `save()` and `saveAll()` methods

Sidenote: in Python, a Collection-oriented Repository would be a simple dict (or a custom wrapper around a dict) whereas a Persistence-oriented Repository would contain database queries either directly or through an ORM.

# Unique identity generated from Persistence Layer

The theory says that Domain objects should not be aware of anything Infrastructure-related.

Now let’s consider the following situation:

    All Entities require a unique id
    Entities are created in the Domain Layer and later saved to a Repository
    In our case the Entity id is dictated by the database (for instance, a MySQL autoincrement)

So how do we do that, knowing that our Entities need an id before they are saved to a database and therefore before that very id has a chance to be created?

The answer suggested by Vaugh Vernon is to equip our Repository with a special method called nextIdentity() in charge of providing the id of the next Entity to be added/saved into the Repository.

If the Identity is decided by the Application Layer (for instance a GUID), then the implementation of this method will return a GUID. But if the Identity is decided by the database like in our example, then the implementation of this method will run a quick query to the database to get the next autoincrement id.

# Final thoughts

The above notes are only a very partial and personal summary of all the concepts explained in the book. There are for instance entire chapters on things like Event Sourcing or RESTful integration between Bounded Contexts, which go at length into implementation details, and which I haven’t even mentionned.

Also, the Author adds detailed testing strategies along with most explanations and implementations of concepts, which will prove quite useful to the practitioner (e.g. how to test Value Objects, Repositories etc.).

Overall, the book was a good read that I would definitely recommend to intermediate software developers, Java or not.