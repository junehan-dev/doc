The Principal structure of a program
====================================

이 장에서 다루는 내용은

   - C 문법
   - identifiers의 정의하는 법
   - objects의 정의하는 법
   - 컴파일러에게 statements로 Instructing하는 법

C 프로그램에서 고려해야 할 측면

   - syntactical aspect (형태소의)
   - semantic aspect (의미적인)
      - declarative
      - definitions of objects
      - statements

문법요소
--------

:Special words: ``#include, int, void, double, for, return``

   이 특수 단어들은 C언어가 내재하는 기능을 대표하며 바뀔 수 없다.

:Punctuations(meaning of spacing): ``{...}, (...), [...], /*...*/, <...>, ',', ';'``

   - ``<...>`` 기호는 다행히 C에선 거의 쓰이지 않는다.
   - comment라인은 컴파일러에게 무시된다. 따라서 코드를 문서화하기에 가장 적합한 장소가 된다.

      그러한 올바른 문서는 가독성과 이해력을 끌어올린다.

:Identifier(names): 이 이름들은 program에게 특정한 entities를 전달한다.

   - Data Objects: Variables
   - Type: ``size_t`` 와 같은 aliases. Object의 분류를 명시한다.

       ``_t``\가 뒤에 붙는데, 이것은 C의 컨벤션중 하나로 타입에 대한 명시를 분명히 사용자에게 전달하는 것이다.

:Functions: ``main, printf``

   - ``main``\function's in-turn(뒤따르는)는 defined이다.

      - main에 대한 declaration뒤에 block이 뒤따라서, 어떤 행동을 해야하는 지를 명시한다.
      - main은 starting point of execution이기 때문에 항상 존재해야한다.

:Operators: ``++, <, =, *``

   - C언어의 많은 Operators중 아래의 것들을 다룹니다.

      - *=* : Initialization and Assignment
      - *<* : Comparison
      - *++* : Increment a variable
      - *\** : Multiply two values



.. important::

   Three main semantic categories that C distinguishes:

   - declarations (선언)
   - definitions (정의)
   - statements (진술)

Declarations
------------

| 어떤 name의 정의에 뒤따라서 무엇을 의미하는지 컴파일러에게 전달한다.
| 여기서 Identifier(name)과 Keywords의 다른 점이 나타난다.

모든 identifier들은 선언 되어야 한다.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- 아래의 대상들은 선언만 하는 것으로, keyword를 사용하여 identifier로 선언하는 것이다.

   .. code-block:: c

      int main(void);
      double A[5];
      size_t i;
 

   개념적으로

      - object (name of specific memory)
      - specification (type of object)
      - content of object (value of object)
      - name or lable (identifier)

   이들을 구분하는 것은 중요하다.

- 다른 3가지의 identifiers는 아래처럼 선언하는 것은 아니지만, 어딘가에 선언되어 있는 것이다.

   .. code-block:: c

      int printf(char const format[static 1], ...);
      typedef unsigned long size_t;
      #define EXIT_SUCCESS 0

   - 이 정보는 주로 그들의 *include files* 또는 *header files*\에 포함되어 있다.
   - 그들의 semantics를 알아야한다면, 해당 파일 직접 찾아보는 것은 안 좋은 방법이다. (``man``\을 사용하자.)

Identifiers들은 오래 유지되는 선언들을 가질 수 있다.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*The Scope* 는 프로그램의 한 부분이며, identifiers에 접근 가능한 공간이다.

   | 만약 파일에서 동일한 명칭들을 여러 가지 만난다면 어려운 일이 될 것이며, 이는 금지된다.
   | C언어는 "프로그램의 동일한 부분" 이라는 것에 대해서 매우 구체적으로 하는 편이다.


Declarations는 identifier들을, definitions는 object를 명시한다.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*An Initilization*
   object에 대해 선언과 초기값을 부여하여 문법적으로 구축하는 것을 의미한다.

| C에서는 그러한 선언이 initializer이 포함되었다면, object를 정의하고 그에 합당한 이름을 붙이는 것이다.
| 이는 컴파일러에게 value가 저장된 수 있는 공간을 제공하라고 알려주는 것이다.


0이 아닌 모든 값은 논리적으로 true를 의미한다.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

   if  (i != 0) {

   }

   if (i) {

   }

