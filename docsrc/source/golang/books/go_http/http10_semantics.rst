Semantics of Http/1.0
=====================

**Timeless, 4-core components of HTTP** 

   - Method, URL(domain + path)
   - Header
   - Body
   - Status code

Simple form transfer (x-www-form-urlencoded)
--------------------------------------------

form encode request:

   .. literalinclude:: srcs/curl_cmds/http10_form_encode.sh
      :language: bash
      :encoding: latin-1

   output:

      .. code-block:: bash

         Content-Length: 71
         Content-Type: application/x-www-form-urlencoded
         User-Agent: curl/7.82.0

         title=Head+First+PHP+%26+MySQL&author=Lynn+Beighley%2C+Michael+Morrison

URL encoding (wikipedia)
   | **Percent-encoding**\은 **URL encoding**\이라고도 알려져 있으며,
   | 형식적인 데이터를 *US-ASCII*\만을 허용하는 URI로 encode하는 방법이다. 
   | 일반적으로 주류 URI셋에서 사용되는데, ``application/x-www-form-urlencoded``\의 Media type 데이터의 준비작업에 사용되며 HTML form에 사용된다.

File transfer using form (multipart/form-data)
----------------------------------------------

.. code-block:: html

   <form action="POST" enctype="multipart/form-data></form>

| 멀티파트를 이용하는 경우는 한번의 요청으로 복수의 파일을 받는 쪽에서 구분할 수 있어야합니다.
| 경계문자열(boundary)가 포함되어 브라우저가 독자적인 포멧을 랜덤하게 만들어냅니다.

.. code-block:: text

   Content-Type: multipart/form-data; boundary=-----WebkitFromBoundaryy0YfbccgoID172j7

.. note::

   헤더에 *Content-Disposition* 항목이 포함됩니다.

Multipart form test
^^^^^^^^^^^^^^^^^^^

- Request

   .. literalinclude:: srcs/curl_cmds/http10_multipart_file.sh
      :language: bash
      :encoding: latin-1

- Client

   .. code-block:: text

      POST / HTTP/1.0
      > Host: 127.0.0.1:18888
      > User-Agent: curl/7.82.0
      > Accept: */*
      > Content-Length: 354
      > Content-Type: multipart/form-data; boundary=------------------------8eef5e797ff1fda2
      >
      * We are completely uploaded and fine
      * Mark bundle as not supporting multiuse
      * HTTP 1.0, assume close after body
      < HTTP/1.0 200 OK
      < Date: Wed, 13 Apr 2022 12:27:39 GMT
      < Content-Length: 32
      < Content-Type: text/html; charset=utf-8

- Server

   .. code-block:: text

      POST / HTTP/1.0
      Host: 127.0.0.1:18888
      Connection: close
      Accept: */*
      Content-Length: 333
      Content-Type: multipart/form-data; boundary=------------------------8eef5e797ff1fda2
      User-Agent: curl/7.82.0

      --------------------------8eef5e797ff1fda2
      Content-Disposition: form-data; name="file2"
      Content-Type: text/html

      simple string

      --------------------------8eef5e797ff1fda2
      Content-Disposition: form-data; name="file1"; filename="test.txt"
      Content-Type: text/html

      simple string

      --------------------------8eef5e797ff1fda2--

.. note::

   text/plain

   form의 encoding type은 기본적으로 www-form-urlencoded,
   파일을 전송하고 싶을 때: multipart/form-data.

   text/plain은 개행을 통해 각 값을 구분해 전송합니다.

   ```
   title=The Art of community
   author=john bacon
   ```

   서비스쪽이 보안이 약할 때 문제를 일으키는 경우가 있다고 합니다.




Content negotiation
-------------------
   서버와 클라이언트간 통신을 최적화 하고자 하나의 요청에서 최고의 설정을 공유하는 시스템

.. csv-table::
   :header: "Request Header", "Response Header", "negotiation target"

   "Accept", "Content-Type", "MIME Type"
   "Accept-Language", "Content-Type / html tag", "language"
   "Accept-Charset", "Content-Type", "character set"
   "Accept-Encoding", "Content-Encoding", "compression type"

