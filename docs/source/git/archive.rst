Archive
=======

.. highlights:: HOW TO export a git project

What does git Archive do?
-------------------------
``git-archive`` 명령은 archive file을 특정한 commits, brances, or trees등의 git Refs에서부터 생성하는 유틸리티이다.
``git-archive`` 는 addition arguments에 따라 archive output을 변경할 수 있다.

Git export examples
-------------------

Most basic
   .. code-block:: bash

      git archive --format=tar HEAD

이 커맨드가 실행되었을 때, current HEAD ref of repo에서 부터 archive를 생성한다.
By Defaulut, ``git archive``는 생성한 아카이브를 ephemeral(임시적인) stdout stream으로 파이프하여 stream할 것이다.
그리고 당신은 이 output stream을 영속적인 파일로 capture해야 할 것이다.
이 영속적인 파일 명을 ``--output`` 옵션을 통해서 명시하거나, stdout을 파이프라이닝하여 redrection하여 저장할 수 있다.

   .. code-block:: bash

      git archive --output=./git_HEAD_ref_0.2b.tar --format=tar HEAD


``--format`` 옵션은 흔한 압축파일 포멧인 zip, tar.gz를 수용한다.
둘 중 하나의 포멧을 옵션에 전달하면, 압축된 아카이브를 얻을 수 있다.
포멧을 명시하지 않으면 ``--output`` 옵션에 의해 추측(inferred)될 것이다.
repo의 부분적인 아카이브는 ``path argument`` 를 전달 함으로 생성 될 수 있다.

   .. code-block:: bash

      git archive --output=./git_HEAD_ref_0.2r.tar --format=tar HEAD ./build
      
위의 케이스에서 ./build 디렉터리를 매개변수로 전달하였다. 이 커맨드는 ``./build`` 디렉터리 안의 파일들만 전달할 것이다.

Options
-------

이전의 예제에서 ``git-archive`` 프로그램에서 주로 사용되는 케이스를 설명하였다.
이후에는 추가적인 옵션이다.

``--prefix=<prefix>/``

   prefix옵션은 생성한 아카이브 내부 파일에 붙는다. 이것은 아카이브의 contents를 고유한 네임스페이스 아래로 추출되도록 하는데 도움이 된다.

``--remote=<repo>``

   이 옵션은 remote하는 repo의 url을 기대한다.
   이 옵션과 함께 호출 될때, ``git-archive`` 는 명시된 remote-repo를 fetch하고, 아카이브를 명시한 ref에서 생성할 것이다.

   .. hint::
      만약 해당 remote-repo와 remote-repo:ref둘다 유효하다면!

Configuration
-------------

git-global-conf-values중 ~git archive~가 참조할 필드들이 있다. 이들은 ``git config`` 유틸리티를 사용하여 셋업된다.

.. code-block:: bash

   tar.umask
   tar.<format>.command
   tar.<format>.remote

``tar.umask``

   unmask config option은 유닉스 레벨 권한-비트 제한을 출력 archive파일에 명시한다.

``tar.<format>.command``

   이 설정 옵션은 ``git-archive`` 출력이 동작할 커스텀 쉘 명령을 정의한다.
   이것은 ``--output`` 옵션을 생략하는 것과 유사하고, ``git-archive`` 의 stdout stream을 커스텀 툴로 piping한다.

``tar.<format>.remote``

   만약 이것을 enabled되면, 리모트 클라이언트들이 ``<format>`` 타입의 아카이브를 fetch하도록 허락한다.


Summary
-------

git-archive는 배포용 패키지를 깃-repo에서 생성하는데 유용한 프로그램이다.
repo의 특정 레퍼런스를 타겟으로 할 수 있고, 해당 ref의 content만을 패키징 할 수 있다.
git archive는 더해진 압축을 기능화할 수 있는 several output formats를 가지고 있다.
