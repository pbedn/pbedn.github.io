+++
title = "Python dictionary is now officially ordered!"
date = "2018-06-30"
draft = false
tags = ['python']
+++

Python 3.5 brought us 4-100 times faster *OrderedDict* thanks to its implementation in C, before was written in Python. Python 3.6 improved a standard library dictionary, by making it use more compact representation. Now dictionary could use 20% to 25% less memory compared to Python 3.5, and the order of items was kept but as suggested it was only an implementation detail and should not be used officially (backwards compatibility). Well, in Python 3.7 it has been approved.

Which one should we use now? Standard dict or OrderedDict ?

<!--more-->

---

Let's dive a bit into the history of dictionaries innovation in Python 3 over the years (source: Python official documentation).

**Python 3.0**

* Dict methods *dict.keys()*, *dict.items()* and *dict.values()* now return *views* instead of lists. Views shows what is inside the dictionary without making a copy.

```python
>>> d = dict(a=10, b=20, c=30, d=40)
>>> d
>>> {'b': 20, 'c': 30, 'a': 10, 'd': 40}
>>> d.keys()
>>> dict_keys(['b', 'c', 'a', 'd'])
>>> d.items()
>>> dict_items([('b', 20), ('c', 30), ('a', 10), ('d', 40)])
```

* The *dict.iterkeys()*, *dict.iteritems()* and *dict.itervalues()* methods are no longer supported.

```python
>>> d.iterkeys()
>>> ...
    AttributeError: 'dict' object has no attribute 'iterkeys'
```

**Python 3.1**

* PEP 372: Adding an ordered dictionary to collections

```python
>>> from collections import OrderedDict
>>> OrderedDict(a=10, b=20, c=30, d=40)
>>> d['a'] = 10
>>> d['b'] = 20
>>> d.items()
[('a', 10), ('b', 20)]
```
**Python 3.2**

* New method move_to_end() in collections.OrderedDict class. It takes an existing key and moves it to either the first or last position in the ordered sequence.

```python
>>> d = OrderedDict.fromkeys(['a', 'b', 'X', 'd', 'e'])
>>> list(d)
['a', 'b', 'X', 'd', 'e']
>>> d.move_to_end('X')
>>> list(d)
['a', 'b', 'd', 'e', 'X']
```

* Set and dict can now hold more than 2**32 entries on builds with 64-bit pointers (previously, they could grow to that size, but their performance degraded catastrophically).


**Python 3.3**

* PEP 412 - Key-Sharing Dictionary.

    Dictionaries used as attribute dictionaries (with ```__dict___``` attribute) are implemented in a split-table form. It means they can share keys with other attribute dictionaries of instances of the same class.

```python
>>> import sys

>>> d = dict(a=10, b=20)
>>> sys.getsizeof(d)
>>> 136

>>> class D():
>>>     def __init__(self, a, b):
>>>         self.a = a
>>>         self.b = b
>>> d = D(10, 20)
>>> sys.getsizeof(d)
>>> 32
```

**Python 3.4**

* PEP 456: Secure and Interchangeable Hash Algorithm (SipHash is now default string and bytes hash algorithm).

**Python 3.5**

* Improvement in collections.OrderedDict, now implemented in C, which makes it 4 to 100 times faster.

**Python 3.6**

* The dict type has been reimplemented to use a more compact representation based on a proposal by Raymond Hettinger and similar to the PyPy dict implementation. This resulted in dictionaries using 20% to 25% less memory when compared to Python 3.5.

**Python 3.7**

* The insertion-order preservation nature of dict objects has been declared to be an official part of the Python language spec.

```python
>>> dict(a=10, b=20, c=30, d=40)
>>> {'a': 10, 'b': 20, 'c': 30, 'd': 40}

>>> a = dict(a=10,b=20)
>>> b = dict(b=20,a=10)
>>> a
{'a': 10, 'b': 20}
>>> b
{'b': 20, 'a': 10}
>>> a == b
True
>>> a is b
False
```

