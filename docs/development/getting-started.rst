Getting started
===============

Development dependencies
------------------------
Working on ``cryptography`` requires the installation of a small number of
development dependencies in addition to the dependencies for
:doc:`/installation`. These are listed in ``dev-requirements.txt`` and they can
be installed in a `virtualenv`_ using `pip`_. Before you install them, follow
the **build** instructions in :doc:`/installation` (be sure to stop before
actually installing ``cryptography``). Once you've done that, install the
development dependencies, and then install ``cryptography`` in ``editable``
mode. For example:

.. code-block:: console

    $ # Create a virtualenv and activate it
    $ # Set up your cryptography build environment
    $ pip install --requirement dev-requirements.txt
    $ pip install --editable .

You will also need to install ``enchant`` using your system's package manager
to check spelling in the documentation.

.. note::
    There is an upstream bug in ``enchant`` that prevents its installation on
    Windows with 64-bit Python. See `this Github issue`_ for more information.
    The easiest workaround is to use 32-bit Python for ``cryptography``
    development, even on 64-bit Windows.

You are now ready to run the tests and build the documentation.

OpenSSL on OS X
~~~~~~~~~~~~~~~

You must have installed `OpenSSL`_ via `Homebrew`_ or `MacPorts`_ and must set
``CFLAGS`` and ``LDFLAGS`` environment variables before installing the
``dev-requirements.txt`` otherwise pip will fail with include errors.

For example, with `Homebrew`_:

.. code-block:: console

    $ env LDFLAGS="-L$(brew --prefix openssl)/lib" \
        CFLAGS="-I$(brew --prefix openssl)/include" \
        pip install --requirement ./dev-requirements.txt

Alternatively for a static build you can specify
``CRYPTOGRAPHY_OSX_NO_LINK_FLAGS=1`` and ensure ``LDFLAGS`` points to the
absolute path for the `OpenSSL`_ libraries before calling pip.

.. tip::
    You will also need to set these values when `Building documentation`_.

Running tests
-------------

``cryptography`` unit tests are found in the ``tests/`` directory and are
designed to be run using `pytest`_. `pytest`_ will discover the tests
automatically, so all you have to do is:

.. code-block:: console

    $ py.test
    ...
    62746 passed in 220.43 seconds

This runs the tests with the default Python interpreter.

You can also verify that the tests pass on other supported Python interpreters.
For this we use `tox`_, which will automatically create a `virtualenv`_ for
each supported Python version and run the tests. For example:

.. code-block:: console

    $ tox
    ...
    ERROR:   py26: InterpreterNotFound: python2.6
     py27: commands succeeded
    ERROR:   pypy: InterpreterNotFound: pypy
     py33: commands succeeded
     docs: commands succeeded
     pep8: commands succeeded

You may not have all the required Python versions installed, in which case you
will see one or more ``InterpreterNotFound`` errors.


Explicit backend selection
--------------------------

While testing you may want to run tests against a subset of the backends that
cryptography supports. Explicit backend selection can be done via the
``--backend`` flag. This flag should be passed to ``py.test`` with a comma
delimited list of backend names.


.. code-block:: console

    $ tox -- --backend=openssl
    $ py.test --backend=openssl,commoncrypto

Building documentation
----------------------

``cryptography`` documentation is stored in the ``docs/`` directory. It is
written in `reStructured Text`_ and rendered using `Sphinx`_.

Use `tox`_ to build the documentation. For example:

.. code-block:: console

    $ tox -e docs
    ...
    docs: commands succeeded
    congratulations :)

The HTML documentation index can now be found at
``docs/_build/html/index.html``.

.. _`Homebrew`: http://brew.sh
.. _`MacPorts`: https://www.macports.org
.. _`OpenSSL`: https://openssl.org
.. _`pytest`: https://pypi.python.org/pypi/pytest
.. _`tox`: https://pypi.python.org/pypi/tox
.. _`virtualenv`: https://pypi.python.org/pypi/virtualenv
.. _`pip`: https://pypi.python.org/pypi/pip
.. _`sphinx`: https://pypi.python.org/pypi/Sphinx
.. _`reStructured Text`: http://sphinx-doc.org/rest.html
.. _`this Github issue`: https://github.com/rfk/pyenchant/issues/42
