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

전자메일 HTTP의 기원
^^^^^^^^^^^^^^^^^^^^

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

Receiving Header 
^^^^^^^^^^^^^^^^

.. todo::

   1.4.2 헤더 수신

DNS Server
----------

우리가 주소창에 문자로 구성된 Domain name을 입력했을 때,
먼저 ISP가 가진 DNS resolver를 통해 .com, .org와 같은 도메인의 정보담당하는 서버에게서 해당 domain name정보에 대한 담당주소지를 할당 받을 수 있습니다.

전화번호부가 권역 규모별로 나눠져있는 것과 같습니다.

사용하는 도메인 네임을 구매한 회사가 있다면, ISPN에서 해당 도메인 네임을 관리하는 회사의 서버까지 방문한 뒤, 해당 도메인을 IP주소로 전달 받게 됩니다.

.. image:: https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png

1. 브라우저를 열고 주소를 www.example.com으로 입력합니다.
2. www.example.com 은 DNS resolver에게 향하는데, 주로 IPS가 관리하는 서버입니다.
3. DNS resolver는 DNS root name server로 향하게 합니다.
4. 이때 TLD, .com에 대한 name server가 example에 대한 주소를 발견하고 해당 주소가 aws route53 name server과 담당하고 있다는 것으로 응답을 종료합니다.
5. Route 53 네임 서버가 example.com의 hosted zone을 탐색하고, www.example.com레코드를 발견합니다.
   관련 정보, IP주소 같은 정보를 취득 후에 DNS Resolver에게 전달하는 것으로 응답을 종료합니다.
6. 이것으로 해당 IPv4주소는 유한한 정보이기 때문에, 국가별로 구매한 번호의 Range가 있습니다. 그런 기본원리를 바탕으로 network를 통해 이 주소에 대한 길을 따라 요청정보가 패킷단위로 분산되어 전달되고, 마찬가지로 응답정보도 분산되어 돌아오는 것을 누적시켜 브라우저에 최종적으로 출력하는 것으로 도메인에 대한 해석이 종료됩니다.
   
