Directives
==========

Directives는 명시적인 마크업의 기본 블록이다.
docutils가 다수의 Directives를 제공하는 반면, sphinx는 더 많은 것을 제공하며, Directives를 주요한 확장 매커니즘의 하나로 사용한다.

`Domains`_ 문서를 보면, domains에 의해 더해진 roles를 볼 수 있다.

.. _Domains: https://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html

.. tip::

   restructuredtext Primer문서에 docutils에 의해 제공되는 Directives를 확인할 수 있다.



Table of contents
-----------------

reST가 일부 문서들의 연결하기위한 빌트인을 제공하지 않기 때문에, 또는
문서를 여러 출력파일로 나누지 않기 떄문에, sphinx는 커스텀-Directives를 사용하여,
개별 파일들을 문서로 구성하는 관계를 더한다. 마찬가지로 tables of contents도 그렇다.
**toctree** Directives는 중앙에 있는 요소이다.

.. note::

   다른 파일에서 하나의 파일을 단순한 "inclusion"하는 것은 ``include`` Directives를 통해 가능하다.

.. note::

   현재 문서를 위한 contents table을 생성하기 위해서, reST의 표준 ``contents Directive`` 를 사용하면 된다.

toctree
^^^^^^^

이 Directive는 "TOC tree"를 현재 위치에 삽입한다.
directive body에 주어진 문서의 개별적인 TOCs("sub-TOC trees"를 포함한)를 사용하여 그리한다.
관계적인 문서이름들('/'로 시작하지 않는,)는 directive가 일어나는 문서에 대해 상대적이다.
절대경로 이름들은, source디렉터리에 대해 상대적이다. 숫자인 **maxdepth** 옵션은 아마, tree의 깊이를 가리키기 위해 주어진다.;
default로는 모든레벨이 포함된다.

"TOC tree"의 대표는 개별 아웃풋 포멧에 따라 달라진다.
빌더가 다수의 파일을 아웃풋으로 한다면, 그것을 하이퍼링크의 컬렉션으로 취급할 것이다.
반면, 빌더가 하나의 파일만을 출력한다면, 그것을 문서의 content로 TOC tree에 있는 것을 넣을 것이다.

이 예제를 보면,(파이톤 docs의 라이브러리 레퍼런스 인덱스에서 취한 것이다.)::

   .. toctree::
      :maxdepth: 2

      intro
      strings
      datatypes
      numbric
      (and more...)

이것은 2가지를 수행한다:

   - Table of contents from thos docments are inserted, with a maximum depth of two, that means one nested heading. toctree directives in those doucment are also taken into account
   - Sphinx knows the relative order of the documents. *intro*, *strings* and so forth, and it knows that they are children of the shown document, the library index. From this information it generates "next chapter", "previous chapter" and "parent chapter" links.

Entries
^^^^^^^

Document titles in toctree는 자동적으로 referenced document의 title을 읽어올 것입니다.
만약 그것이 당신이 원한행동이 아니라면, 직접 명시적으로 title과 target을 similar syntax to reST hyperlinks를 작성하세요.
(그리고 sphinx의 `cross-referencing syntax`_ 도요). 이는 아래처럼 보입니다::

   .. toctree::

      intro
      All about strings <strings>
      datatypes

.. _cross-referencing syntax: https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#xref-syntax

두번째 줄은 *strings* 문서 에 대한 링크입니다. 그러나 All about strings를 strings의 title을 대신해서 사용합니다.
직접 명시적인 링크를 줄 수도 있습니다. 문서이름이 아니라 HTTP URL을 부여할 수도 있죠

Section numbering
^^^^^^^^^^^^^^^^^

만약 섹션 번호가 html output에도 있길 바란다면, **toplevel** toctree를 *numbered* 옵션을 주세요.
예를 들면::

   .. toctree::
      :numbered:

      foo
      bar

번호매김은 foo의 헤딩에서 부터 시작될 겁니다. sub-toctree는 자동적으로 번호매김이 됩니다.
(그들에게는 *numbered* 옵션을 주면 안되요.)

특정 depth까지만 번호매김을 적용하는 것도 가능합니다. depth를 numeric argement로 numbered옵션에 주면 처리 될겁니다.

Additional options
^^^^^^^^^^^^^^^^^^

- caption
- name
- titlesonly
- glob
- hidden
- reversed
- includehidden

*caption* 옵션을 사용하여 toctree caption을 줄 수 있습니다. 그리고 *name* 옵션을 사용하여,
내부적인 타겟 명을 주면 그것은 *ref* 를 통해 referenced 될 수 있습니다::

   .. toctree::
      :caption: Table of contents
      :name: mastertoc

      foo

만약 tree안에 있는 documents의 title만이 보여지길 원한다면, (동일 레벨의 다른 내부 헤딩을 제외하고,), *titlesonly* 옵션을 사용할 수 있습니다::

   .. toctree::
      :titlesonly:

      foo
      bar