File type 결정
^^^^^^^^^^^^^^

``Accept: text/html,application/xml;q=0.9,image/webp,*/*;p=0.8``

   - image/webp
   - \*/\*;q=0.8

::

   quality value는 0~1 사이의 수치로, 기본 1.0일 경우에는 명시하지 않습니다.
   0.9의 우선순위로 webp를 요청하고, 그렇지 않으면 다른 포맷(0.8)을 다음으로 두고 있는 표현입니다.


Charset 결정
^^^^^^^^^^^^

``Accept-Charset: window-949,utf-8;q=0.7,*;q=0.3``

   브라우저가 모든 charest-encoder를 내장하고 있기 때문에, 협상이 필요없어 졌습니다.

``Content-Type: text/html; charset=UTF-8``

   HTML의 경우 문서 안에 쓸 수도 있습니다.

1. ``<meta http-equiv="Content-Type" content="text/html; charset=UTF-8>``
2. ``<meta charset="UTF-8">``

압축을 이용한 통신속도 향상
^^^^^^^^^^^^^^^^^^^^^^^^^^^

통신에 걸리는 시간보다 압축과 해체가 짧은 시간에 이루어지므로, 압축을 함으로써 웹페이지를 표시할 때 걸리는 전체적인 처리시간을 줄일 수 있습니다.

PROS::

   - 전력소비 감소
   - 데이터 사용 비용 절감


압축에 대한 negotiation은 header안에서 명시합니다.

``Accept-Encoding: deflate, gzip``

.. literalinclude:: srcs/curl_cmds/http10_compress.sh
   :language: bash
   :encoding: latin-1

