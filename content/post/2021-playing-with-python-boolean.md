+++
title = "Playing with Python: Boolean of empty containers"
date = "2021-04-15"
draft = false
tags = ['python']
+++

It is funny that when one decides to refresh the basics of Python, watches the first video about types and is already stuck for an hour testing edge cases and wondering why?
Let's explore the bool function with empty containers.

<!--more-->

#### Simple tests

```python
>>> bool()
False
>>> bool(())
False
>>> bool([])
False
>>> bool({})
False
>>> bool(tuple())
False
>>> bool(list())
False
>>> bool(dict())
False
>>> bool(set())
False
```

Basics :)

#### Nested tuple tests

```python
>>> bool(tuple(tuple()))
False
>>> bool((()))
False
>>> bool(tuple(list()))
False
>>> bool(([]))
False
>>> bool(tuple(dict()))
False
>>> bool(({}))
False
>>> bool(tuple(set()))
False
```

Easy, right?

```python
>>> type(())
<class 'tuple'>
>>> type(([]))
<class 'list'>
>>> type(({}))
<class 'dict'>
>>> ({}) == {}
True
>>> ([]) == []
True
```

Looks like Python treats here our tuple just like additional braces.


#### Nested list tests

```python
>>> bool(list(tuple()))
False
>>> bool([()])
True
>>> bool(list(list()))
False
>>> bool([[]])
True
>>> bool(list(dict()))
False
>>> bool([{}])
True
>>> bool(list(set()))
False
```

That was interesting! Why do we get true for bool with a list containing tuple when we use literals but not when we call using function?

```python
>>> len(list(tuple()))
0
>>> len(list(()))
0
>>> len([()])
1
```

Boolean in Python translates to integer, and when we check the length function, we can see that whatever we put inside the list literal has a length greater than zero. So why call to list function returns zero?

From Python documentation:

    By default, an object is considered true unless its class defines either a __bool__() method that returns False or a __len__() method that returns zero, when called with the object.

Alright, that is kind of clear. I guess we could go deeper into the CPython rabbit hole (which I did by the way) to find what exactly ```__len__``` does... but in quality blog articles we have to say: Let's leave this as an exercise to the reader ;)


#### Nested set tests

```python
>>> bool(set(tuple()))
False
>>> bool({()})
True
>>> bool(set(list()))
False
>>> bool({[]}) # Boom !
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'

>>> bool(set(dict()))
False
>>> bool({{}}) # Boom !
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'dict'

>>> bool(set(set()))
False
>>> bool({{None}})
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'set'
```

While it might look surprising, the last TypeErrors are quite straightforward. Tuple as the immutable sequence can be used as a dictionary key or stored in a set because it has support for the hash() built-in. However, list, dictionaries and sets are mutable objects. 

---

Resources:

* [Python documentation - your only source of truth][python-docs]

[python-docs]: https://docs.python.org/3/index.html
