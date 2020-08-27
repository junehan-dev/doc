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

verbatim text의 displays가 더 길어지는 것은 아마도, 외부 파일을 포함하는 것을 통해서 이루어 질 수 있다.
파일은 ``literalinclude`` directive를 통해서, 이루어 질 것이다.

examples::

   .. literalinclude:: example.rb
      :language: ruby
      :emphasize-lines: 12,15-18,33-
      :linenos:
      :lines: 10-23 
      :encoding: latin-1

파일명은 주로 현재 파일의 위치에 대해 상대적인 것으로 많이 사용한다. 그러나, 만약에 absolute path라면,
그것은 top source directive에 기반한다.

Additional options

``code-block`` 처럼, directive는 ``flag`` , ``lineno-start`` , ``emphasize-lines`` 옵션을 지원한다.
또한 ``name`` , ``dedent`` , ``caption`` 을 지원한다.
차이점은 highlight 대신 ``language`` 옵션을 통해서 highlight-syntax를 지원한다.
``caption`` 옵션은 no arg로 처리될 수 있고, 그러면 파일명을 caption으로 처리한다.

파일에 tab들은 ``tab-width`` 옵션을 주면 특정한 space로 확장될 수 있다.
include파일들은 ``conf.py`` 의 ``source_encoding`` 으로 인코드될것으로 가정된다.
만약 파일이 다른 인코딩을 가지고 있다면, 해당 인코딩을 ``encoding`` 옵션으로 명시할 수 있다.

directive는 또한 파일의 부분만을 포함할 수 도 있다. 만약 파이썬 모듈이라면, 클래스, 함수, 메서드를 ``pyobject`` 옵션을 통해서 줄 수 있다.::

   .. literalinclude:: example.py
      :pyobject: Timer.start

위를 대체하는 방법으로, ``lines`` 옵션을 통해 특정 라인만 include 할 수 있다.
어느 파일을 포함할지 에 대한 다른 방법으로는, ``start-after`` + ``end-before`` 옵션을 주는 것이다.

만약 ``start-after`` 이 string option으로 주어졌다면, 해당 문자열이 포함된 첫 줄을 follow하는 lines만 포함된다.
만약 ``end-before`` 옵션이 주어진다면, 해당 문자열이 포함되는 줄 전까지 만이 포함된다.
``start-at`` 과 ``end-at`` 옵션도 비슷하게 동작하는데, 해당 조건을 만족하는 줄을 포함한다는 뉘앙스의 차이가 있다.
``start-after/start-at`` 과 ``end-before/end-at`` 은 동일한 문자열을 가질 수 있다.

.. note::

   만약 ini 파일형의 **[second-section]** 만을 선택하고 싶다면,
   ``:start-at: [second-section]`` 과 ``:end-before: [third-saction]``::

      [first-section]

      var_in_first=true

      [second-section]

      var_in_second=true

      [third-section]

      var_in_third=true

   이 옵션이 유용한 경우로 tag comments로 처리할 때 그러하다.
   ``:start-after: [initialized]`` 와 ``end-before: [initialized]`` 옵션은 comment사이의 lines를 처리한다::

      if __name__ == "__main__:
          # [initialize]
          app.start(":8000")
          # [initialize]

위의 방법중 어떤 방식으로든 특정 줄만 선택되었을 때, ``emphasize-lines`` 에 해당하는 줄번호들은, 해당 선택된 것들을 가리킨다.
기본적으로 1에서 부터 시작하기 떄문에, 해당 줄을 먼저 ``linenos`` 옵션으로 확인한 후에 처리하는 것이 직관적일 것이다.

포함된 코드에 line을 선행 혹은 덧붙이는 것도 가능하다.
``prepend`` | ``append`` 옵션을 통해서. 이것을 PHP 코드중 <?php/?> 마크를 포함하지 않는 코드를 highlight할 때 유용하다.

만약 코드의 diff를 보여주고 싶다면, old file의 filename을 ``diff`` 옵션을 처리하면 된다::

   .. literalinclude:: example.py
      :diff: example.py.orig

Glossary
^^^^^^^^

이 directive는 반드시 reST definition-list-like markup에 terms-definition가 붙여진 형태로 포함되어야한다.
해당  term-정의들은 이 후, ``term`` role을 통해 referenced될 것이다.::

   .. glossary::

      environment
         A structure where information about all documents under the root is
         saved, and used for cross-referencing.  The environment is pickled
         after the parsing stage, so that successive runs only need to read
         and parse new and changed documents.

      source directory
         The directory which, including its subdirectories, contains all
         source files for one Sphinx project.

일반 definition lists와 차이로는, 엔트리에 대한 복수의 terms가 허락된다는 점, 그리고
inline markup이 term에 허용된다는 점이 있다. 너는 모든 term들에 링크할 수 있다.::

   .. glossary::

      term 1
      term 2
         definition of both terms.

glossary가 정렬되었을 때, 첫번째 term이 sort order를 결정한다.
만약 "grouping keys"를 *general index entries* 로 명시하고 싶다면, 'term-key' 를 'term-key : key', 로 작성하면 된다.::

   .. glossary::

      term 1 : A
      term 2 : B
         definition of both terms.

