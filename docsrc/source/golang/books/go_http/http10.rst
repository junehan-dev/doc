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

.. code-block:: sh

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


