Syntax of Http/1.0
==================

History of Http
---------------
terminology for http protocol
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   - *IETF* (The internet Engineering Task Force)
      인터넷의 접속성을 향상시키는 것을 목적으로 만들어진 임의 단체
   - *RFC* (Request For Comments)
      IETF가 만든(접속성을 위한) 프로토콜(데이터 형식) 규약 문서
   - *IANA* (Internet Assigned Numbers Authority)
      Web database (well-known port number, content-type, 등) 관리단체
   - *W3C* (World Wide World Consortium)
      웹 관련 표준화를 담당하는 비영리 단체 (통신 프로토콜/알고리즘)
   - *WHATTWG* (Web Hypertext Application Technology Working Group)
      웹 관련 규격을 논의하는 단체
            
   
HTTP/0.9
--------

HTTP/0.9
   매우 단순한 프로토콜, 해당 경로의 서버에 해당 문서를 가져오는 프로토콜

   .. note::

      0.9 버전은 gohttp에서 지원하지 않고 1.x 이상 버전과 해과 하위 호환성이 없으므로 송수신 모두 0.9버전을 다룰 수 없기 때문에 1.0으로 테스트합니다.

.. literalinclude:: srcs/01_http10/01_go_echo_server.go
   :language: go 
   :linenos:
   :lines: 1-26
   :encoding: latin-1

.. code-block:: bash

   $curl --http1.0 --verbose http://127.0.0.1:18888/greeting

   *   Trying 127.0.0.1:18888...
   * Connected to 127.0.0.1 (127.0.0.1) port 18888 (#0)
   > GET /greeting HTTP/1.0
   > Host: 127.0.0.1:18888
   > User-Agent: curl/7.82.0
   > Accept: */*
   >
   * Mark bundle as not supporting multiuse
   * HTTP 1.0, assume close after body
   < HTTP/1.0 200 OK
   < Date: Mon, 11 Apr 2022 13:41:58 GMT
   < Content-Length: 32
   < Content-Type: text/html; charset=utf-8
   <
   <html><body>hello</body></html>
   * Closing connection 0

HTTP/0.9 to HTTP/1.0
--------------------

HTTP 0.9의 기능::

   - 하나의 문서를 전송하는 기능 밖에 없었다.
   - 통신되는 문서는 HTML content만 가능했다.
   - 클라이언트 쪽에서 검색 이외의 요청을 보낼 수 없었다.
   - 새로운 문장을 정송하거나 갱신 또는 삭제 할 수 없었다.

전자메일, 뉴스그룹 프로토콜의 기능을 도입하면서 업데이트가 이루어 졌습니다.

전자메일: HTTP의 기원
---------------------

.. code-block:: text

   MIME-Version: 1.0
   Received: by 10.176.69.212 with HTTP; Wed, 6...
   From:
   Date:
   Message-ID: <CAAw3=...>
   Subject: hi
   To: yoshiki@shibu.jp
   Content-Type: text/plain; charst=UTF-8

   hello

- header에 추가된 내용::

   - 송신자 
   - 버전
   - 보낸 시간

- HTTP에도 차용된 내용

   - client request

      1. User-Agent
      #. Referer
      #. Authorization
   
   - server response

      1. Content-Type
      #. Content-Length
      #. Content-Encoding
      #. Date

.. literalinclude:: srcs/curl_cmds/http10_multi_header.sh
   :language: sh
   :linenos:
   :lines: 1-3
   :encoding: latin-1


Header의 전송과 수신
--------------------
 
.. note::

   - Header 끝에 빈 줄을 넣으면 그 줄 아래로 전부 바디가 됩니다.
   - 바디 기준으로 읽어야할 바이트 수는 Header.Content-Lenght에 명시되어 있습니다.

*Content-Type*
   바디의 데이터 종류에 대해 특정할 때 지정된 *MIME-식별자타입*\을 사용합니다.

*MIME Type*
   | 어떤형식의 데이터로 body를 해석해야하는지 정형화된 규약이 정의되어 있습니다.
   | (https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
   | 전자메일을 위해 만들어 졌으며, 이는 웹(www)이 등장하기 훨씬 이전의 규격입니다.
   | 당시에는 POSTSCRIPT, TEX같은 텍스트파일들을 가정했지만, 지금헤더에서처럼 동일하게 *X-*\로 시작하는 키워드로 미지의 대상을 기술 할 수 있었습니다.
   | *다음과 같은 헤더를 전송해 브라우저가 추측하도록 지시하지 않는 것이 현재 주류의 방법입니다.*
      | ``X-Content-Type-Options: nosniff``


뉴스그룹: HTTP의 기원
---------------------

뉴스 그룹은 분산 아키텍처로 되어 있습니다.

::

   복수의 서버가 master/slave구조로 역할이 분배되어 있습니다.
   슬레이브 서버 또한 마스터 서버에 접속해 정보를 *shard db* 같은 개념으로 가져왔던 것으로 보입니다.
   이 계층구조 서버간의 통신 프로토콜이 *NNTP*\[*]입니다.
   뉴스그룹은 method와 status code 두 기능을 남겨주었습니다.

Method
^^^^^^

.. todo::

   뉴스그룹/메서드, 스테이터스 코드

DNS Server
----------

우리가 주소창에 문자로 구성된 Domain name을 입력했을 때,
먼저 ISP가 가진 DNS resolver를 통해 .com, .org와 같은 도메인의 정보담당하는 서버에게서 해당 domain name정보에 대한 담당주소지를 할당 받을 수 있습니다.


전화번호부가 권역 규모별로 나눠져있는 것과 같습니다.

사용하는 도메인 네임을 구매한 회사가 있다면, ISPN에서 해당 도메인 네임을 관리하는 회사의 서버까지 방문한 뒤, 해당 도메인을 IP주소로 전달 받게 됩니다.

.. image:: https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png

1. 브라우저를 열고 주소를 www.example.com으로 입력합니다.
2. www.example.com 은 DNS resolver에게 향하는데, 주로 ISP가 관리하는 서버입니다.
3. DNS resolver는 DNS root name server로 향하게 합니다.
4. 이때 TLD, .com에 대한 name server가 example에 대한 주소를 발견하고 해당 주소가 aws route53 name server과 담당하고 있다는 것으로 응답을 종료합니다.
5. Route 53 네임 서버가 example.com의 hosted zone을 탐색하고, www.example.com레코드를 발견합니다.
   관련 정보, IP주소 같은 정보를 취득 후에 DNS Resolver에게 전달하는 것으로 응답을 종료하고 사용자는 IP주소를 획득하게 됩니다.
   
.. [*] Network News Transfer Protocol, 네트워크 뉴스 전송 프로토콜

