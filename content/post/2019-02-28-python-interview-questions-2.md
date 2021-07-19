+++
title = "Series: Python Interview Questions #2"
date = "2019-02-28"
draft = false
tags = ['python', 'interview']
+++

Task: merge and sort two ordered lists
```python
list_a = [8, 10, 12, 44, 63, 66]
list_b = [2, 3, 20, 106, 144]
solution = [2, 3, 8, 10, 12, 20, 44, 63, 66, 106, 144]
```

<!--more-->

And here Pythonista might cheat a bit by using Python built-in functionality. However, is this an efficient solution?

```python
# 1. Adding the list and use sorted 
def solution1(a, b):
    lst = a + b
    return sorted(lst)

# 2. Is extending list better?
def solution2(a, b):
    lst = a[:]
    lst.extend(b)
    return sorted(lst)
    
# 3. Smart way using heapq
from heapq import merge
def solution3(a, b):
    return merge(a, b)
```

If the task was to write the pure algorithm manually, we could do the following:

```python
def solution4(a, b):
    solution = [0] * (len(a)-1 + len(b)-1)
    i, j, k = 0, 0, 0
    
    while i < len(a) and j < len(b):
        if a[i] < b[j]:
            solution[k] = a[i]
            i += 1
        else:
            solution[k] = b[j]
            j += 1
        k += 1

    if i < len(a):
        solution.extend(a[i:])
    if j < len(b):
        solution.extend(b[j:])
    return solution
```

The final solution using Python tricks again (iterator)

```python
def solution5(a, b):
    if a[-1] > b[-1]:
        a, b = b, a

    iterator = iter(b)
    y = iterator.__next__()
    solution = []

    for x in a:
        while y < x:
            solution.append(y)
            y = iterator.__next__()
        solution.append(x)

    solution.append(y)
    solution.extend(iterator)
    return solution
```

---

Summarising the results:

1. Python clearly wins over manually written algorithms, and solution three is a way better than solution five. So there is not too much need to implement our algorithm in the real world. Just use Python batteries.
2. Also, using `heapq.merge` (solution three) is the fastest way from the first three solutions.

For those who like to see numbers:

```python
ms - milliseconds
µs - microseconds
ns - nanoseconds

list_a = [8, 10, 12, 44, 63, 66]
list_b = [2, 3, 20, 106, 144]

%timeit solution1(list_a, list_b)
669 ns ± 10.1 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit solution2(list_a, list_b)
886 ns ± 48.1 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit solution3(list_a, list_b)
594 ns ± 17 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit solution4(list_a, list_b)
5.64 µs ± 188 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

%timeit solution5(list_a, list_b)
2.6 µs ± 104 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

I have have been using Jupyter notebook for calculations, located [HERE](https://github.com/pbedn/hugo-blog/blob/master/src/merge_and_sort_two_ordered_lists.ipynb)

---

References:

* [stackoverflow thread on combining two sorted lists](https://stackoverflow.com/questions/464342/combining-two-sorted-lists-in-python)
* [geeksforgeeks on merging two sorted arrays](https://www.geeksforgeeks.org/merge-two-sorted-arrays/)
