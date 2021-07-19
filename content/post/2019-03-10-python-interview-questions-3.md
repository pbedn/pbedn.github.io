+++
title = "Series: Python Interview Questions #3"
date = "2019-03-10"
draft = false
tags = ['python', 'interview']
+++

Task: Define the function that takes two parameters (dictionaries as default). Without modifying original dictionaries return the new one that includes all key and value pairs
however, if key repeats itself, then add values

```python
a = {'a': 5, 'b': 1, 'c': -1, 'd': 1}
b = {'a': 1, 'b': 5, 'c': 1, 'e': 1}
solution = {'a': 6, 'b': 6, 'c': 0, 'd': 1, 'e': 1}
```

<!--more-->

```python
# 1. Copy and check for existing key
def solution1(a, b):
    result = dict(a)
    for k, v in b.items():
        if result.get(k):
            result[k] += v
        else:
            result[k] = v
    return result

# 2. We could use try-except clause instead
def solution2(a, b):
    result = dict(a)
    for k, v in b.items():
        try:
            result[k] += v
        except KeyError:
            result[k] = v
    return result
    
# 3. If values would be ONLY POSITIVE, we could use Counter
from collections import Counter

def solution3(a, b):
    return Counter(a) + Counter(b)

# 4. For Python 3.5+ there is dictionary unpacking
def solution4(a, b):
    intersection = set(a) & set(b)
    c = {k: a[k] + b[k] for k in intersection}
    return {**a, **b, **c}
    

# 5. Or even shorter
solution5 = lambda a, b: {**a, **b, **{k: a[k] + b[k] for k in a.keys() & b}}
```

I like solution three, but it could be used only for individual cases, and solution 4, because of
sets, unpacking dictionaries and being very clear for reading. A nice trick is in solution five. We do not have to make parameter b as set explicitly, it is enough to take keys from parameter a ([dictionary view objects](https://docs.python.org/release/3.3.0/library/stdtypes.html#dict-views)) and use intersection with parameter b. 

---

Also, traditionally I would like to see which solution is the fastest (solution three excluded):

```python
µs - microseconds
ns - nanoseconds

%timeit -n 100000 solution1(a, b)
1.56 µs ± 80.1 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit -n 100000 solution2(a, b)
1.61 µs ± 106 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit -n 100000 solution3(a, b)
11 µs ± 216 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit -n 100000 solution4(a, b)
2.01 µs ± 64.8 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit -n 100000 solution5(a, b)
2.08 µs ± 122 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

I have have been using [Jupyter notebook](https://github.com/pbedn/hugo-blog/blob/master/src/combine_two_dicts_and_add_values_if_repeating.ipynb) for calculations 

---

References:

* [Stack Overflow thread: combining two dicts](https://stackoverflow.com/questions/11011756/is-there-any-pythonic-way-to-combine-two-dicts-adding-values-for-keys-that-appe/11011846?fbclid=IwAR0TDEIuWeCkmHJw3UyBrsLdnrymq-AXyel9ox1oOaxFK6xe9m7gS3008qI)
