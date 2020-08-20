HOW_TO_SPHINX
=============

| basically documents, easily test.
| Provide sphinx documents at https://codenamenadja.github.io/sphinx_test/index.html

INDEX
-----

- TocTree_
- Entries_ 

.. _TocTree: https://codenamenadja.github.io/sphinx_test/index.html

toctree
-------
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
