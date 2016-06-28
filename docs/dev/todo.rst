How to Help
===========

Requests는 활발한 개발자와 컨트리뷰터등 많은 참여를 환영합니다.

#. open isssues 혹은 새로운 이슈를 열어 버그에대한 토론을 시작합니다.
   Contributor Friendly 태그가 달린 이슈는 사람들에게 아직 코드가 친근하지 않은 사람들에게 이상적입니다.
#. GitHub에서 `the repository <https://github.com/kennethreitz/requests>`_ 를 포크하여 당신이 변경한 새로운 branch를 만드세요.
#. 버그를 고쳤다는것은 보여줄수있는 테스트를 작성하세요.
#. 메인 테이너가 merged 하거나 공개될때까지 버그와 pull request를 보내주세요
   반드시 `AUTHORS <https://github.com/kennethreitz/requests/blob/master/AUTHORS.rst>`_ 에 자신을 추가하세요.

Feature Freeze
--------------

v1.0.0 , Requests는 feature freeze상태에 들어서 있습니다.
새로운 특징과 Requests 와 Pull Requests 구현은 받아들여지지 않을수 있습니다.
새로운 기능과 Pull Requests로 받은 구현들은 받아지지 않을 수 있습니다.

Development Dependencies
------------------------

당신은 Requests의 테스트를 구동하기위해 py.test를 설치해야 합니다. ::

    $ pip install -r requirements.txt
    $ py.test
    platform darwin -- Python 2.7.3 -- pytest-2.3.4
    collected 25 items

    test_requests.py .........................
    25 passed in 3.50 seconds

Runtime Environments
--------------------

Requests가 현재 지원하는 Python 버전은 다음과 같습니다.

- Python 2.6
- Python 2.7
- Python 3.1
- Python 3.2
- Python 3.3
- PyPy 1.9

Python 3.1과 3.2는 언제든 지원이 종료될수 있습니다.
Google App Engine 공식적으로 지원하지 않습니다. 호환성을위한 Pull Requests는 받아질수 있습니다만 그들이 코드를 복잡하게 만드는것은 원치 않습니다.

Are you crazy?
--------------

- C extensions 없이 SPDY 지원은 엄청난것입니다.

Downstream Repackaging
----------------------

만약 니가 Requests 리패키징을 원한다면, 당신은 올바른 SSL기능을 위해 ``cacerts.pem`` 파일을 재배포해야합니다.