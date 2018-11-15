+++
title = "Series: Python Interview Questions #1"
date = "2018-11-15"
draft = false
tags = ['python', 'interview']
+++

This morning for the interviewed person we had a short algorithmic task. Given n = 5, create a list of multiplied integers:
```python
[1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 5, 5, 5, 5, 5]
```

<!--more-->

First working example we got was:
```python
def list_a(n):
    temp = []
    for i in range(1, n+1):
        for j in range(i):
            temp.append(f)
    return temp
```
This is ok, but we could do better and suggested for shorter version. Then we got nicer one-liner:
```python
list_b = [i for i in range(1, n+1) for k in range(i)]
```

My question was about algorythmic complexity, can we do even better?
```python
def list_c(n):
    temp = []
    for i in range(1, n+1):
        temp += [i] * i
    return temp
```

Some tests in Jupyter notebook to measure execution time:
```python
%timeit list_a(1000)
32.7 ms ± 937 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

%timeit list_b(1000)
14.3 ms ± 396 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

%timeit list_c(1000)
3.47 ms ± 52.7 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```

---

The final note, beginning from python 3.5 we can use 'Additional Unpacking Generalizations' as described in PEP 448.
It is nice feature that allows us to unpack lists with ease:
```python
>>> print(*[1], *[2], 3)
1 2 3
>>> [*range(4), 4]
[0, 1, 2, 3, 4]
```
But can we modify our function to one-liner?

```python
def list_d(n):
    temp = []
    for i in range(1, n+1):
        temp.append(*[[i] * i])
    return temp
 
SyntaxError: iterable unpacking cannot be used in comprehension
```

Unfortunately not yet. However, as we read in PEP 448 

> This PEP does not include unpacking operators inside list, set and dictionary comprehensions **although this has not been ruled out for future proposals**.
