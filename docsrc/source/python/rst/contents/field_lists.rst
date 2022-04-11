Field Lists
===========

field lists는 아래처럼 마크업 되는 필드들의 연속성들 이다.

.. code-block:: rst

   :fieldname: Field Content

.. note::

    필드 리스트들의 values는 문자열로 해석된다. 파이썬 자료구조중 배열이나, 사전들은 사용 불가

File-wide metadata
------------------

파일 상단에 필드 리스트는, 대개 docutils에 의해 docinfo로 해석되어 페이지에 보여진다.
그러나 sphinx에서, 어떤 마크업에 붙는 필드리스트는, docinfo에서 sphinx env로 이동하여 document metadata로 인식한다.
그리고 아웃풋에 출력되지 않는다.

.. note::

   문서 타이틀 이후에 보이는 필드 리스트는 docinfo의 부분으로 아웃풋으로 출력될 것이다.

Special metadata fields
-----------------------

sphinx는 원형의 필드들을 위해 커스텀 필드들을 제공한다.
그 아래서 인식되는 **metadata fields** 는::

   tocdepth
      the maximun depth for a table of contents of this file.

   nocomments
      set될 경우, web app은 comment form을 보여지도록 생성하지 않을 것이다.

   orphan
      set될 경우, 이 파일에 대한 warnings는 어떤 toctree에도 포함되지 않을 것이다.

   nosearch
      set될 경우, 이 파일에 대한 텍스트 search가 비허용 될 것이다.

.. code-block:: rst

   :tocdepth: 2
   :nocomments:
   :orphan:
   :nosearch:

.. note::

   이 메타데이터는 local toctree의 depth에 영향을 준다.
   그러나 이것은 global toctree에 영향을 주지 않는다.
   따라서, global이 사용하는 sidebar에는 영향이 없다.
    
