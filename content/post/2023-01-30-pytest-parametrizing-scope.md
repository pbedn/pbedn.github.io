+++
title = "Using scopes in Pytest fixtures"
date = "2023-01-30"
draft = false
tags = ['python', 'pytest']
+++

When we have time-expensive functionality in our fixtures, it is good to know when they are invoked and run.
Although this should be intuitive, let's test on a simple example.

<!--more-->

From pytest documentation:

> Fixtures are created when first requested by a test, and are destroyed based on their scope:
>
> - function: the default scope, the fixture is destroyed at the end of the test.
> - class: the fixture is destroyed during teardown of the last test in the class.
> - module: the fixture is destroyed during teardown of the last test in the module.
> - package: the fixture is destroyed during teardown of the last test in the package.
> - session: the fixture is destroyed at the end of the test session.

Our naive fixtures have will print random integers from the range 1 to 10e6, not because this is expensive, but just to see different outputs. Let's test three of them most commonly used: function, class and module. Side note - we do not have to explicitly write *scope="function"*, as this is the default, and can be left empty.

```python

from random import randint
import pytest

# Creating new expensive objects (calls to db, network, business rules calculation etc.)
@pytest.fixture
def function_fixture():
    print(f'Object: {randint(1, 10e6)}')

@pytest.fixture(scope='class')
def class_fixture():
    print(f'Object: {randint(1, 10e6)}')

@pytest.fixture(scope='module')
def module_fixture():
    print(f'Object: {randint(1, 10e6)}')


class TestApp:

    # FUNCTION FIXTURE
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_function(self, option, function_fixture):
        print("test_function")

    # CLASS FIXTURE
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_class(self, option, class_fixture):
        print(f"test_class {id(class_fixture)}")

    # CLASS FIXTURE - Second call
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_class_other(self, option, class_fixture):
        print("test_class_other")

    # MODULE FIXTURE
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_module(self, option, module_fixture):
        print("test_module")

    # MODULE FIXTURE - Second call
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_module_other(self, option, module_fixture):
        print("test_module_other")


class TestAppSecondTime:

    # CLASS FIXTURE
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_app_class(self, option, class_fixture):
        print("test_app_class")

    # MODULE FIXTURE
    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_app_module(self, option, module_fixture):
        print("test_app_module")

```

Result

```bash
$ pytest -sv

test_app.py::TestApp::test_function[first] Object: 2781723
test_app.py::TestApp::test_function[second] Object: 9731772

test_app.py::TestApp::test_class[first] Object: 3011073
test_app.py::TestApp::test_class[second] PASSED
test_app.py::TestApp::test_class_other[first] PASSED
test_app.py::TestApp::test_class_other[second] PASSED

test_app.py::TestApp::test_module[first] Object: 9155138
test_app.py::TestApp::test_module[second] PASSED
test_app.py::TestApp::test_module_other[first] PASSED
test_app.py::TestApp::test_module_other[second] PASSED

test_app.py::TestAppSecondTime::test_app_class[first] Object: 8088427
test_app.py::TestAppSecondTime::test_app_class[second] PASSED
test_app.py::TestAppSecondTime::test_app_module[first] PASSED
test_app.py::TestAppSecondTime::test_app_module[second] PASSED

```

As we can see class fixture, though called each two times for TestApp, was initialised only once, thus saving us an expensive run. Similarly, the module fixture was initialized only once per two classes.

Conclusions: 

* Fixture with Function Scope is initialized each time we call it.
* Fixture with Class Scope is initialized once per class.
* Fixture with Module Scope is initialized only once.

That was the easy part. What is worth remembering, is that:

* Fixtures can be requested more than once per test - return values are cached.
* The parametrization of test functions happens at collection time.

---

Resources:

* [Pytest reference - Scope][pytest-scope]

[pytest-scope]: https://docs.pytest.org/en/7.1.x/how-to/fixtures.html#scope-sharing-fixtures-across-classes-modules-packages-or-session
