+++
title = "Singleton Design Pattern in Python"
date = "2020-01-02"
draft = false
tags = ['python', 'design pattern']
+++

Recently I dived into design patterns a little bit, to refresh a rusty knowledge.
It is something we are using in Python daily, however not often realizing that.
And I like reading those religious debates between programmers. One of them is the usage
of Singleton in Python. Let's see what is Pythonistas take on this beloved pattern.

<!--more-->

Design patterns are tried and tested solutions to common problems in Software Design.
The capture experiences and provides a solution template.

Categories of Design Patterns:

* Creational - objects creation mechanism, concern the ways and means of object instantiation
* Structural - how to assemble objects and classes into longer structures, 
deal with the mutual composition of classes or objects
* Behavioral - effective communication between objects, 
analyze the ways in which classes or objects interact and distribute responsibilities among them

Basic rules:

* Separate the things that change from those that stay the same
* Program to an interface, not and implementation
* Prefer composition over inheritance 
* Delegation

---

*Singleton* is a creational design pattern that lets you ensure that a class has only one instance, 
while providing a global access point to this instance.

There are many ways to implement it. First one might use magic *__new__* method. 
However, it is not clear for the client of a class if it is creating a Singleton. 
Subclassing is a problem.

```python
class Singleton:
    _instance = None
    
    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super().__new__(cls, *args, **kwargs)
        return cls._instance

>>> first_instance = Singleton()
>>> second_instance = Singleton()
>>> first_instance is second_instance  # True

# however it is not always clear

class Foo(Singleton): pass
class Bar(Singleton): pass

>>> f = Foo()
>>> b = Bar()
>>> f is b  # True

```

We could use a variation of Singleton with shared state - Monostate pattern, called 'Borg' by Alex Martelli.
Here we are more concerned about the state than identity. 

```python
class Borg:
    _shared_state = {}
    
    def __new__(cls, *args, **kwargs):
        obj = super().__new__(cls, *args, **kwargs)
        obj.__dict__ = cls._shared_state
        return obj

# and subclassing is no problem

class Foo(Borg): pass
class Bar(Foo): pass
class Baz(Foo): _shared_state = {}  # data overriding
```

The second way uses *__call__* and metaclasses.

```python
class SingletonMeta(type):
    _instance = None
    
    def __call__(self):
        if not self._instance:
            self._instance = super().__call__()
        return self._instance

class Singleton(metaclass=SingletonMeta):
    ...

>>> first_instance = Singleton()
>>> second_instance = Singleton()
>>> first_instance is second_instance  # True
```

A better approach uses *classmethod*. It is more clear and does not block you from creating a usual instance.
Additionally provides lazy instantiation.

```python
class Singleton:
    _instance = None
    
    @classmethod
    def get_instance(cls):
        if not cls._instance:
            cls._instance = cls()
        return cls._instance

>>> first_instance = Singleton.get_instance()
>>> second_instance = Singleton()
>>> first_instance is second_instance  # False
>>> third_instance = Singleton.get_instance()
>>> first_instance is third_instance  # True
```

The more straightforward way uses modules, which are imported only once. It is hidden functionality we are often using and do not associate it with Singleton design pattern. But it accomplishes the same thing.

```python
class Singleton:
    pass

singleton = Singleton()

# from another module
from my_module import singleton
```

The most common reason against Singletons is that they are an implicit shared state. For example,
global variables and top-level modules are explicit shared state. However, let's say that the
singletons are constant. Because the usage of global constant, is widely accepted, it makes the singletons right choice
because none of the users can break them. Another point is when the singletons do not have shared state, 
only take data (good example here is logging). In this scenario, the simplicity of using the singletons makes them a reasonable choice.

In the end, some voices will always say that no matter how we implement them, Singletons are not Pythonic.
And argue with that ;)

---

References:

* [Design Patterns in Python for the Untrained Eye - Pycon 2019][ortiz]
* [Why you don't need design patterns in Python? - Pycon 2019][bucz]
* [Creating a singleton in Python - StackOverflow][stack]
* [Singleton - Refactoring Guru][guru]
* [Advanced Topics in Programming Languages Series: Python Design Patterns - Google Tech Talks][mart]

[ortiz]: https://www.youtube.com/watch?v=o1FZ_Bd4DSM
[bucz]: https://www.youtube.com/watch?v=G5OeYHCJuv0
[stack]: https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python
[guru]: https://refactoring.guru/design-patterns/singleton
[mart]: https://www.youtube.com/watch?v=1Sbzmz1Nxvo