---

Now some simplified theory about dictionaries (source: Python official documentation, Brandon Rhodes and Raymond Hettinger talks).

* **dictionary** - *mapping* object that maps *hashable* values to arbitrary objects

* **mapping** - container object that supports arbitrary key lookups (dict and collections: defaultdict, OrderedDict, Counter)

* **hashable** - object is hashable if it has a hash value which never changes during its lifetime (```__hash__``` method) and can be compared to other objects (```__eq__``` method)

Dictionary is a hash table that maps key to values. Python dictionary *key* in comparison to list can be any value. Therefore, to save it in RAM we need to convert (hash) key to integer (index in list is already an integer) and compute index.

Under the hood, Python dictionary is in the beginning an 8 element list. 

```
[Index, Hash, Key, Value]
[000  , ... , ...,  ... ]
[001  , ... , ...,  ... ]
...
[111  , ... , ...,  ... ]
```

Python currently uses SipHash cryptographic algorithm for hashing (details on why can be found watching [Brandon Rhodes PyCon 2017 talk][brandon-2017]). Previously it was multiplication and later randomized multiplication. Hashing function always produces the same hash. However, sometimes we get the same hash return on different hashed key. Also, when two keys want the same index slot, we get a collision. Python is using *open addressing technique* to solve that problem. It tries to find the next free address in the hash table, using special probing technique (iterates over slots to find free one; python doesn't do it in a linear way). When a key was deleted from the dictionary, it did not leave the slot empty but put there special constant telling that it was just deleted. 

The first conclusion of that process is the fact that dictionaries were not ordered. At least this was the case in all Pythons < 3.5. The second was time complexity. Amortized Worst Case is O(n), but usually dictionaries behave better, having average case O(1) and that is for get, set and delete item; O(n) for iteration and copy. Why amortized worst case is O(n)? Because of collisions.

---

Let's play with both dictionaries implementation and choose the winner.

```python
Python 3.6.5 (v3.6.5:f59c0932b4, Mar 28 2018, 16:07:46) [MSC v.1900 32 bit (Intel)]
IPython 6.4.0 -- An enhanced Interactive Python. Type '?' for help.

# dict comprehension
>>> std_dict = {a: a*a for a in range(1000000)}
# generator comprehension
>>> ord_dict = OrderedDict(((a, a*a) for a in range(1000000)))

# type object
>>> type(std_dict)
dict
>>> type(ord_dict)
collections.OrderedDict

# size of an object in bytes
>>> sys.getsizeof(std_dict)
25165888
>>> sys.getsizeof(ord_dict)
49554528

# equality
>>> std_dict is ord_dict
False
>>> std_dict == ord_dict
True


>>> %timeit del std_dict[10000]; std_dict[10000] = 10000
185 ns ± 17.4 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

>>> %timeit del ord_dict[10000]; ord_dict[10000] = 10000
249 ns ± 23.3 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

>>> %timeit i = randint(1, 100000); del std_dict[i]; std_dict[i] = i
2.88 µs ± 11.9 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

>>> %timeit i = randint(1, 100000); del ord_dict[i]; ord_dict[i] = i
3.21 µs ± 194 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

The conclusion is interesting. Standard dictionary is smaller in size and is a bit faster on deleting and adding a single key.

Ps. OrderedDict has nice additional *method move_to_end(key, last=True)* - it moves an existing key to either end of an ordered dictionary.

---

Resources:

* [Python official documentation][python-dict-docs]
* [Brandon Rhodes PyCon 2010 talk][brandon-2010]
* [Brandon Rhodes PyCon 2017 talk][brandon-2017]


[python-dict-docs]: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
[brandon-2010]: https://www.youtube.com/watch?v=oMyy4Sm0uBs
[brandon-2017]: https://www.youtube.com/watch?v=66P5FMkWoVU
