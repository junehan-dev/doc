Evchat-2205-nodejs
==================

.. image:: https://lh3.googleusercontent.com/pw/AM-JKLUmHa6iGCQyfEVxcQKDqqYr5yQFgW6X9XSlDCXVdaFxuupzaYHrOpLhK8nqZtg9NpH6uocFtEMMh6fiKFFP5SlYHq_DRTCSw3qlR9zAV1F_4bzGHyVDZlWtTwvjC4xIJjvEgdxgt6LqzaxZ2KxTcChl=w174-h190-no?authuser=0

https://www.youtube.com/watch?v=w-0YWDXmW0I

architecture
------------

:ROOM API SERVER:

   - 방의 생성과 관련된 API
   - 검색과 관련한 기능을 위해 방을 생성할때 태그, 카테고리, 지역 등의 추가 정보가 존재한다.

:AUTH API SERVER:

   - 모든 서버에서 통용되는 JWT TOKEN을 발급하기 위한 서버로 Social provider의 인증을 기준으로 registration이 가능하다.
   - AccessToken 30m의 유효기간을 가진다.
   - RefreshToken 2D의 유효기간을 가진다.

:CHAT API SERVER:

   - ROOM API SERVER에서 생성된 방에 대한 검증을 전제하여 join이 가능한지 불가능한지를 검사한다.
   - expire일자 기준 참여중인 유저들에게 leave이벤트가 발생한다.

.. image:: https://lh3.googleusercontent.com/pw/AM-JKLXOkS2YrcwCNhQPv7xwm8XmBsHlIuFDSuUly4D7_yFZzGqy58U1z99IwGY4x25V4CEp9iQ1npVa_S7Bz2-FgBGdXWzyGpFkEUthf9IwYpYy2eD6_fetTa8R3W8NSo-PIIk_4R3mhOi6y2tFADm9ID1_=w731-h681-no?authuser=0

.. image:: https://lh3.googleusercontent.com/pw/AM-JKLWeN6NdRgGpmgHJANUWAWSsIUmoQGZBGQejlhaY7qm11bB228eqvXEnjrQRoGmsIdyLCFZLW-ZiDE0pGWQMPjx6MaK_r6YO8VjS5KqWnF8mvNWfLYreL5r9m0ufYCBWhAd_Tsw0gOB-7QyB06UYN2Bh=w2100-h1296-no?authuser=0


