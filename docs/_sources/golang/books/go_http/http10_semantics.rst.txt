Semantics of Http/1.0
=====================

**Timeless, 4-core components of HTTP** 

   - Method, URL(domain + path)
   - Header
   - Body
   - Status code

.. important::

   HEADER!!!
   

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

Redirect using Form
-------------------

.. todo::
 
   220413.http1.0/semantics/form redirect/

