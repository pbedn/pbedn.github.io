+++
title = "Summary: Python Positional-Only Parameters - PEP 570"
date = "2019-06-07"
draft = false
tags = ['python', 'python 3.8+']
+++

In Python, we have positional and keywords arguments. With the introduction of new syntax in PEP 570,
we can specify positional-only parameters in Python function definitions.
This feature is especially useful for library developers, to ensure
correct usage of an API. Let's discuss what this PEP is all about.

<!--more-->

### First some background

We should understand the difference between positional and keyword arguments.

**Positional arguments:**

1) Appear at the beginning of a function argument list in the format *arg=value*

2) And/Or they could be passed as elements of an iterable preceded by *

```python
def function_arg(a, b, c):
    return a, b, c

# example combinations
function_arg(1, 2, 3)     # a)
function_arg(*(1, 2, 3))  # b)
function_arg(1, *(2, 3))  # b)
```

**Keyword arguments:**

1 Are arguments preceded by an identifier in the format *kwarg=value*

2) And/Or passed as a value in a dictionary preceded by **

```python
def function_kwarg(a=None, b=None, c=None):
    return a, b, c

# example combinations
function_kwarg(a=5, b=10, c=15)               # a)
function_kwarg(b=10, c=15, a=5)               # a)
function_kwarg(**{'a': 5, 'b': 10, 'c': 15})  # b)
function_kwarg(5, 10, c=15)                   # b)
function_kwarg(5, 10, **{'c': 15})            # b)
function_kwarg(5, b=10, **{'c': 15})            # b)

```

Typical beginner mistake is to put the keyword argument before the positional in the function; then we get SyntaxError.

```python
f(a=1, 2, 3)
..
    f(a=1, 2, 3)
          ^
SyntaxError: positional argument follows keyword argument
```

We can use any number of positional and keyword arguments. Here I use idiom name *args* for positional and *kwargs* for keyword arguments.

```python
def function_args_kwargs(*args, **kwargs):
    return args, kwargs

function_args_kwargs(1, 2, 3, a=5, b=10, c=15)
```

If we want to use keyword-only arguments without unlimited positional arguments, we can use empty * after positional arguments.

```python
def function_limited_pos_and_kwargs(a, *, b=None, c=None):
    return a, b, c

>>> function_limited_pos_and_kwargs(1, b=2, c=3)
(1, 2, 3)
>>> function_limited_pos_and_kwargs(1, 2, c=3) # but this returns error
...
TypeError: function_limited_pos_and_kwargs() takes 1 positional argument but 2 positional arguments (and 1 keyword-only argument) were given
```

To recap this part, old function definition grammar looks like:

    def function_name(positional_or_keyword_parameters, *, keyword_only_parameters):

### PEP 570

This PEP introduces new syntax, /, for specifying positional-only parameters. If we read the internal docs, we could already see such sign in some function definitions, but as noted it was only a documentation convention. Currently, the new function definition grammar looks like:

    def function_name(positional_only_parameters, /, positional_or_keyword_parameters,
         *, keyword_only_parameters):

As we can see, all parameters left to the slash are positional-only, and after slash, we use previous rules.

Lets look at some valid  and invalid function definitions:

Valid:
```python
function(p1, p2, /, p_or_kw, *, kw)
function(p1, p2=None, /, p_or_kw=None, *, kw)
function(p1, p2=None, /, *, kw)
function(p1, p2=None, /)
function(p1, p2, /, p_or_kw)
function(p1, p2, /)

function(p_or_kw, *, kw)
function(*, kw)
```

Invalid:
```python
function(p1, p2=None, /, p_or_kw, *, kw)
function(p1=None, p2, /, p_or_kw=None, *, kw)
function(p1=None, p2, /)
```

And a few examples:

```python
def function(a, b, /, c, d=None, *, e=None):
    return a, b, c, d, e
    
>>> function(1, 2, 3, 4, 5)
...
TypeError: function() takes from 3 to 4 positional arguments but 5 were given

>>> function(1, 2, 3, 4, e=5)
(1, 2, 3, 4, 5)

>>> function(1, 2, 3, c=4, e=5)
...
TypeError: function() got multiple values for argument 'c'

>>> function(1, 2, 3, d=4, e=5)
(1, 2, 3, 4, 5)

>>> function(1, 2, c=3, d=4, e=5)
(1, 2, 3, 4, 5)

>>> function(1, b=2, c=3, d=4, e=5)
...
TypeError: function() got some positional-only arguments passed as keyword arguments: 'b'
```

We have to remember that / and * are optional. When they are not present in a function definition,
arguments may be passed by a position or by keyword.

Finally, two guidelines from the official documentation:

    
1) Use positional-only if names do not matter or have no meaning, and there are only a few arguments which will always be passed in the same order.
   
2) Use keyword-only when names have meaning, and the function definition is more understandable by being explicit with names.

---

References:

* [Link to PEP 570][link_to_pep570]
* [Trey Hunner post about keyword arguments][link_to_trey_blog]


[link_to_pep570]: https://www.python.org/dev/peps/pep-0570/
[link_to_trey_blog]: https://treyhunner.com/2018/04/keyword-arguments-in-python/
