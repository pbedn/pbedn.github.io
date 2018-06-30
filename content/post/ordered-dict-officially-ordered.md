+++
title = "Python dictionary is now officially ordered!"
date = "2018-06-30"
draft = true
tags = ['python']
+++

Lets dive into the history of innovation a bit. 

Python 3.3
* PEP 412 - Key-Sharing Dictionary
Python 3.5 brought us 4-100 times faster *OrderedDict* thanks to its implementation in C, before was written in Python (Eric Snow). Python 3.6 improved a standard library dictionary, by making it use more compact representation (Raymond Hettinger and ideas from PyPy). Now dictionary could use 20% to 25% less memory compared to Python 3.5, and the order of items was kept but as suggested it was only an implementation detail and should not be used officially (backwards compatibility). Well, in Python 3.7 it has been aprroved.

Which one should we use now? Standard dict or OrderedDict ?

<!--more-->

First some simplified theory about dictionaries. I used Python official documentation, Brandon Rhodes and Raymond Hettinger talks. Resources are cited below.

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

Python currently uses SipHash cryptographic algorithm for hashing (details on why can be found watching [Brandon Rhodes PyCon 2017 talk][brandon-2017]). Previously it was multiplication and later randomized multiplication. Hashing function always produces the same hash. But sometimes we get the same hash return on different hashed key. And when two keys want the same index slot we get a collision. Python is using *open addressing technique* to solve that problem. It tries to find the next free address in the hash table, using special probing technique (iterates over slots to find free one; python doesn't do it in linear way). When key was deleted from dictionary, it did not leave the slot empty but put there special constant telling that it was just deleted. 

Conclusion of that process is fact that dictionaries are not ordered. At least this was the case in all Pythons < 3.5.

Amortized Worst Case is O(n), but usually dictionaries behave better, having average case O(1) and that is for get, set and delete item; O(n) for iteration and copy.

Why O(n), because of collisions -> python uses open addressing -> lists inside

---

Resources:

* [Python official documentation][python-dict-docs]
* [Brandon Rhodes PyCon 2010 talk][brandon-2010]
* [Brandon Rhodes PyCon 2017 talk][brandon-2017]


[python-dict-docs]: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
[brandon-2010]: https://www.youtube.com/watch?v=oMyy4Sm0uBs
[brandon-2017]: https://www.youtube.com/watch?v=66P5FMkWoVU