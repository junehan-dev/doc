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
   | use the "Show Source" link in the sidebar

.. |RST| text:: restructured text
