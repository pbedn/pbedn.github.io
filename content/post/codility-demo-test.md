+++
title = "Codility Demo Test"
date = "2018-05-18"
draft = false
tags = ["python", "codility"]
+++

I have had a chance to solve [Codility demo test][codility-demo-test]. It is available for free, and you can try it unlimited times. While I do not believe that such tests correlate to typical real-world programming tasks, they are often used as a first interview filter by some companies. Here I show a few Python variations for the solution.

<!--more-->

It is fascinating that each programmer solves the same task differently. First two solutions are mine, third belongs to my girlfriend, and last I found on [nkbits blog][nkbits-missing-integer]. 

---

### Task description

Write a function:

*def solution(A)*

that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = [1, 3, 6, 4, 1, 2], the function should return 5.

Given A = [1, 2, 3], the function should return 4.
Given A = [−1, −3], the function should return 1.

Assume that:

N is an integer within the range [1..100,000];
each element of array A is an integer within the range [−1,000,000..1,000,000].
Complexity:

* expected worst-case time complexity is O(N);
* expected worst-case space complexity is O(N) (not counting the storage required for input arguments).

---

### First approach

First naive implementation, I like to use sets everytime I can. In range I used m+2 in to prevent set being empty and throwing an error when using min function.

```python
def solution(A):
    m = max(A)
    if m <= 0:
        return 1
    possible = set(e for e in range(1, m+2)) - set(A)
    return min(possible)
```

Results:

* Task score - 88%
* Correctness - 100%
* Performance - 75%

I received TIMEOUT ERROR in medium performance tests (chaotic sequences length=10005 (with minus))

### Second

Here I did better, first filtering for only positive numbers an returning 1 in case of an empty set. Then I use python built-in set symmetric difference.

```python
def solution(A):
    s = {x for x in A if x > 0}
    if not s:
        return 1
    last = max(s)+1

    # Find elements present in either of the two sets, but not common to both the sets
    new_set = s^set(range(1, last))

    if not new_set:
        return last
    return min(new_set)
```

Results:

* Task score - 100%
* Correctness - 100%
* Performance - 100%

Detected time complexity: O(N) or O(N * log(N))


### Third

My girlfriend approach results in time complexity worse than O(N), because of using *sorted*. Still, I find it interesting.

```python
def solution(A):
    B = [x for x in A if x > 0]
    B = sorted(B)
    if 1 not in B:
        return 1
    for i in range(0, len(B) - 1):
        if B[i+1] - B[i] > 1:
            return B[i] + 1
    return max(B) + 1
```

Results:

* Task score - 100%
* Correctness - 100%
* Performance - 100%

Detected time complexity: O(N) or O(N * log(N))

### Fourth try: How to make O(N) ?

Looking for the solution, the best answer from [stackoverflow][stackoverflow] was to use **pigeonhole principle**. I found a ready answer on [nkbits blog][nkbits-missing-integer] with that approach, and the author claims that solution should be O(N) in time and space complexity. When analyzing tha code it looks he is right, but after running demo test in codility, I still got O(N) or O(N * log(N))... Why? Is this Python language fault or Codility server error?

```python
def solution(A):
    N = len(A)
    count = [0]*(N + 1)
    
    # Counts all elements of A tha belongs to sequence {1, ..., N}
    for k in range(N):
        if N >= A[k] > 0:
            count[A[k]] += 1
    
    # Searches for the lesser integer that not belongs to A 
    for k in range(1, N + 1):
        if count[k] == 0:
            return k
    
    # If A has all elements from 1 to N, N + 1 is the minimal integer
    return N + 1
```

Results:

* Task score - 100%
* Correctness - 100%
* Performance - 100%

Detected time complexity: O(N) or O(N * log(N))

[Link to results][github-codility-results]

[stackoverflow]: https://stackoverflow.com/questions/25002381/missing-integer-variation-on-solution-needed
[codility-demo-test]: https://app.codility.com/demo/take-sample-test/
[nkbits-missing-integer]: https://blog.nkbits.com/2015/07/14/codility-missing-integer/
[github-codility-results]: https://app.codility.com/demo/results/demo5GNVDV-2NB/
