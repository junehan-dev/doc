Phinx RST 
=========

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   configuration
   primer
   contents/directives
   contents/field_lists

Defining_document_structure
^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. you did setup with sphinx-quickstart.
#. ``conf.py``, ``master document``, ``index.rst`` generated as core

   - ``master document``: serves welcome page, and contain root of the table of content tree ``TOCTree``

#. Toctree directive initially empty, and looks like,

--------

TOCTREE_SAMPLE
   .. code-block:: rst

      .. toctree::
         :maxdepth: 2

         folder_at_conf/installation
         folder_at_conf/quickstart

--------

이것은 정확히 sphinx에서 규정하는 toctree이다.
포함해야할 문서는 document names로써 주어진다. 요약하면, 파일 확장자는 신경끄고, 슬래쉬를 사용하여 dir-seperate만 해주면 된다.
their section titles will be inserted(maxdepth레벨에 따라) at place where the toctree directive is placed.
also, sphinx now know avout the order and hierarchy of your documents.

Adding_content
^^^^^^^^^^^^^^

in sphinx ``source`` directory files, you can use most features of standard |RST|.
there are several features added by sphinx.
for example, you cna add cross-file references in portable way using ``ref`` role.

   if you are viewing the HTML version, you can look at the source for this coument
   use the "Show Source" link in the sidebar

.. |RST| replace:: restructured_text


Running_the_build
^^^^^^^^^^^^^^^^^

guess you added some files and content, A build is startd with sphinx-build program

.. code-block:: bash

   sphinx-build -b html sourcedir builddir

sphinx-build-OPTIONS_
^^^^^^^^^^^^^^^^^^^^^

- ``-b buildername``

   desc
      selects a builder
   Positional argument[buildername]
      - :html: default builder
      - :dirhtml: build html but sith single directory per-document, make for prettier URLS if served from a webserver
      - :man: manual pages in groff format for UNIX systems.
      - :singlehtml: single html with whole content
      - :doctext: run all doctests in documentation, if doctest extension is enabled

- ``-M buildername``

   desc
      Alternative to ``-b``, Uses sphinx ``make_mode``.

.. _sphinx-build-OPTIONS: https://www.sphinx-doc.org/en/master/man/sphinx-build.html#cmdoption-sphinx-build-b

Documenting Objects
^^^^^^^^^^^^^^^^^^^

one of main objective of sphinx is *documenting of objects in any domain.*

domain
   A Collection of object types that belong together,
   complete with markup to create and reference descriptions of there objects.

most prominent domain is Python domain, example in below...

.. code-block:: rst

   .. py:function:: enumerate(sequence [, start=0])

   Returns an Iterator that yields tuple of an index and it's value as a sequence


.. py:function:: enumerate(sequence [, start=0])

   Returns an Iterator that yields tuple of an index and it's value as a sequence

there are more directives for documeting other types of python obj.
   - ``py:class``
   - ``py:method``
   - ``py:fuction``

there is also cross-referencing role for each of there object types.
*this markup will create a link to documentation of ``enumerate()``*

   the :py:func:`enumerate` function can be used for ...

if Python domain is the defaultone, ``py:`` can be ommited.
and also doesn't matter which file contains the actual doc for directive.

Each domain will have special rules for..
   - how the signatures can look like,
   - and make the fommated output look pretty
   - or add specific features like links to parameter types, e.g, c/c++ domains.

Basic configuration
^^^^^^^^^^^^^^^^^^^^

since ``conf.py`` is executed by sphinx, you can do non-trival tasks in it,
like extending ``sys.path`` or importing a module to find out the version you are documenting.

.. 너는 하찮지 않은 태스크를 그 안에 할 수 있다.

Autodoc
^^^^^^^

python conventially documents really a alot of docstrings.
so, use extension called ``autodoc``.

to activate it. in ``conf.py`` put the string ``sphinx.ext.autodoc`` into ``extensions``  config value.

.. code-block:: py

   extension = ['sphinx.ext.autodoc']

the, needs to add few additional directives at  your disposal.
for examlple, to document the fuction ``io.open()``, reads its signature and docstring from the source file, like,

.. code-block:: rst

   .. autofunction:: io.open

you can aslo document whole classes or even modules automatically,
using member options for auto directives, like,

.. code-block:: rst

   .. automodule:: io
      :members:

``autodoc`` needs to import your modules in order to extract docstrings.
therefore, you must add approrpicate path to ``sys.path`` in your ``conf.py``

.. WARNING::
   autodoc imports the modules to be documented.

Intersphinx
^^^^^^^^^^^

When you wnat to make links to such sphinx-documents on internet on your doc,
you can do it with ``sphinx.ext.Intersphinx``

to use them, activate it in conf.py by 'sphinx.ext.intersphinx' to extensions list and,
*set up the ``intersphinx_mapping* config value.

for examlple, to lint to ``io.open()`` int the python lin intersphinx_mapping,
setup your ``intersphinx_mapping`` like:

   .. code-block:: py

      intersphinx_mapping = {'python': ('https://docs.python.org/3', None)}

and now, you canr write cross-reference like :py:func:`io.open`.
Any cross-ref that has no match will be looked up in documentation sets configured in ``intersphinx_mapping``
also works for some other domain's rols including ``:ref:``. not only python...
but doesnt work for ``:doc:`` as that is non domain role.
