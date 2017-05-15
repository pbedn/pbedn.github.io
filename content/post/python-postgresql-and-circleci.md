+++
title = "Deploy Python app using PostgreSQL to CircleCI"
tags = []
draft = false
date = "2017-05-15"
+++

Deploying to CircleCI as easy as creating yaml file (sometimes you even do not need it as Circle will automatically recognize structure and behavior). But usually it is better to setup at least basic configuration. The problem begins with more complicated functionality and CI documentation is minimal, or to say 'very hacker friendly'.

My problem was how to use build-in PostgreSQL database with Python application. This is quick tip how I solved it.
<!--more-->

I created simple Python app for this post purpose. All files are inside [my repository][my-circleci-repo], which I connected to Github with version 1.0 of CircleCI.

CircleCI settings are all default except *BUILD SETTINGS: Environment Variables*, where I added variable

    Name: DB_NAME
    Value: circleci

Create **circle.yml** file, and put inside root of repository


    machine:
      timezone:
        Europe/Warsaw
      python:
        version: 3.5.2

      environment:
        DATABASE_URL: postgres://ubuntu:@127.0.0.1:5432/circle_test

    dependencies:
      pre:
       - pip install pytest
       - pip install psycopg2

    database:
      override:
        - psql -U ubuntu -d circle_test -a -f schema.sql

    test:
      override:
        - py.test --junitxml=$CIRCLE_TEST_REPORTS/output.xml

The most time it took me to find correct configuration how to override database, and it turned out to be very simple.


    - psql -U ubuntu -d circle_test -a -f schema.sql

This line deciphered using [psql-docs]

* -U ubuntu: username
* -d circleci: database name
* -a echo all
* -f schema.sql: filename

If you look inside my repository at the [test_database.py][] file. You will find line which says:

    DATABASE = os.environ["DB_NAME"]

Here I am using that environmental variable, added in the beginning.

That is it. Now push to Github, and enjoy having automatic testing. If it still does not work, try to fork [my repo][my-circleci-repo] and play with it.

---

Links:

* [My repository with example app and configuration][my-circleci-repo]
* [CircleCI documentation about database setup][circleci-db-docs]
* [Psql documentation][psql-docs]


[my-circleci-repo]: https://github.com/pbedn/circleci-1.0-pytest-postgresql
[circleci-db-docs]: https://circleci.com/docs/1.0/manually/#databases
[psql-docs]: https://www.postgresql.org/docs/9.5/static/app-psql.html
[test_database.py]: https://github.com/pbedn/circleci-1.0-pytest-postgresql/blob/master/tests/test_database.py