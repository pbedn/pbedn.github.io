+++
title = "Summary: Assignment Expressions - PEP 572"
date = "2019-07-28"
draft = false
tags = ['python', 'python 3.8+']
+++

The famous PEP 572, and one of the reasons Guido Van Rossum - Python BDFL - retired from decision process (stepped down as BDFL).
It is called "Assignment Expressions", and is about adding new syntax := that will allow assign to variables
within an expression.

<!--more-->

### Assignment Expressions Operator
First, let's look at the difference in Python between assignment statement and assignment expression:

Assignment statements are used to associate names of variables with values or to modify attributes of items in mutable objects. 

Assignment expression is a way to assign to variables within an expression, using the new operator := . 
It will look like this: *NAME := expression*. It can be used in almost all places where assignment statements are (using parentheses). They additionally work fine
in lambda functions and comprehensions. In terms of limits, there are cases where they are not supported, and even more,
cases where usage is not recommended, though possible.

#### SYNTAX
Assignement expression syntax examples:
```python
# Handle a matched regex
if (match := pattern.search(data)) is not None:
    # Do something with match

# A loop that can't be trivially rewritten using 2-arg iter()
while chunk := file.read(8192):
   process(chunk)

# Reuse a value that's expensive to compute
[y := f(x), y**2, y**3]

# Share a subexpression between a comprehension filter clause and its output
filtered_data = [y for x in data if (y := f(x)) is not None]
```

As we see, they might be useful to short required lines of code. It is familiar for people coming from, for example, C world.

#### VALID AND INVALID

Lets look at some valid  and invalid examples:
````python
y := f(x)  # INVALID
(y := f(x))  # Valid, though not recommended


y0 = y1 := f(x)  # INVALID
y0 = (y1 := f(x))  # Valid, though discouraged

foo(x = y := f(x))  # INVALID
foo(x=(y := f(x)))  # Valid, though probably confusing

def foo(answer = p := 42):  # INVALID
    ...
def foo(answer=(p := 42)):  # Valid, though not great style
    ...
    
def foo(answer: p := 42 = 5):  # INVALID
    ...
def foo(answer: (p := 42) = 5):  # Valid, but probably never useful
    ...

(lambda: x := 1) # INVALID
lambda: (x := 1) # Valid, but unlikely to be useful
(x := lambda: 1) # Valid
lambda line: (m := re.match(pattern, line)) and m.group(1) # Valid

>>> f'{(x:=10)}'  # Valid, uses assignment expression
'10'
>>> x = 10
>>> f'{x:=10}'    # Valid, passes '=10' to formatter
'        10'

def foo(answer = p := 42):  # INVALID
    ...
def foo(answer=(p := 42)):  # Valid, though not great style
    ...
    

````

#### SCOPE

An assignment expression does not introduce a new scope. It is using the current one.
And honors nonlocal or global declaration. A lambda is a scope for assignement expression.
The same rule is for comprehensions. Assignment expression binds the target in the containing scope.

Example how to capture and use later a variable inside any and all. 
```python
if any((comment := line).startswith('#') for line in lines):
    print("First comment:", comment)
else:
    print("There are no comments")

if all((nonblank := line).strip() == '' for line in lines):
    print("All lines are blank")
else:
    print("First non-blank line:", nonblank)
```

Example how to update a mutable state inside a comprehension. Here we compute a partial sums in list comprehension.
```python
total = 0
partial_sums = [total := total + v for v in values]
print("Total:", total)
```

### Controversy 

Up to this point, Guido Van Rossum as Python BDFL was in charge of accepting PEPs.
Of course, he had BDFL Delegates that were helping with PEPs and other stuff.
But all discussions and controversy made him tired of fighting for this PEP. 
As a result (at least partially) he stepped down as BDFL and removed himself from the decision process.
PEP 572 started discussions about new direction and leaders for the Python community.
And the result was a change of Python governance that is described more in [PEP 8016][link_to_pep8016].
However, Guido only temporarily lost his superpowers. 
A year later, on one of the conferences, he said that because Python is his child,
he cannot stay entirely outside. 
Currently, he serves as one of five Steering Council members, elected in February 2019.

### My Opinion

I have not a chance to use that operator, and will not for some time. But when I will, I think it might serve its purpose.
Python code (almost pseudocode) was always much shorter than other languages. That makes it even better.
After the first cry, developers will get used to and start to exploit its features.

---

References:

* [Link to PEP 572][link_to_pep572]
* [Link to PEP 8016][link_to_pep8016]
* [Dustin Ingram - PEP 572: The Walrus Operator - PyCon 2019][youtube_walrus]
* [PEP 572 and decision-making in Python][pep-mess]


[link_to_pep572]: https://www.python.org/dev/peps/pep-0572/
[youtube_walrus]: https://www.youtube.com/watch?v=6uAvHOKofws
[pep-mess]: https://lwn.net/Articles/757713/
[link_to_pep8016]: https://www.python.org/dev/peps/pep-8016/
