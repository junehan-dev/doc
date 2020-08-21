Intro
=====

Defining_document_structure
---------------------------

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
--------------

in sphinx ``source`` directory files, you can use most features of standard |RST|.
there are several features added by sphinx.
for example, you cna add cross-file references in portable way using ``ref`` role.

   if you are viewing the HTML version, you can look at the source for this coument
   use the "Show Source" link in the sidebar

.. |RST| replace:: restructured_text


Running_the_build
-----------------

guess you added some files and content, A build is startd with sphinx-build program

.. code-block:: bash

   sphinx-build -b html sourcedir builddir

sphinx-build-OPTIONS_
---------------------

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
-------------------

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