.. code-block:: bash

   > GET / HTTP/1.0
   > Host: 127.0.0.1:18888
   > User-Agent: curl/7.82.0
   > Accept: */*
   > Accept-Encoding: gzip, br
   >
   * Mark bundle as not supporting multiuse
   * HTTP 1.0, assume close after body
   < HTTP/1.0 200 OK
   < Date: Fri, 15 Apr 2022 11:34:53 GMT
   < Content-Length: 32
   < Content-Type: text/html; charset=utf-8

- 서버가 압축 content를 지원하지 않기 떄문에 차이가 발생하지 않았을 뿐 요청을 정상적으로 처리되었습니다.
- 서버가 gzip을 지원했을 경우, 아래와 같은 응답을 반환합니다.

   .. code-block:: bash

      < Content-Encoding: gzip
      < Content-Length: (zipped size of bytes)

- gzip보다 효율이 좋은 *Brotil*\는 구글의 proposal로 등장하였습니다

   *HTTP헤더를 사용해 1왕복을 데이터 교환 단계에서 하위 호환성을 유지하면서도, 서로 최적의 통신을 할 수 있게 시스템이 정비되었습니다.*

.. seealso::

   sdch(Shared Dictionary Compressing for HTTP), 는 IANA비등록 압축알고리즘이며, 중복되는 문구를 공유 사전으로 사용하여 통신량을 대폭 줄일 수 있는 알고리즘입니다.
   이런 공유 방식은 HTTP/2의 헤더부분 압축에도 사용됩니다.

Cookie
------

쿠키란 웹사이트의 정보를 브라우저에 개별 domain에 대해서 저장하는 작은 파일입니다.

.. code-block:: bash

   response:

   Set-Cookie: LAST_ACCESS_DATE=Jul/31/2016
   Set-Cookie: LAST_ACCESS_TIME=12:04
   ----------------------------
   client:

   Cookie: LAST_ACCESS_DATE=Jul/31/2016
   Cookie: LAST_ACCESS_TIME=12:04

set-cookie라는 약속된 헤더를 통해서 key-value pair를 저장하고,

다음 요청시에 쿠키정보를 포함한 request를 전송하는 것으로 상태를 이어갈 수 있습니다.

.. literalinclude:: srcs/01_http10/02_coookie.go
   :language: go
   :encoding: latin-1

.. literalinclude:: srcs/curl_cmds/http10_cookie.sh
   :language: bash
   :encoding: latin-1

.. code-block:: bash

   -----------------FIRST: NO COOKIE
   > GET / HTTP/1.0
   > Host: 127.0.0.1:18888
   > User-Agent: curl/7.82.0
   * HTTP 1.0, assume close after body

   < HTTP/1.0 200 OK
   * Added cookie VISIT="TRUE" for domain 127.0.0.1, path /, expire 0
   < Set-Cookie: VISIT=TRUE
   <
   <html><body>FIRST VISIT!!</body></html>
   * Closing connection 0

   ---------------SECOND: YEP COOKIE
   > GET / HTTP/1.0
   > Host: 127.0.0.1:18888
   > User-Agent: curl/7.82.0
   > Cookie: VISIT=TRUE
   * HTTP 1.0, assume close after body

   < HTTP/1.0 200 OK
   * Replaced cookie VISIT="TRUE" for domain 127.0.0.1, path /, expire 0
   < Set-Cookie: VISIT=TRUE
   < Date: Fri, 15 Apr 2022 12:35:19 GMT
   < Content-Length: 35
   < Content-Type: text/html; charset=utf-8
   <
   <html><body>VISITED~</body></html>
   * Closing connection 0

.. important::

   *HTTP*는 *statless*를 기반으로 개발되었지만, *cookie*를 이용하면 서버가 상태를 유지하는 *Stateful*\처럼 보이게 서비스를 제공할 수 있습니다.

쿠키의 잘못된 사용법
^^^^^^^^^^^^^^^^^^^^

.. danger::

   - 쿠키 보관이 클라이언트에서 임의적으로 이루어지고 접근이 쉽기 떄문에 조작의 가능성이 매우 높다.
   - 서버의 지시대로 보관기간을 꼭 지키지는 않는다.
   - 암호화 되어도 일단 획득이 너무 쉽다는 것 자체가 문제입니다.

따라서 사라져도 문제가 없는 정보나, 언제든 복원할 수 있는 정보를 저장하는 용도로 적합합니다.

.. caution::

   - 쿠키의 최대 크기는 4KB사양으로 정해져 있어, 더 보낼 수는 없습니다.
   - 통신량의 증가는 요청과 응답속도 모두에 영향이 있습니다.

secure속성을 부여하면, HTTPS의 암호화된 데이터로 쿠키를 전송하지만, HTTP에서는 평문으로 전송됩니다.

   - 정보를 넣을 때는 서명이나 암호화처리가 불가피합니다.

쿠키에 추가 제약속성
^^^^^^^^^^^^^^^^^^^^

쿠키를 보낼 곳을 제어하거나, 쿠키의 수명을 설정하는 등의 기술이 정의되어 있습니다.

.. tip::

   HTTP클라이언트는 이 속성을 해석해 쿠키 전송을 제어할 책임이 있습니다.

- *속성*\은 대소문자를 구분하지 않습니다.
- *속성*\은 *';'*\기호를 사용해 얼마든지 나열 가능합니다.


:Expires, Max-Age: 쿠키의 수명을 설정

   - Max-Age는 초단위 지정
   - Expires는 *Wed, 09 Jun, 2021 10:18:14GMT* 같은 형식의 문자열을 해석합니다.

:Domain: 클라이언트에서 쿠키를 전송할 대상 서버 domain

   - 생략시, 발행한 서버에 대해서 적용이 됩니다.

:Path: 클라이언트에서 쿠키를 전송할 대상 서버 path

   - 생략시, 쿠키를 발행한 서버 Path

:Secure: https프로토콜을 사용할 때만 클라이언트는 서버로 쿠키를 전송한다.

   - 쿠키는 매핑된 URL을 대상으로 전송처를 정하기 때문에,
     DNS해킹으로 URL을 사칭하면 의도치 않은 서버에 쿠키를 전송할 위험이 있습니다.
   - DNS해킹은 무료 와이파이 서비스등으로 간단히 가능합니다.
   - Secure속성을 추가하면 http접속일때는 브라우저가 경고를 하고 접속하지 않아 정보 유출을 막게됩니다.

:HttpOnly: 쿠키를 자바스크립트로 다룰 수 없도록 한다.

   - 이 속성을 붙이면 자바스크립트 엔진으로부터 쿠키를 감출 수 있습니다.
   - *Cross-site-scripting*\등 자바스크립트가 실행하는 보안위험에 대한 방어가 가능합니다.

:SameSite: RFC의 규약에 존재하지 않으며 크롬브라우저에서 도입한 속성으로 출처의 도메인으로 전송하도록 브라우저가 강제합니다.

authorization and session
-------------------------

Basic Auth
^^^^^^^^^^

Basic 인증
   유저명과 password를 BASE64인코딩 처리합니다.
   변환가능하므로, 그대로 원본을 추출할 수 있습니다.

   .. caution::

      SSL/TLS 통신을 사용하지 않은 채로 감청시에 손쉽게 유출됩니다.

.. literalinclude:: srcs/curl_cmds/http10_auth_basic.sh
   :language: bash
   :encoding: latin-1

.. code-block:: bash

   * Connected to 127.0.0.1 (127.0.0.1) port 18888 (#0)
   * Server auth using Basic with user 'june'
   > GET / HTTP/1.0
   > Host: 127.0.0.1:18888
   > Authorization: Basic anVuZTpwd2QxMjM0
   > User-Agent: curl/7.82.0
   > Accept: */*

