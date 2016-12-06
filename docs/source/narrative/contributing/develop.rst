===================
Developing Websauna
===================

.. contents:: :local:

Install from Github
-------------------

Create a virtualenv and install the latest master from Github using pip.

Running tests
-------------

Unit tests are `PyTest based <http://pytest.org/>`_. They use `Selenium browser automation framework
<http://selenium-python.readthedocs.org/>`_ and `Splinter simplified element interatcion
<https://splinter.readthedocs.org/en/latest/>`_.

First test run
++++++++++++++

To run tests you need to install development and tests dependencies:

.. code-block:: console

    pip install "websauna[dev,test,celery]"
    # or in case of local development of websauna
    pip install ".[dev,test,celery]"

The other thing is that you need to have `phantomjs` in your system as well, to install it follow instructions from http://phantomjs.org/download.html.

Prepare wheel archive which will speed up scaffold tests tremendously::

     bash websauna/tests/create_wheelhouse.bash

Create ``setup-test-secrets.bash`` (git ignored) with following content::

    RANDOM_VALUE="x"
    FACEBOOK_CONSUMER_KEY="x"
    FACEBOOK_CONSUMER_SECRET="x"
    FACEBOOK_USER="x"
    FACEBOOK_PASSWORD="x"

    export RANDOM_VALUE
    export FACEBOOK_CONSUMER_KEY
    export FACEBOOK_CONSUMER_SECRET
    export FACEBOOK_USER
    export FACEBOOK_PASSWORD

Enable it in your shell::

    source setup-test-secrets.bash

Running all tests silently using a headless test browser::

    py.test websauna --splinter-webdriver=phantomjs --splinter-make-screenshot-on-failure=false --ini=test.ini


Tox
---

:term:`Tox` is used to run tests against multiple versions of Python.

To run tests locally using tox:

.. code-block:: console

    tox -- --ini=websauna/conf/test.ini

More examples
-------------

Run tests using Tox. Here is a Tox run using Python 3.5 and Chrome:

.. code-block:: console

     tox -e py35 -- --ini=websauna/conf/test.ini -x --splinter-webdriver=chrome

Running a single test case with pdb breakpoint support:

.. code-block:: console

    py.test -s --ini=test.ini --splinter-webdriver=phantomjs -k test_login_inactive

Running functional tests with an alternative browser:

.. code-block:: console

    py.test --splinter-webdriver=firefox websauna/tests/test_frontpage.py --ini=test.ini


