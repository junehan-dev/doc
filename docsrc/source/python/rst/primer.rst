Primer
======

This is bried introduction to rst concepts and syntax,
intended to provide authors with enough information to author
documents productivly. reST는 단순하고, 화려하지 않은 마크업 언어로 설계되었기 때문에, 오래 걸리진 않을 겁니다.

Paragraphs
----------
Paragrap는 reST문서에서 가장 기본형의 블록입니다. Paragraph들은 단순한 문자의 덩어리이고 1줄 혹은 그 이상의 둘로 나눠집니다.
파이썬에서 그렇듯이, 들여쓰기는 reST에서 매우 중요합니다, 따라서 동일한 Paragraph의 모든 라인은 좌측에 맞춰 동-레벨의 intentation을 가져야 합니다.

line markup
-----------

표준 reST inline은 사용하기 정말 쉽죠,: 아래처럼,

   - one asterisk: *text* for itailic emphasis(light emphasis)
   - two asterisk: **text** for blod emphasis(strong emphasis)
   - backquotes: ``text`` for code samples.

만약 asterisks or backquotes가 텍스트 중에 보이거나, 인라인 마크업표시용과 붙여서 사용된다면,
backslash를 사용하여 escaped되어야 합니다.
아래의 마크업 제한사항을 주의하세요.

   .. hint::
      - it may not be nestd,
      - content may not start with whitespace: ``* test*`` is wrong.
      - must sperated from surrounding text by non-word characters. use a backslash escaped space to work around that: thisis\ *one*\

.. _Roles: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#rst-roles-alt

이 제한사항들은 docutils의 버전에 따라 바뀔 수 있습니다.
이러한 인라인 마크업을 `Roles`_ 를 통해 대체하거나 확장할 수 있습니다.

Literal blocks
--------------

코드 블록은 Paragraph를 특별한 표시인 ::.으로 종료함으로 사용 가능합니다.
문맥상 블록은 반드시  indented되어야 합니다.(그리고 모든 Paragraphs처럼, 하나의 빈줄로 경계를 표시해야 합니다.)::

   This is a normal text paragraph. The next paragraph is a code sample::

      It is not processed in any way, except
      that the indentation is removed.

      It can span multiple lines.

   This is a normal text paragraph again.

\:\:마커는 매우 영리하게 처리됩니다.

   - 만약 paragraph로서 paragraph의 것으로 일어난다면, 해당 paragraph는 완전히 문서에서 제외됩니다.
   - 만약 공백이 선행된다면, 마커는 삭제됩니다.
   - 만약 공백이 아닌것에 붙여 온다면, *마커는 하나의 콜론으로 전환됩니다.*

Directives
----------

지시체는 명시적인 마크업의 일반적인 블럭입니다.
roles를 따라, 이것은 reST의 확장 매커니즘의 하나이고,
스핑크스는 이것을 매우 깊게 활용할 수 있도록 만들어 놓았습니다.

Docutil support Directives
^^^^^^^^^^^^^^^^^^^^^^^^^^

   - Admonitions

      - **warining type**:   ``danger``, ``error``, ``warning`` 
      - **information type**: ``causion``, ``attension``, ``hint``, ``important``, ``note``, ``tip``, ``admonition``
    
   - images

      - image: image file src type
      - figure: image with caption, (and optional legend)

   - addition body elements

      - contents: local. i.e. for current file only, table of content 
      - container: container with custom class, useful to generate outer <div> in html
      - rubic: a heading without relation to document sectioning
      - topic: special highlighted body elements
      - sidebar: special highlighted body elements
      - parsed-literal: literal block thet support inline-markup
      - ephigraph: block quote with optional attribution line
      - highlights: block quoutes with their own class attribute
      - pull-quoute: block quoutes with their own class attribute
      - compound: compount paragraph

   - special tables

      - table: table with title
      - csv-table: table, generated from comma-sepeated values
      - list-table: table, generated from a list of lists

   - special Directives

      - raw: include raw target-format markup
      - include: **(include reST from another file)::**

         in sphinx, when given absolute include file path,
         this directive takes it as relative to the source directory.

      - class: assgin a class attribute to ther next element

   - html sepcifics

      - meta: generate html <meta> tags
      - title: override document title

   - influencing markup

      - default-role: set a new default role
      - role: create a new role

.. warning::

   sectnum, hearder, footer Directives.. DO NOT USE!

.. note::

   Sphinx에서 추가된 Directives는 Directives_ 문서를 참조해주세요.

.. _Directives: https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html

.. important::

   directive는 ``name``, ``argumets``, ``options``, ``content`` 로 구성됩니다.
   (이 기본 룰을 기반으로 활용해야 합니다. 이후 custom-directives를 만들때 꼭 필요하니 말이죠.)

아래 예제를 한번 보죠::

   .. function:: foo(x)
                 foo(y, z)
      :module: some.module.name

      Returns a line of text input from the user.

function은 지시자의 이름입니다. 두 개의 매개변수가 주어지고,
첫-줄과 둘째-줄에 남아있는 것들이죠. *module* 옵션도 그렇습니다.
(발견하셨을거라 추측하지만, 옵션들은 매개변수 이후에 바로 주어지고, 콜론에 의해서 지정됩니다.)
옵션들은, 반드시 directve-content와 동일한 레벨에 indent되어야 합니다.

directive-content는 하나의 공백-줄 이후에 따라오며, directive 시작부에 상대적인 레벨로 indented됩니다.

Footnotes
---------

*note*
   | 문서나 챕터의 끝에 최하단에 있는 문장입니다.
   | note는 저자의 말이나, 텍스트에 지원부인 레퍼런스의 Citations를 포함합니다.

*footnotes*
   | ``endnote`` 들은 챕터의 끝에서 분리된 헤딩으로 수집되는 반면, 
   | *페이지* 의 foot에 있는 노트들입니다.
   | footnote와 다르게 endnote는 메인 텍스트 레이아웃을 방해하지 않는다는 장점이 있습니다.
   | 그러나 독자가 노트가 필요할 때 페이지를 이동해야한다는 단점이 있죠.

``[#name]_`` 를 사용하여 footnote의 위치를 마크할 수 있습니다.
그리고, footnote-바디를 문서의 최하단에, "Footnotes" rubic헤딩(문서에 관계를 잡지 않는 헤딩) 이후에 두면 됩니다, 아래처럼요::

   Lorem ipsum [#f1]_ dolor sit amet ... [#f2]_

   .. rubric:: Footnotes

   .. [#f1] Text of the first footnote.
   .. [#f2] Text of the second footnote.

명시적으로 (``[1]_``)식으로 넘버링을 하거나, 이름없이 자동 넘버링인, (``[#]_``). 을 사용할 수 있습니다.


Citations
---------

*Citation*
   | reference to source.
   | precisely, is Abbreviated alphanumeric expression embedded in the body of an intellectual work,
   | that denotes(by a sign, indicate) an entry of bibilographic(list of alll source used) references section of the work
   | for purpose of ack the relevance of the works of other to the topic of discussion at the spot where citation appears.

표준 reST citations는 그들이 "global" 하다는 추가적인 특성과 함께 지원됩니다.
예를 들어 모든 citations는 모든 파일에서 참조될 수 있죠.
아래 처럼 사용하세요::

   Lorem ipsum [Ref]_ dolor sit amet.

   .. [Ref] Book or article reference, URL or whatever.

citation사용은, footnote와 비슷하지만, 라벨이 넘버링 불가하고 #으로 시작하지 않는다는 점에서 다릅니다.