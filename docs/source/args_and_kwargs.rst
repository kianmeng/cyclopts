.. _Args & Kwargs:

=============
Args & Kwargs
=============

In python, a function can consume a variable number of positional and keyword arguments:

.. code-block:: python

   def foo(normal_required_variable, *args, **kwargs):
       pass

There is **nothing special** about the names ``args`` and ``kwargs``;
the functionality is derived from the leading ``*`` and ``**``.
``args`` and ``kwargs`` are the defacto standard names for these variables.
In this document, we'll usually just refer to them as ``*args`` and ``**kwargs``.

Cyclopts commands may consume a variable number of positional and keyword arguments.
The priority ruleset is as follows:

1. ``--keyword`` CLI arguments first get matched to normal variable parameters.

2. Unmatched keywords get consumed by ``**kwargs``, if specified.

3. All remaining tokens get consumed by ``*args``, if specified.
   A prevalant use-case is in a typical :ref:`Meta App`.

--------------------------
Args (Variable Positional)
--------------------------
A variable number of positional arguments consume all remaining positional arguments from the command-line.
Individual elements are converted to the annotated type.

.. code-block:: python

   @app.command
   def foo(name: str, *favorite_numbers: int):
       print(f"{name}'s favorite numbers are: {favorite_numbers}")


.. code-block:: console

   $ my-script foo Brian
   Brian's favorite numbers are: ()

   $ my-script foo Brian 777
   Brian's favorite numbers are: (777,)

   $ my-script foo Brian 777 2
   Brian's favorite numbers are: (777, 2)

--------------------------
Kwargs (Variable Keywords)
--------------------------
A variable number of keyword arguments consume all remaining CLI tokens starting with ``--``.
Individual values are converted to the annotated type.
As with normal python ``**kwargs``, the keywords are limited to python identifiers.
Most prominently, no spaces allowed.
Keyword name-conversion is the :ref:`same as commands <Changing Name>`.

.. code-block:: python

   @app.command
   def add(**country_to_capitols):
       for country, captiol in country_to_capitols.items():
           print(f"Adding {country} with captiol {capitol}.")


.. code-block:: console

   $ my-script add --united-states="Washington, D.C." --canada=Ottawa
   Adding united_states with captiol Washington, D.C..
   Adding canada with captiol Ottawa.
