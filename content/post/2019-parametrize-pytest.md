+++
title = "Parametrizing tests with pytest"
date = "2019-08-21"
draft = false
tags = ['python', 'pytest']
+++

In pytest we can parametrize in three different ways:

 * *pytest.fixture()* - allows to parametrize fixture functions
 * *@pytest.mark.parametrize* - allows to define arguments for a test function or class
 * *pytest_generate_tests* - allows to define custom parametrization schemes

This article presents a few examples of how to use parametrization in pytest
using *@pytest.mark.parametrize*.
The article starts from the basic tests, and end with the usage of ids and indirect.

<!--more-->

#### Basic example
Let's start with a basic example, and we will build on that in more advanced cases.
This example is rather easy, our *test_power_of_two* passes parameter 
*test_input* to our function *power_of_two* argument x. 

```python
import pytest

def power_of_two(x):
    return 2 ** x
    
@pytest.mark.parametrize("test_input, expected", [
    (2, 4),
    (4, 16),
])
def test_power_of_two(test_input, expected):
    assert power_of_two(test_input) == expected
```

#### Named test with ID
We can set a test ID for each set of values in the test. When invoking our test
with *--collect-only* (assumed our file is named test.py) we get:

```python
>>> pytest test.py --collect-only

collecting ... collected 2 items
<Module test.py>
  <Function test_power_of_two[2-4]>
  <Function test_power_of_two[4-16]>
```

However, when we use *ids* option in parametrize, we can specify returned 
name of the test, that can be used later for calling selected test with *-k*.

```python
import pytest

def power_of_two(x):
    return 2 ** x
    
@pytest.mark.parametrize("test_input, expected", [
    (2, 4),
    (4, 16),
], ids=['two', 'four'])
def test_power_of_two_ids(test_input, expected):
    assert power_of_two(test_input) == expected
```

```python
>>> pytest test.py --collect-only

collecting ... collected 2 items
<Module test.py>
  <Function test_power_of_two_ids[two]>
  <Function test_power_of_two_ids[four]>
```

#### Deferring the setup of expensive resources
The parametrization of the test function happens at collection time. 
That is why we can defer expensive resource to be processed only at the actual test run. To do that we need to specify fixture with our function and, unfortunately, if we like to use 'expected' another fixture for it.
Let's see the examples. The first time we run collect only and second time running the test.
We can see that our expensive function is called only during the test.

```python
@pytest.fixture(scope="function")
def expensive_power_of_two(request):
    print('\nCalling expensive function')
    return 2 ** request.param


@pytest.fixture(scope="function")
def expected(request):
    print('Calling expensive expected')
    return request.param


@pytest.mark.parametrize("expensive_power_of_two, expected", [
    (2, 4), 
    (4, 16),
], ids=['two', 'four'], indirect=True)
def test_power_of_two_exp(expensive_power_of_two, expected):
    assert expensive_power_of_two == expected
```

```python
>>> pytest test.py --collect-only

collecting ... collected 2 items
<Module test.py>
  <Function test_power_of_two_exp[two]>
  <Function test_power_of_two_exp[four]>
```

```python
>>> pytest test.py -vv

collecting ... collected 2 items

test.py::test_power_of_two_exp[two] 
Calling expensive function
Calling expensive expected
PASSED                               [ 50%]
test.py::test_power_of_two_exp[four] 
Calling expensive function
Calling expensive expected
PASSED                              [100%]
```

#### Apply indirect on selected arguments

We can apply 'indirect' keyword to only selected arguments, by passing list or tuple
of arguments names to indirect. 

```python
@pytest.mark.parametrize("expensive_power_of_two, expected", [
    (2, 4),
    (4, 16),
], ids=['two', 'four'], indirect=['expensive_power_of_two'])
def test_power_of_two_exp_sel(expensive_power_of_two, expected):
    assert expensive_power_of_two == expected
```

```python
>>> pytest test.py -vv

collecting ... collected 2 items

test.py::test_power_of_two_exp_sel[two] 
Calling expensive function
PASSED                           [ 50%]
test.py::test_power_of_two_exp_sel[four] 
Calling expensive function
PASSED                          [100%]
```

More examples are in pytest documentation about parametrize. See references.

---



References:

* [Pytest documentation about parametrize][pytest_doc_parametrize]

[pytest_doc_parametrize]: http://doc.pytest.org/en/latest/parametrize.html
