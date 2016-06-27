.. _install:

Installation
============

이 문서는 Requests의 설치를 담고있습니다.
어떠한 소프트웨어 패키지를 이용하던지 가장 첫번째 순서는 재대로 설치하는것 입니다.

Pip Install Requests
--------------------

Requests를 설치하는 간편한 방법은 terminal에서 아래의 코드를 쓰는것입니다::

    $ pip install requests

만약 `pip <https://pip.pypa.io>`_ 을 설치하지 않았다면,
`this Python installation guide <http://docs.python-guide.org/en/latest/starting/installation/>`_ 를 따라 설치할수 있습니다.



Get the Source Code
-------------------

Requests는 GitHub에서 개발되고 있습니다. 코드는 `이곳 <https://github.com/kennethreitz/requests>`_ 에서 언제든 받을 수 있습니다.

또한 public repository를 복사 할수 있습니다 ::

    $ git clone git://github.com/kennethreitz/requests.git

혹은, `tarball <https://github.com/kennethreitz/requests/tarball/master>`_ 에 접속하여 다운로드 받을수 있습니다.::

    $ curl -OL https://github.com/kennethreitz/requests/tarball/master
      # optionally, zipball is also available (for Windows users).

한번 소스를 복사했다면 파이썬 패키지에 끼워 넣을 수 있고 또한 site-package에 쉽게 설치할수 있습니다.::

    $ python setup.py install