| 위의 2개의 버전 중 무엇이 더 가독성이 좋은지에 대한 것은 코딩스타일의 문제이고, 의미없는 논쟁이다.
| 첫 번째의 것은 C코드를 읽는 초보인 사람에게 조금 더 쉬울 것이고, 두 번쨰의 것은 C의 타입 시스템에 지식이 있는 이들에게 선호된다.

| ``bool``\타입은 ``stdbool.h``\에 명시되어 있으며, ``false, true``\를 사용할 수 있도록 한다.
| 기술적으로 단지 0과 1의 다른 표현이지만, 명시적인 표현이라는 점에서 의미가 있다.

.. literalinclude:: ../srcs/ch01/usingBool.c
   :language: c
   :linenos:
   :lines: 1-12
   :encoding: latin-1

Basic Values and data
---------------------
   | 이제까지 Statement  and expressions, 어떻게 진행되는지에 대해 다뤘다.
   | 이제 C프로그램이 동작시키는 것들, value와 data에 대해 다룰 것이다.
   | 만약 이미 C에 대해서 데이터 조작에 익숙하다면, 기존의 지식을 잊기위해 노력할 필요가 있다.
   | 컴퓨터의 값에 대해서 명확한 표현들을 생각하는 것은 사용되는 것보다 더 도움이 될 것이다.

The abstract state machine
^^^^^^^^^^^^^^^^^^^^^^^^^^

*C 프로그램은 값들에 대해서 원인을 찾지 사용법에 대해 생각하지 않는다.*

   | 특정 값의 representation(표현)은 당신이 대부분 신경쓰지 말아야할 것이다.
   | C프로그램에서 주로 얘기해야할 부분은 C의 *abstract state machine(추상 상태 장치)*\이다.
   | 이것은 플랫폼에 관계없이 당신의 프로그램의 실행에 대한 정보를 준다.
   | state의 구성요소인 object들은 고정된 해석방식과 시간에 따라 달라지는 값들을 가지고 있다.

   | 명확한 최적화를 위해서는, C컴파일러가 탐지가능한 상태들을 재생산하는 프로그램을 생성하는 것이 중요하다.
   | 이것은 일부 변수들의 내용들과 프로그램의 실행중에 생성되는 출력으로 구성되어 있다.
   | 이 모든 변화의 메커니즘이 *abstract state machine*\이다.

:value: 무슨 상태인가

   - 추상 개체(abstract entity)이다.
      - 프로그램을 넘어서 존재하는 것
      - 프로그램의 특정한 구현체를 넘어서 존재하는 것
      - 프로그램의 실행동안 값이 대표하는 것을 넘어서 존재하는 것

   - 모든 값들은 숫자이고 숫자로 변환된다.

      | 이 속성이 C가 실제로 신경쓰는 것이다.
      | 수학적인 개체로 프로그램에 독립적인 구체적인 실체화이다.
      | 프로그램 실행의 state는 아래와 같은 요소로 정해진다.::

         - executable
         - execution point
         - data
         - outside intervention(내부화), like IO

      | C프로그램이 다양한 시스템에서 이식성을 가져야 하므로, 우리는 코드와 입출력 이상의 것을 원하는데,
      | 우리는 컴퓨팅의 결과가 프로그램이 아닌, 프로그램 사양 그 자체에 의존하기를 희망한다.
      | 이 이식성을 위해 중요한 단계가 *types*\의 개념이다.

:type: 이 상태가 무엇을 의미하는가

   | type은 C가 값과 관련시키는 추가적인 속성이다.
   | 이제까지 ``size_t, bool, double``\등의 타입을 보았다.

   - 모든 값들은 정적으로 결정되는 타입을 지닌다.
   - 프로그램의 실행에 영향을 주지 않는 선에서 실행 순서를 변경할 수 있다.

   | 프로그램 설명과 추상 상태 머신사이에 느슨함이 허용되는 것이 매우 가치있는 기능인데,
   | 주로 최적화의 일환으로 언급된다.
   | 이 언어의 단순함과 결합되어, 이것은 다른 장치가 많은 언어에 비해 두드러지는 주요 특성이다.

:representation: 어떻게 상태가 구분되는가

.. todo:: C/modernc/basic_types