``key : value`` 에 오는 value가  grouping에 사용된다. keys는 normailzed하지 않다;
key "A" 과 "a"은 다른 그룹이다. 모든 "key"의 모든 캐릭터들은 첫문자를 대신하여 사용된다;
이것은 "Combining character Sequence"와 "Surrogate Pairs" 그룹핑-key로 사용된다.

in i18n 상황에서, 원본 텍스트만이 "term"을 가진다고 해도, "localized term : key"라고 명시할 수 있다.
이 경우,  "localized term"은 "key" 그룹으로 카테고리화 된다.

[Normalization_wiki (statisitcs)]_
   정규화, 통계학과 통계-app에서 **normalization** 은 range of meanings를 가질 수 있다.
   단순한 경우로는, **normalization of ratings** 는 adjusting value measured on different scales to anotinally common scale, often prior to averaging.
   다른 단위의 것들을 흔한스케일로 변환하기 위해 미세조정하는 것, 가끔 평균작업에 선행된다.

Surrogate
   relating to birth of child by means of surrogacy

Surrogacy
   arrangement, often supported by a legal agreement, where by a woman agrees to bear child for another person 

.. [Normalization_wiki (statisitcs)] https://en.wikipedia.org/wiki/Normalization_(statistics)


Meta-information markup
-----------------------

``.. sectionauthor:: name <email>``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   현재 section에서 author를 명시한다. email의 도메인명부분은 소문자 표기되어야 한다::

      .. sectionauthor:: Guido van Rosson <guido@python.org>

default로, 이 마크업은 아웃풋에 반영되지않는다. 이는 기여를 추적하는 것을 도와준다,
이는 conf-value에서 ``show_authors`` 를 True로 설정해주면 output Paragraph를 생성해준다.

``.. codeauthor:: name <email>``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   ``codeauthor`` directive는, 여러번 등장 할 수 있으며, ``sectionauthor`` 처럼 문서의 부분의 저자를 이름 붙인다.
   이 또한 ``show_authors`` 가 True로 설정되어야 output을 문서에 반영한다.

Index-gernerating markup
------------------------

스핑크스는 자동으로 index entries를 모든 object descriptions(class, functions, attrs)로 부터 생성한다.

별도로 명시적은 마크업을 통해서 구성하는 것이 가능하니, index를 더 이해하기 쉽게, 그리고 index entires중 선별할 수도 있다.

``.. index:: <entries>``
^^^^^^^^^^^^^^^^^^^^^^^^

   이 지시자는 단일 혹은 복수의 index entries를 포함한다. entry는 타입과 value로 ``:`` 으로 분리하여 작성한다.
   예를 들면::

      .. index::
         single: execution; context
         module: __main__
         module: sys
         triple: module; search; path

   이 directive는 **5개의 entires를 포함한다.** 그들은 링크가 동작하는 generated index로 변환 될 것이다.
   ``index`` directives가 cross-reference targets를 그들의 위치에서 생성하기 떄문에,
   그들이 의미하는 것을 가리키는 것(like heading...)을 넣기 전에 entry를 먼저 넣는 것은 당연하다.

   Possible entry types::
   
      - single

         Create single index entry.
         subentry를 생성할 수 있으며, subentry text를 semicolon으로 나누는 것으로 만들 수 있다.

      - pair

         ``pair: loop; statement`` is a shortcut that creates two index entries, namely loop; statement and statement; loop.

      - triple

         마찬가지로, ``triple: module; search; path`` is shortcut that creates three index entries,

            1. ``module; search path``
            2. ``search; path``
            3. ``module and path; module search``

      - see

         ``see: entry; other`` create an index entry that refers from entry to other

      - seealso

         like see, but insert ``see also`` instead of ``see``.

      - module, keyword, operator, object, exception, statement, builtin

         이들은 2개의 인덱스 엔트리를 생성한다. 예를 들어::
         
            ``module: hashlib``

            entires module; hashlib과, hashlib를 생성한다.
         
   "main" index entries를 prefixing with exclmation mark함으로써 mark up 할 수 있다.
   "main" 에 대한 entries-references는 generated index에서 emphasized된다.
   예를 들어 이 두 페이지들은 아래를 포함한다.

   .. code-block:: rst

      .. index:: Python
   
   and other page

   .. code-block:: rst

      .. index:: ! Python
   
   이럴 경우 뒤쪽 페이지에대한 backlink가 3개의 backlinks 중에서 emphasized된다.

   index directives가 "single" entries를 containg한다면 아래 shotrcut을 적용해도 좋다.

   .. code-block:: rst

      .. index:: BNF, grammar, syntax, notation

   이것은 4개의 인덱스 엔트리를 생성한다.


   options

   :name: label for hyperlink(text)
      Define implicit target name that can be referenced by usign ``ref``

      .. code-block:: rst

         :name: py-index

