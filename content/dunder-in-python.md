---
title: "Dunder in Python"
date: 2025-11-24
draft: false
tags: ["python", "programming", "software-development"]
status: "seeding"
summary: "Double underscore methods and attributes in Python that provide special functionality to classes and objects."
---

"Dunder" is short for "double underscore" and is commonly used in programming to refer to special methods and attributes in Python that are surrounded by double underscores on both sides of their name. These dunder methods and attributes have specific meanings and behaviors that provide functionality to classes and objects in Python.

## Common Dunder Methods

### Object Representation
- `__init__`: Constructor method, called when an object is created
- `__str__`: String representation for end users
- `__repr__`: String representation for developers/debugging

### Operator Overloading
- `__add__`, `__sub__`, `__mul__`: Arithmetic operations
- `__eq__`, `__lt__`, `__gt__`: Comparison operations
- `__len__`: Length/size of object
- `__getitem__`, `__setitem__`: Indexing and slicing

### Context Managers
- `__enter__`, `__exit__`: Used with `with` statements

## Dunder Attributes

- `__name__`: Name of module, class, or function
- `__doc__`: Documentation string
- `__dict__`: Dictionary containing object's attributes
- `__class__`: Class to which an object belongs
- `__module__`: Module in which class is defined

## Purpose

Dunder methods enable:
- Customizing object behavior
- Operator overloading
- Context management
- Making objects more Pythonic and intuitive

## Related Concepts

- Object-oriented programming
- Python language features
- Special methods and magic methods

---
*Definition based on Python programming practice.*

