Requests: HTTP for Humans
=========================

.. image:: https://img.shields.io/pypi/v/requests.svg
    :target: https://pypi.python.org/pypi/requests

.. image:: https://img.shields.io/pypi/dm/requests.svg
        :target: https://pypi.python.org/pypi/requests

Requests 는 사람이 이해하기 쉬운 순수한? HTTP library입니다.
**경고:** Recreational 다른 HTTP라이브러리의 사용은 다음과 같은 위험성을 내포할수 있습니다.
보안 취약점, 장황한 코드, 쓸데없는 시간낭비,
끊임없는 문서를 읽기, 짜증, 두통, 심지어 죽음까지 포함되어 있을 수 있습니다.

다음은 만능 Requests 사용법입니다.

.. code-block:: python

    >>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
    >>> r.status_code
    200
    >>> r.headers['content-type']
    'application/json; charset=utf8'
    >>> r.encoding
    'utf-8'
    >>> r.text
    u'{"type":"User"...'
    >>> r.json()
    {u'disk_usage': 368627, u'private_gists': 484, ...}

Requests를 사용하지 않은 유사코드는 `여기 <https://gist.github.com/973705>`_서 보실 수 있습니다.

Requests는 당신이 손쉽게 거대한 파일을 HTTP/1.1 requests로 보내는것을 가능케 해줍니다.
당신이 원하는 url에 쿼리를 추가하거나 form-encode된 POST 데이터를 수동으로 추가할 필요가 없습니다.
Requests를 이용하여 HTTP connection pooling 을 자동으로 유지할수있습니다.
HTTP connection pooling과 Keep-alive는 Requests가 urllib3를 이용하여 100% 자동으로 이루어집니다.

게다가 어린 아이들도 쉽게 사용할 수 있습니다. Requets는 Python 패키지중 가장 많은 다운로드를 갖고 있으며,
매달 700만건 이상 다운로드 하고있습니다. 이용해보고 나면 아마 당신은 Requests를 계속해서 이용할 것입니다.

Feature Support
---------------

Requests는 최신 web에 대한 준비가 되어 있습니다.

-- 국제적 도메인과 URLs
-- Keep-Alive & Connection Pooling
-- 세션과 쿠키
-- 브라우저와 같은 SSL을 이용한 연결
-- 기존/경량화된 인증
-- 우아한 Key/Value 쿠키
-- 자동 압축해제
-- 컨텐츠 자동 디코딩
-- Respone 바디의 유니코드화
-- Multipart 파일 업로드
-- HTTP(S) Proxy 지원
-- Connection Timeouts
-- 스트리밍 다운로드
-- ``.netrc`` 지원
-- 파편화된 Requests
-- 스레드 안정성

Requests는 Python 2.6 — 3.5, 그리고 PyPy를 지원합니다.

Installation
------------

Requests의 설치는 다음과 같습니다.

.. code-block:: bash

    $ pip install requests
    ✨🍰✨

확실히 만족할 것입니다.
Documentation
-------------

Fantastic documentation is available at http://docs.python-requests.org/, for a limited time only.
환상적인 문서는 http://docs.python-requests.org/ 에서 볼 수 있습니다.

How to Contribute
-----------------

#. 이미 오픈된 이슈 혹은 새로 시작된 이슈에서 새로운 아이디어나 버그에 대해 토론하세요.
`Contributor Friendly`_ 태그는 아직 코드가 익숙하지 않은 사용자들을 위한 이슈입니다.
#. GitHub의 `the repository`_를 Fork하고 **master** 브랜치에 수정을 하세요.
#. 테스트를 작성하고 버그를 고쳤다는 것을 보여주시거나 이를 이용해 기대되는것을 보여주세요.
#. 풀리퀘스트와 버그를 보내주세요 메인테이너가 합치고 발행할것입니다. 그리고 스스로 AUTHORS_ 에 자신을 추가하세요.

.. _`the repository`: http://github.com/kennethreitz/requests
.. _AUTHORS: https://github.com/kennethreitz/requests/blob/master/AUTHORS.rst
.. _Contributor Friendly: https://github.com/kennethreitz/requests/issues?direction=desc&labels=Contributor+Friendly&page=1&sort=updated&state=open
