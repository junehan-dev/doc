HOW_TO_SPHINX_AND_DDD
=====================

| basically documents, easily test.
| Provide sphinx documents at https://codenamenadja.github.io/python-ddd/index.html

INDEX
-----

- :`folder structure`_: with intro.
- :`how to docstring`_: to recognize fine with sphinx.

folder structure
----------------

- :``project folder/`` root dir

   - :``setup.py`` file to automate setup project
   - :``my_package/`` package 1

      - :``__init__.py`` package recognization
      - :``factory.py`` sample class module
      - :``views.py`` sample function module
      - :``tests/`` test dir

         - :``test_factory_views.py`` sample testing scenario

   - :``settings/`` setting files


how to docstring
----------------

- before type annotations

.. code-block:: python

    def sample(arg1:int, arg2) -> bool:
        """Sample class for testing docstring.

        :param arg1: value to be ~ed.
        :param arg2: value to be ~ed.
        :type arg2: int

        :Keyword Arguments:
        :message: some keyword arg.(default to (,))

        :return: return will be...
        :return type: bool
        """