+++
title = "Python parametrize with scope"
date = "2021-07-23"
draft = true
tags = ['python', 'pytest']
+++

Intro

<!--more-->

Main part

```python

from random import randint
import pytest

# Creating new expensive object (calls to db, business rules calculation)
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

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_function(self, option, function_fixture):
        pass

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_class(self, option, class_fixture):
        pass

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_class_other(self, option, class_fixture):
        pass

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_module(self, option, module_fixture):
        pass

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_module_other(self, option, module_fixture):
        pass


class TestAppSecondTime:

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_app_class(self, option, class_fixture):
        pass

    @pytest.mark.parametrize('option', ['first', 'second'])
    def test_app_module(self, option, module_fixture):
        pass

```

Result

```bash
$ pytest -sv pytest-fixtures-test/tests

pytest-fixtures-test/tests/test_app.py::TestApp::test_function[first] Object: 2781723

pytest-fixtures-test/tests/test_app.py::TestApp::test_function[second] Object: 9731772

pytest-fixtures-test/tests/test_app.py::TestApp::test_class[first] Object: 3011073

pytest-fixtures-test/tests/test_app.py::TestApp::test_class[second] PASSED
pytest-fixtures-test/tests/test_app.py::TestApp::test_class_other[first] PASSED
pytest-fixtures-test/tests/test_app.py::TestApp::test_class_other[second] PASSED
pytest-fixtures-test/tests/test_app.py::TestApp::test_module[first] Object: 9155138

pytest-fixtures-test/tests/test_app.py::TestApp::test_module[second] PASSED
pytest-fixtures-test/tests/test_app.py::TestApp::test_module_other[first] PASSED
pytest-fixtures-test/tests/test_app.py::TestApp::test_module_other[second] PASSED
pytest-fixtures-test/tests/test_app.py::TestAppSecondTime::test_app_class[first] Object: 8088427

pytest-fixtures-test/tests/test_app.py::TestAppSecondTime::test_app_class[second] PASSED
pytest-fixtures-test/tests/test_app.py::TestAppSecondTime::test_app_module[first] PASSED
pytest-fixtures-test/tests/test_app.py::TestAppSecondTime::test_app_module[second] PASSED

```
---

Resources:

* [Visible text][some-name]

[some-name]: https://example.com