Digest Auth
^^^^^^^^^^^

Digest 인증
   hash function(암호화는 쉽지만 복호화는 어렵게)을 이용합니다.
   보호된 영역에 접속을 시도할 시에, 복호화에 실패하면 *401  Unauthorized*\의 응답을 반환합니다.

- *RESPONSE HEADER*

   ``WWW-authenticated: Digest realm="영역명", nonce="1234567890", algorithm=MDS, qop="auth"``

   DESC

      - realm: 보호 영역 명칭
      - nonce: 서버가 매번 생성하는 random data
      - qop: 보호수준

      클라이언트에서는 *nonce*\와 주어진 값을 통해 생성한 **cnonce**\로 다음과 같은 BODY를 구합니다.

- *RESPONSE BODY*

   .. code-block:: go

      A1 = USERNAME ":" realm ":" "PASSWORD"
      A2 = HTTP METHOD ":" Content URI
      response = MD5( MD5(A1) ":" nonce ":" nc ":" cnonce ":" qop ":" MD5(A2)

   DESC

      - nc: 전송횟수로 16진수 8자리(4byte)로 표현됩니다.


      클라이언트에서는 생성한 **cnonce**\와 계산으로 구한 response를 모아 다음과 같은 header를 포함하는 request를 구성합니다.

- *REQUEST HEADER*

   .. code-block:: go

      Authorization: Digest username="username", realm="area name"
            nouce="1234567890", url"/scret.html", algorithm=MD5,
            qop=auth, nc=00000001, cnounce="0987654321",
            response="9d47a3f8b2d5c"
   

   DESC

      - 서버측에서도 이 헤더의 정보와 서버에 저장된 유저명 패스워드로 같은 계산을 실시하여 비교합니다.

         클라이언트가 재발송한 요청과 동일한 response body가 계산되면, 사용자가 정확하게 유저명과 패스워드릴 입력했음을 의미합니다.

      - **이 과정으로 유저명과 패스워드 자체를 요청에 포함하지 않고 사용자를 올바르게 인증하였습니다.**

- digest server and client

   .. literalinclude:: srcs/01_http10/03_digest.go
      :language: go
      :encoding: latin-1

   .. literalinclude:: srcs/curl_cmds/http10_auth_digest.sh
      :language: bash
      :encoding: latin-1

   .. code-block:: bash

      *   Trying 127.0.0.1:18888...
      * Connected to 127.0.0.1 (127.0.0.1) port 18888 (#0)
      * Server auth using Digest with user 'june'
      > POST /digest HTTP/1.0
      > Host: 127.0.0.1:18888
      > User-Agent: curl/7.82.0
      > Accept: */*
      > Content-Length: 0
      > Content-Type: application/x-www-form-urlencoded
      >
      * Mark bundle as not supporting multiuse
      * HTTP 1.0, assume close after body
      < HTTP/1.0 401 Unauthorized
      < Www-Authenticate: Digest                              realm="Secret Zone",                            nonce="TgLc25U2BQA=f410a2789473e18e6587be703c2e67fe2b04afd",                          algorithm=MD5,                          qop="auth"
      < Date: Sat, 16 Apr 2022 05:52:42 GMT
      < Content-Length: 0
      <
      * Closing connection 0
      * Issue another request to this URL: 'http://127.0.0.1:18888/digest'
      * Hostname 127.0.0.1 was found in DNS cache
      *   Trying 127.0.0.1:18888...
      * Connected to 127.0.0.1 (127.0.0.1) port 18888 (#1)
      * Server auth using Digest with user 'june'
      > POST /digest HTTP/1.0
      > Host: 127.0.0.1:18888
      > Authorization: Digest username="june", realm="Secret Zone", nonce="TgLc25U2BQA=f410a2789473e18e6587be703c2e67fe2b04afd", uri="/digest", cnonce="NmIwMjc5NDc5ODkxYmI4MzNiOWVmNTEyMjdjYjgyNDI=", nc=00000001, qop=auth, response="f6159fc829fdce222d65cffb4d9423b3", algorithm=MD5
      > User-Agent: curl/7.82.0
      > Accept: */*
      > Content-Length: 18
      > Content-Type: application/x-www-form-urlencoded
      >
      * Mark bundle as not supporting multiuse
      * HTTP 1.0, assume close after body
      < HTTP/1.0 200 OK
      < Date: Sat, 16 Apr 2022 05:52:42 GMT
      < Content-Length: 38
      < Content-Type: text/html; charset=utf-8
      <
      <html><body>secret page</body></html>
      * Closing connection 1

   1. 요청이 auth digest의 경우 아래와 같은 헤더를 부여받아 response를 받는다.

      ``Www-Authenticate: Digest realm="Secret Zone", nonce="TgLc25U2BQA=f410a2789473e18e6587be703c2e67fe2b04afd", algorithm=MD5, qop="auth"``

   2. 다시 요청을 보낼때 아래와 같이 ``cnonce`` , ``reponse`` 를 추가하여 header.Authorization을 전송하는데, 코드상에서 이것을 해석하는 부분은 비어있지만, MD5알고리즘으로 정해진 임의 규칙에 따라 암호화 했다는 것을 알 수 있다.

      ``Authorization: Digest username="june", realm="Secret Zone", nonce="TgLc25U2BQA=f410a2789473e18e6587be703c2e67fe2b04afd", uri="/digest", cnonce="NmIwMjc5NDc5ODkxYmI4MzNiOWVmNTEyMjdjYjgyNDI=", nc=00000001, qop=auth, response="f6159fc829fdce222d65cffb4d9423b3", algorithm=MD5``

쿠키를 통한 세션 관리
^^^^^^^^^^^^^^^^^^^^^

지금은 *BASIC/DIGEST* Auth는 쓰여지지 않는 이유

   - 요청할 때마다 패스워드와 유저 ID를 매회 전송해야 Session을 유지하는 것처럼 보일 수 있다.
   - 사용자 Device를 식별 불가능 하다. (OS의 기능에 대해 분류할 방법이 없다.)

Form, cookie, session token을 활용하는 방식

   - DIGEST 인증과 달리 직접 유저정보를 전송하므로 SSL/TLS가 필수적입니다.
   - 서버는 세션토큰을 생성하고, 토큰을 클라이언트에게 전송합니다.

Signed Cookie
^^^^^^^^^^^^^

DESC

   public key / private key로 데이터를 확인할 수 있는 키를 둘 다 서버만 가지고 있어서, 클라이언트의 세션토큰에 대한 정합성을 서버에서만 확인 할 수 있는 시스템입니다.

PROS

   - 서버 측에서 데이터 저장을 위해 준비할 필요 없이, 즉시 토큰 자체를 암호화된 정보로 취급하면 초기 토큰 생성과정 이후 AUTH에 관련하여 데이터베이스에 접근을 필요로 하지 않는다는 점입니다.
   - micro service 구현시에도 암호화 방식을 공통화 하면, 서비스끼리 같은 세션으로 소통할 수 있다는 장점이 있습니다. (*kerberos*\)

Proxy
-----

`wikipedia.Proxy`
^^^^^^^^^^^^^^^^^

   Proxy 서버는 client request와 server providing resource 중간에서 전달자 역할을 합니다.
   직접 서버에 요청을 연결하는 대신에 프록시 서버에게 연결하도록 하여, request를 분석하고, 요구되는 네트워크 transaction을 수행합니다.

   아래와 같은 효과를 가질 수 있습니다.

      - request의 복잡성을 조절하는 역할
      - 로드 밸런싱, 보안 등의 이점

   Proxies는 분산 시스템에 encapsulation과 구조를 더하는 것으로 소개되었습니다.

   따라서 중간의 자리에서, 암시적으로 request의 원형을 *masking*\하는 효과를 가집니다.

프록시는 *HTTP/1.0* 규격에서 부터 언급되었습니다.

.. code-block:: bash

   GET /helloworld
   Host: localhost:18888
   GET https://example.com/helloworld
   Host: example.com

HEADER.Proxy-Authenticate
^^^^^^^^^^^^^^^^^^^^^^^^^

프록시 서버가 악용되지 않도록 인증을 이용해 프록시 서버를 보호하는 경우도 있습니다.

중계되는 프록시는 중간의 호스트 IP주소를 특정 Header에 기록해 갑니다.

``X-Forwarded-For: client, proxy1, proxy2`` *(RFC 7239)*

``$ curl --http1.0 --proxy http://proxy1.com -U user:pass http://server/helloworld``

HTTPS통신의 프록시 지원은 HTTP1.1에 추가된 *CONNECT* method를 사용합니다.

Cache
-----

| content의 diversity에 따른 데이터 전송량 증가에 따라, 컨텐츠가 변경되지 않았을 때,
| 로컬에 저장된 파일을 재사용함으로써 다운로드 데이터를 줄이고 성능을 높이는 cache 매커니즘이 등장했습니다.

.. note::

   GET, HEAD메서드 이외에는 '기본적으로' 캐시되지 않습니다.

갱신 일자에 따른 캐시
^^^^^^^^^^^^^^^^^^^^^

HTTP/1.0 당시에는 정적 컨텐츠 위주여서 파일의 timestamp가 변경되었는지 비교하는 것으로 데이터 전송여부를 결정할 수 있었습니다.

   Response Header
      ``Last-Modified: Wed, 08 Jun 2016 15:23:45 GMT``

웹 브라우저가 캐시된 URL을 다시 읽을 때에 서버에서 반환된 일시를 그대로 넣어 요청합니다.

   REQUEST HEADER
      ``If-Modified-Since: Wed, 08 Jun 2016 15:23:45 GMT``

위 요청에 따라 데이터가 변경되었다면, 

   - *200 OK*\, 데이터가 변경되어 정상적으로 응답
   - *304 Not Modified*\, 데이터가 변경되지 않았으니 캐시를 사용하라는 응답

ETag
^^^^

날짜와 시간을 이용한 캐시 비교만으로 해결할 수 없을 때가 있습니다.

사용자의 정보나 상태에 따라 부분적인 변경이 한 주소에 동적으로 일어나는 경우가 늘어나거나 하는 등의 경우에, 날짜와 시간을 근거로 캐시의 신선함을 판단하기가 어려워 집니다.

이럴 때 사용할 수 있는 것이 *HTTP/1.1*\에서 추가된 *ETag(Entity tag)*\입니다.

*Wikipedia.ETag*
   파일의 데이터로 비교하여 캐시의 유효성을 검증하는 매커니즘

   - 클라이언트가 조건적인 요청을 전달하는 것을 가능하게 합니다.
   - 대상 URL에 대한 자원이 변경되면 새로운 Etag가 생성되고 전달됩니다.

   
*ETag*\는 서버가 자유롭게 결정해서 반환할 수 있습니다.

   - 아마존 S3의 경우 컨텐츠 파일의 해시 값이 사용되는 것으로 추정됩니다.

Cache-Control
^^^^^^^^^^^^^


