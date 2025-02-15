=======
Version
=======

All CLI applications should have the basic ability to check the installed version; i.e.:

.. code-block:: console

   $ my-application --version
   7.5.8

By default, cyclopts parses the special flag ``--version``.

The resolution order for determining the version string is as follows:

1. An explicitly supplied version string to the root Cyclopts application:

   .. code-block:: python

      app = cyclopts.App(version="7.5.8")

2. The invoking-package's `defacto PEP8 standard`_ ``__version__`` string.
   Cyclopts attempts to derive the package module that instantiated the :class:`App <cyclopts.App>` object by traversing the call stack.

   .. code-block:: python

      # mypackage/__init__.py
      __version__ = "7.5.8"

      # mypackage/__main__.py
      # ``App`` will use ``mypackage.__version__``.
      app = cyclopts.App()

3. If (2) fails, then the default version string ``"0.0.0"`` will be displayed.

The ``--version`` flag can be changed to a different name(s) via the ``version_flags`` parameter.

.. code-block:: python

   app = cyclopts.App(version_flags="--show-version")
   app = cyclopts.App(version_flags=["--version", "-v"])

To disable the ``--version`` flag, set ``version_flags`` to an empty string or iterable.

.. code-block:: python

   app = cyclopts.App(version_flags="")
   app = cyclopts.App(version_flags=[])


.. _defacto PEP8 standard: https://peps.python.org/pep-0008/#module-level-dunder-names