``:index:``
^^^^^^^^^^^


   ``index`` directives는 block-level 마크업이며, 다음 Paragraph의 시작으로 링크하는 반면,
   link target을 사용되는 곳에 직접 set할 수 있는 ``role`` 이 있다.

   ``role`` 의 content는 텍스트안에 유지되고 있고, 텍스트 엔트리로 쓰이는 simple phrase 일 수 있다.
   또한 cross-references의 explict targets처럼 보이는 text와 index entry의 조합일 수도 있다. 
   그러한 경우에, "target" 부분은, full entry일 수 있다. 예를 들어::

      This is normal reST :index:`paragraph` that contains several
      :index:`index entries <pair: index; entry>`.

Including content based on tags
-------------------------------

``.. only:: <expression>``
^^^^^^^^^^^^^^^^^^^^^^^^^^

   directive의 content를 include한다, 오직 *expression* is true.
   expression은 tags로 구성되어야 한다.::

      .. only:: html and draft
   
   undefined tags are false, defined tags(via the -t command-line option or within conf.py) are true.
   Boolean expressions와 괄호를 사용하는 것``html and (latex or draft)`` 표현은 잘 동작한다.

   format과 name of the current builder(html, latex or text)는 항상 tag로서 set된다.
   format과 name을 명시적으로 분별하기 위해서, ``format_`` | ``builder_`` prefix가 added된다.::

      epub builder는 html, epub를 format_html, builber_epub로 태그를 정의한다.

   위 표준 태그들은 conf.py file이 read된 이후에 set되어, 그들은 conf에서는 볼 수 없다.

   모든 태그들은 *standard python identifier syntax* 를 `identifiers and keywords`_ documentation에 명시된 대로 따라야 한다..
   그는, tag표현이 syntax of Python variables를 따르는 태그들 만으로 구성될 수 있다는것이다.
   ASCII에서, 이것은 A-Z 까지 lower, uppercase로 구성되며, underscore과 0-9의 숫자형-문자 로 구성된다.

   .. warning::

      이 directive는 오직 document의 content를 조작하기 위해 설계 되었다. 이것은 sections, labels등을 조작할 수 없다.

.. _identifiers and keywords: https://docs.python.org/3/reference/lexical_analysis.html#identifiers

tables
------

- reST's grid table::

   +------------------------+------------+----------+----------+
   | Header row, column 1   | Header 2   | Header 3 | Header 4 |
   | (header rows optional) |            |          |          |
   +========================+============+==========+==========+
   | body row 1, column 1   | column 2   | column 3 | column 4 |
   +------------------------+------------+----------+----------+
   | body row 2             | ...        | ...      |          |
   +------------------------+------------+----------+----------+

- reST's simple table::

   =====  =====  =======
   A      B      A and B
   =====  =====  =======
   False  False  False
   True   False  False
   False  True   False
   True   True   True
   =====  =====  =======

일반적인 처리를 위햐서라면 위의 2 내장 table formmater를 사용하세요.::

   - grid table sytax(reST tables)
   - simple table syntax(reST tables)
   - csv-table syntax
   - list-table syntax

`csv-table`_
^^^^^^^^^^^^

   .. warning::

      "csv-table" directive의 ":file:"과 ":url:" 옵션은 잠재적으로 보안문제를 야기합니다.
      ``file_insertion_enalbed`` 런타임 세팅을 통해 disabled 가능합니다.

   .. code-block:: rst

      .. csv-table:: Frozen Delights!
         :header: "Treat", "Quantity", "Description"
         :widths: 15, 10, 30

         "Albatross", 2.99, "On a stick!"
         "Crunchy Frog", 1.49, "If we took the bones out, it wouldn't be
         crunchy, now would it?"
         "Gannet Ripple", 1.99, "On a stick!"

   .. csv-table:: Frozen Delights!
      :header: "Treat", "Quantity", "Description"
      :widths: 15, 10, 30

      "Albatross", 2.99, "On a stick!"
      "Crunchy Frog", 1.49, "If we took the bones out, it wouldn't be
      crunchy, now would it?"
      "Gannet Ripple", 1.99, "On a stick!"


   .. _csv-table: https://docutils.sourceforge.io/docs/ref/rst/directives.html#csv-table

`list-table`_
^^^^^^^^^^^^^

   .. code-block:: rst

      .. list-table:: Frozen Delights!
         :widths: 15 10 30
         :header-rows: 1

         * - Treat
           - Quantity
           - Description
         * - Albatross
           - 2.99
           - On a stick!
         * - Crunchy Frog
           - 1.49
           - If we took the bones out, it wouldn't be
             crunchy, now would it?
         * - Gannet Ripple
           - 1.99
           - On a stick!

   .. list-table:: Frozen Delights!
      :widths: 15 10 30
      :header-rows: 1

      * - Treat
        - Quantity
        - Description
      * - Albatross
        - 2.99
        - On a stick!
      * - Crunchy Frog
        - 1.49
        - If we took the bones out, it wouldn't be
          crunchy, now would it?
      * - Gannet Ripple
        - 1.99
        - On a stick!


   .. _list-table: https://docutils.sourceforge.io/docs/ref/rst/directives.html#list-table