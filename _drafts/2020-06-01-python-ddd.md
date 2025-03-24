---
title: Domain-Driven Design with Python
tags: python ddd
---


python ddd


Sidenote: in Python, Value Objects would be either built-in immutable data types (`bool`, `int`, `float`, `tuple`, `str` and `frozenset`) or custom objects. The issue with custom objets is that there is no such thing as true private methods in Python. All there is is the coding convention of prefixing the method name with an underscore to signal to other developers that the method *should not* be accessed directly. The other issue is that equality would default to a referene identity where what we want is an identity by value. To do so, you would need to overwrite the `__eq__` dunder method of the custom object in order to test the equality by only looking at the values of the attributes. 


Sidenote: in Python, a class with static methods is usually the implementation of choice for a Domain Service.




Sidenote: in Python, a Collection-oriented Repository would be a simple dict (or a custom wrapper around a dict) whereas a Persistence-oriented Repository would contain database queries either directly or through an ORM.