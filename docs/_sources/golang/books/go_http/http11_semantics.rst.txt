Semantics of Http/1.1
=====================

Remote Procedure Call
----------------------

Procedure
   이전에 단일 개념으로는 *subroutine*\으로 불렸던 개념으로, '일련의 처리가 나열된 처리의 집합'을 의미합니다.

Subroutine (from wikipedia)
   프로그램 명령어들의 연속성으로 특정한 task를 수행하고, unit으로서 패키지화 되어 있습니다.
   이 유닛은 해당 task가 수행되어야 하는 시점이면 어디에서건 사용될 수 있습니다.

Remote Procedure Call (from wikipedia)
   분산 환경컴퓨팅에서, RPC는 컴퓨터 프로그램이 procedure(subroutine)을 다른 주소공간에서(주로 다른 컴퓨터 혹은 공유 네트워크) 실행되도록 생성할 때,
   마치 일반적인 함수 호출처럼 코드가 된 것으로 프로그래머가 원격 정보교환에 대한 세부사항을 명시적으로 코드화할 필요 없는 것을 포함합니다.

.. note::

   자신의 컴퓨터 안에 있는 것처럼 호출하고, 필요에 따라 반환값을 받는 구조

JSON-RPC
^^^^^^^^

https://www.jsonrpc.org/specification

2000년 중반까지 XML, SOAP 포멧의 RPC가 있었으나, XML양식(schema)을 기반으로 하기 때문에

단순한 RPC를 실행하고 싶은 뿐이라도 거대한 코드를 작성해야 했습니다.

JSON-RPC
   XML대신 JSON을 이용한 원격 procedure call입니다.
   JSON-RPC는 SOAP와 대조적으로 단순하게 라는 방침을 내새웠습니다.
 
.. code-block:: sh

   POST /rpc HTTP/1.1
   HOST: api.rpcserver.com
   Content-Type: application/json
   Content-Length: 57
   Accept: application/json

   {
      "version": "2.0",
      "method": "update",
      "params": [1,2,3,4,5],
      "id": "3"
   }


   HTTP/1.1 200 OK
   Content-Type: application/json
   Content-Length: 85
   {
      "version": "2.0",
      "result": [4,5,6,7,8],
      "id": "3",
      "error":null
   }

___


.. note::

   - 방법은 대부분 POST를 사용하는 것이 바람직하다고 합니다.
   - 각 메세지에 버전 지정이 필요합니다.
   - ``method``\, ``params``\로 메서드에 대응하여 전달할 인수를 지정합니다.
   - 반환 값은 사용한 id와 값 자체가 result에 저장됩니다.
   - 오류 시에는 ``error``\에 정보가 저장됩니다.

XML-RPC는 항상 200의 status code로 처리했지만,
JSON-RPC는 이 밖에도 많은 status code를 정했습니다.

   - 200 OK: 정상 종료
   - 204 No Response / 202 Accepted: Notification 시의 반환값
   - 307 Temporary Redirect / 308 Permanenet Redirect: redirect(자동 재전송은 하지 않는다.)
   - 405 Method Not Allowed: GET메서드가 지원되지 않는 곳에 GET을 사용했다.
   - 415 Unsupported Media Type: Content-Type application/json이 아님
   