"globbing"을 toctree directives에서 사용할 수 있습니다.
*glob* flag-옵션을 주는 걸 통해서요.
모든 엔트리들은 이후 가능한 문서들의 리스트에 대해서 매치될 것이고,
매치된 것들은 alphabet순으로 리스트화 될 것 입니다. 예를 들면::

   .. toctree::
      :glob:

      intro*
      recipe/*
      *

이것은 ``intro`` 라는 이름으로 시작되는 모든 documents를 포함할 것 입니다.
그리고 ``recipe`` 폴더 안의 모든 문서들,
마지막으로 남은 모든 문서들(이미 포함된 것은 제외)을 포함할 것입니다.

Special names
^^^^^^^^^^^^^

Sphinx는 일부 document이름들을 특정 용도를 위해 예약해 두었습니다.
따라서 이 이름들은 이미 수행하는 역할이 있기 때문에 네임스페이스를 존중해주어야합니다.
문제를 일으킬거니까요:)::

   - *genindex*, *modindex*, *search*
   - every name beginning with *_* 

.. warning::

   흔하지 않은 문자를 파일명으로 쓰는 것은 그만두세요, 일부 포멧들은 이 문자들을 예상치 못한 방향으로 해석합니다.

      - Do not use colon *:* for HTML based formats. link to other part may not work.
      - Do not use plus *+* for ePub format. some resources may not be found

Paragraph-level markup
----------------------

.. tip::

   directive는 docutils것이 기반이고, 우리는 지금 Sphinx의 directives를 보고 있는거 아시죠?

- note
- warning
- versionadded + vesion

   .. versionadded:: 2.5b
      The *spam* parameter.

- versionchaged + version

   .. versionchanged:: 2.5r
      The *spam* parameter.


- deprecated + version

   .. deprecated:: 3.1
      Use :func:`spam` instead.

- seealso

   .. seealso::

      Module :py:mod:`zipfile`
         Documentation of the :py:mod:`zipfile` standard module.

      `GNU tar manual, Basic Tar Format <http://link>`_
         Documentation for tar archive files, including GNU tar extensions.

- rubric + title
- centered

   .. centered:: LICENSE AGREEMENT

- hlist

   .. hlist::
      :columns: 3

      * A list of
      * short items
      * that should be
      * displayed
      * horizontally

Showing code examples
---------------------

코드를 syntax-highlighted 되게 보여주는 방법은 4가지가 있다.

- using reST *doctest* blocks 
- using reST *literal* blocks, optionally in combination with *highlight* directive
- using *code-block* directive
- using *literalinclude* directive

Doctest블록은 interactive-파이썬-session을 보여줄 때만 가능하고, 반면 남아있는 세개는 다른 언어에 적용가능하다.

literal blocks는 모든 문서나 커다란 섹션규모에서 유용하다. 그냥 code blocks를 사용하면 동일하게 보여질 것이다.
반면, *code-block* directive는 한 문서에서 여러가지 코드를 해당 syntax에 맞춰서 보여줄 때 더 적합하게 구성되어 있다.
마지막으로 *literalinclude* directive는 코드파일 자체를 문서에 포함할 때 유용하다.

이 모든 방식에서 syntax highlight는 `Pygments`_ 에서 가져온 것이다.
문자열 블록을 만났을 때, highlight directive를 해당 소스파일에서 설정하는 것으로 설정된다.
만약 highlight directive에 진입하면, 다음 highlight directive를 만나기 전까지 사용된다.
해당 파일에 highlight directive가 없으면, global highlighting language가 사용된다.
기본적으로 파이톤으로 설정되어 있으나 *highlight_language* config value를 수정하는 것으로 전환 가능하다.
아래 value들이 config에 적용될 수 있다::

   - none (no highlighting)
   - default (similar to python3)
   - guess (let *Pygments* guess the lexer based on contets, only works with well-recongnizable langs)
   - python
   - rest
   - c
   - ... and any other lexer alias that Pygments supports

.. _Pygments: https://pygments.org/

만약 설정한 언어의 highlight가 실패하면 (Pygments emits an "error" token), 해당 블록은 전혀 highlight되지 않을 것이다.

.. important::

   list of lexer aliases supports is tied to Pygment version. check the version of Pygments!

highlight
^^^^^^^^^

exmple::

   .. highlight:: c
      :linenothreshold: 5

options

   - ``:linenothreshold: threshold (number [optional])``
   
      Enable gernerate line numbers for code blocks.
      directive will produce line numbers only for the code blocks longer than n lines

code-block
^^^^^^^^^^

examples::

   .. code-block:: ruby
      :linenos:
      :lineno-start: 10
      :emphasize-lines: 3,5
      :caption: this.ruby
      :name: this-ruby 
      :dedent: 4

          some Ruby code.

.. versionchanged:: 2.0:
     The language argument becoms optional.

   options
   
   - ``:linenos: (no value)``
      Enables to generate line numbers for code block

   - ``:lineno-start: number (number)``
      set the first line number of the code block. if present, ``linenos`` option is also automatically activated

   - ``:emphasize-lines: line numbers(comma seperated numbers)``
      emphasize particular lines of the code block

   - ``:caption: caption of code block(text)``
      set a caption to the code block

   - ``:name: a label for hyperlink(text)``
      Define a implicit target name that can be refenced by using ref.

   - ``:dedent: number(number)``
      strip indentation characters from the code block.

literalinclude: filename
^^^^^^^^^^^^^^^^^^^^^^^^

longer displays of ve


examples::

   .. literalinclude:: example.rb
      :language: ruby
      :emphasize-lines: 12,15-18
      :linenos:

options

   - ``:: ([])```

