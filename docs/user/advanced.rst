.. _advanced:

Advanced Usage
==============

Requests를 좀더 활용할수 있는 방법을 담은 문서입니다.
.. _session-objects:

Session Objects
---------------

세션 오브젝트는 연결 되어 있는동안 파라메터들을 포함한 리퀘스트를 전송 하는 것을 허락합니다.
이것은 ``urllib3`` 에 `connection pooling`_ 을 이용하여 모든 리퀘스트가 세션대신 만든 쿠키를 포함합니다.
그래서 당신이 같은 호스트에 여러 리퀘스트들을 만들었다면 근원적인 TCP연결을 재사용하여
중요한 퍼포먼스 증가를 보줄것입니다.(`HTTP persistent connection`_ 을 참고하세요).


세션 오브젝트는 Requests의 주요 API의 모든 메소드를 갖고있습니다.

cookie를 이용하여 계속하여 리퀘스트를 전송해 봅시다 ::

    s = requests.Session()

    s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
    r = s.get('http://httpbin.org/cookies')

    print(r.text)
    # '{"cookies": {"sessioncookie": "123456789"}}'



세션도 또한 데이터를 포함하여 리퀘스트 메소드를 전송할 수 있습니다.

세션 오브젝트를 사용하여 데이터를 포함하여 전송해 봅시다::

    s = requests.Session()
    s.auth = ('user', 'pass')
    s.headers.update({'x-test': 'true'})

    # both 'x-test' and 'x-test2' are sent
    s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})



어떠한 dict형도 세션레벨의 값들을 포함해서 requests 메소드에 보낼수 있습니다.
메소드 레벨의 파라메터들은 세선 파라메터로 오버라이드 됩니다.


그러나, 세션을 사용하더라도 메소드 레벨의 파라메터들은 계속해서 리퀘스트를 보내지 않습니다.
첫 리퀘스트로 쿠키를 전송하는 예제입니다, 두번째는 전송하지 않습니다. ::

    s = requests.Session()

    r = s.get('http://httpbin.org/cookies', cookies={'from-my': 'browser'})
    print(r.text)
    # '{"cookies": {"from-my": "browser"}}'

    r = s.get('http://httpbin.org/cookies')
    print(r.text)
    # '{"cookies": {}}'



만약 당신이 수동으로 쿠키를 session에  추가할수 있습니다.
:attr:`Session.cookies <requests.Session.cookies>` 를 이용하여 :ref:`Cookie utility functions <api-cookies>` 를 다룰수 있습니다.

세션은 또한 context manager로 이용할수 있습니다.::

    with requests.Session() as s:
        s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')


이것은 처리불가능한 예외가 발생하더라도 ``with`` 블럭이 끝날때 세션을 끝냅니다.

.. admonition:: Remove a Value From a Dict Parameter

    때때로 당신은 Dict 파라메터에서 세션레벨의 키를 생략하고 싶을때가 있을것입니다.
    간단하게 메소드레벨의 파라메터 키값을 ``None`` 으로 하면 자동적으로 생략될것입니다.


모든 값은 세션안에 당신이 접근가능하게 포함되어있습니다.
:ref:`Session API Docs <sessionapi>` 에서 확인해보세요.

.. _request-and-response-objects:

Request and Response Objects
----------------------------

첫째로, 당신은 서버에 보내기위한 리퀘스트 혹은 쿼리를 ``Request`` 오브젝트 를 구성할수 있습니다.
언제던 ``requests.get()`` 을 만들어 부를수 있습니다. 그리고 중요한 친구인 두개도 같이 부를수있습니다.
두번째로, ``Response`` 오브젝트는 하나의 ``requests`` 의 서버에서 온 응답으로 얻을 수 있습니다.
응답 오브젝트는 서버에서 온 모든 정보를 포함하고있습니다.
그리고 또한 본래의 ``Request`` 오브젝트를 포함하고 있습니다.
여기 간단한 Wikipedia의 서버에서 중요한 정보를 가져오는 리퀘스트 예제가 있습니다.::

    >>> r = requests.get('http://en.wikipedia.org/wiki/Monty_Python')

만약 당신이 서버가 우리에게 보낸 헤더에 접근하기 원한다면, 다음과 같이 할 수 있습니다::

    >>> r.headers
    {'content-length': '56170', 'x-content-type-options': 'nosniff', 'x-cache':
    'HIT from cp1006.eqiad.wmnet, MISS from cp1010.eqiad.wmnet', 'content-encoding':
    'gzip', 'age': '3080', 'content-language': 'en', 'vary': 'Accept-Encoding,Cookie',
    'server': 'Apache', 'last-modified': 'Wed, 13 Jun 2012 01:33:50 GMT',
    'connection': 'close', 'cache-control': 'private, s-maxage=0, max-age=0,
    must-revalidate', 'date': 'Thu, 14 Jun 2012 12:59:39 GMT', 'content-type':
    'text/html; charset=UTF-8', 'x-cache-lookup': 'HIT from cp1006.eqiad.wmnet:3128,
    MISS from cp1010.eqiad.wmnet:80'}


그러나, 만약 우리가 서버로 보낸 헤더를 얻기원한다면,
우리는 쉽게 다음과 같이 request의 헤더에 접근할수 있습니다. ::

    >>> r.request.headers
    {'Accept-Encoding': 'identity, deflate, compress, gzip',
    'Accept': '*/*', 'User-Agent': 'python-requests/1.2.0'}

.. _prepared-requests:

Prepared Requests
-----------------

API 콜 또는 Session 콜을 통하여 :class:`Response <requests.Response>` 를 받을때
``request`` 는 실제로 이전에 사용된 ``PreparedRequest`` 의 결과로 봅니다.
당신은 request를 보내기전에 바디나 헤더 혹은 어디서든 추가적인 일을 원할 수 있습니다.
그럴땐 다음과 같이 해보세요.::

    from requests import Request, Session

    s = Session()

    req = Request('POST', url, data=data, headers=headers)
    prepped = req.prepare()

    # do something with prepped.body
    prepped.body = 'No, I want exactly this as the body.'

    # do something with prepped.headers
    del prepped.headers['Content-Type']

    resp = s.send(prepped,
        stream=stream,
        verify=verify,
        proxies=proxies,
        cert=cert,
        timeout=timeout
    )

    print(resp.status_code)


``Request`` 오브젝트에 당신이 아무것도 안할때부터, 그리고 ``PreparedRequest`` 오브젝트를 수정한 후에도
다른 파라메터를 포함하여 ``requests.*`` 나 ``Session.*`` 을 보낼수 있게 준비 되어있습니다.

그러나, 코드를 넘어서 리퀘스트의 :class:`Session <requests.Session>` 오브젝트는 작은 이점을 갖고있습니다.
상태를 적용한 상태로 :class:`PreparedRequest <requests.PreparedRequest>` 를 갖을 수 있습니다.
다음과 같이 :meth:`Request.prepare() <requests.Request.prepare>` 를 부르는 대신
:meth:`Session.prepare_request() <requests.Session.prepare_request>` 를 부를 수 있습니다.::

    from requests import Request, Session

    s = Session()
    req = Request('GET',  url, data=data, headers=headers)

    prepped = s.prepare_request(req)

    # do something with prepped.body
    prepped.body = 'Seriously, send exactly these bytes.'

    # do something with prepped.headers
    prepped.headers['Keep-Dead'] = 'parrot'

    resp = s.send(prepped,
        stream=stream,
        verify=verify,
        proxies=proxies,
        cert=cert,
        timeout=timeout
    )

    print(resp.status_code)

.. _verification:

SSL Cert Verification
---------------------

Requests는 HTTPS리퀘스트의 SSL 인증서를 웹브라우저처럼 확인합니다.
기본적으로,SSL 인증서를 사용할때, 인증 불가능한 인증서라면 requests는 SSLError를 던질것입니다. ::

    >>> requests.get('https://requestb.in')
    requests.exceptions.SSLError: hostname 'requestb.in' doesn't match either of '*.herokuapp.com', 'herokuapp.com'

저는 이 도메인에 SSL을 설정하지 않았습니다. 따라서 예외를 던질것입니다. GitHub는 다음과 같이 던질것입니다.::

    >>> requests.get('https://github.com')
    <Response [200]>


당신은 CA_BUNDLE파일 혹은 디렉토리를 신뢰할수 있는 인증서를 이용하여 인증을 통과 할 수 있습니다.::

    >>> requests.get('https://github.com', verify='/path/to/certfile')

.. note:: 만약 ``verify`` 가 해당 디렉토리에 위치되어있다면,
    디렉토리는 c_rehash utility를 OpenSSL를 이용하여 처리될 것 입니다.

이 신뢰되는 CA의 리스트는 명시된 ``REQUESTS_CA_BUNDLE`` 환경 변수를 통하여 이용할 수 있습니다.
Requests ``verify`` 를 False로 설정했다면 또한 SSL 인증서를 거부할수 있습니다. ::

    >>> requests.get('https://kennethreitz.com', verify=False)
    <Response [200]>


기본적으로 ``verify`` 는 True로 설정 되어있습니다. 선택 가능한 ``verify`` 는 host의 인증서 뿐입니다.
당신은 또한 클라이언트가 사용할 인증서를 하나의 파일(개인키 와 인증서를 포함하고있는)
또는 두개의 파일을 튜플로 명시할수있습니다.::

    >>> requests.get('https://kennethreitz.com', cert=('/path/client.cert', '/path/client.key'))
    <Response [200]>

만약 당신이 경로를 잘못 명시했거나 유효하지 않은 인증서를 설정했을경우 SSLError를 얻을 것 입니다::

    >>> requests.get('https://kennethreitz.com', cert='/wrong_path/client.pem')
    SSLError: [Errno 336265225] _ssl.c:347: error:140B0009:SSL routines:SSL_CTX_use_PrivateKey_file:PEM lib

.. warning:: 개인키는 반드시 복호화해서 저장해 두세요.
   현재 requests는 키복호화를 지원하지 않습니다.

.. _ca-certificates:

CA Certificates
---------------

기본적으로 Requests에 함께 설정된 신뢰할수 있는 root CA들 `Mozilla trust store`_ 에 있는 인증서들입니다.
그러나, Requests버전에 한번씩만 업데이트 됩니다.
이것은 만약 당신이 Requests 한가지의 버전만을 사용한다면, 만료된 인증서가 포함되어 있음을 의미합니다.
Requests 2.4.0버전부터, Requests는 현재 사용하는 시스템의 `certifi`_ 를 이용 하고 있습니다.
이것은 유저가 스스로 신뢰할수있는 인증서를 코드를 바꾸지 않고 그들의 시스템에서 업데이트 할수 있습니다.
보안을 위하여 인증서를 자주 업데이트하길 권장합니다.

.. _HTTP persistent connection: https://en.wikipedia.org/wiki/HTTP_persistent_connection
.. _connection pooling: https://urllib3.readthedocs.io/en/latest/pools.html
.. _certifi: http://certifi.io/
.. _Mozilla trust store: https://hg.mozilla.org/mozilla-central/raw-file/tip/security/nss/lib/ckfw/builtins/certdata.txt

.. _body-content-workflow:

Body Content Workflow
---------------------

기본적으로 request를 만들때 응답의 body를 즉시 다운로드받습니다.
당신은 이 행동을 override 할 수 있습니다.
그리고 respone의 바디를 ``stream`` 파라메터를 이용하여 :class:`Response.content <requests.Response.content>` 에 접근
할때 까지 다운로드하는 것을 연기 할수 있습니다.::

    tarball_url = 'https://github.com/kennethreitz/requests/tarball/master'
    r = requests.get(tarball_url, stream=True)

이러한 관점에서 다운로드된 응답 헤더들과  연결되어있는 상태에서 조건문을 이용하여 컨텐츠를 검색할수 있습니다. ::

    if int(r.headers['content-length']) < TOO_LONG:
      content = r.content
      ...

당신은 또한 :class:`Response.iter_content <requests.Response.iter_content>` 와
:class:`Response.iter_lines <requests.Response.iter_lines>` 메소드를 이용하여 작업을 컨트롤 할 수 있습니다.
그대신, 당신은 decoded 되지 않은 바디를 urllib3의 :class:`urllib3.HTTPResponse <urllib3.response.HTTPResponse>` 의
:class:`Response.raw <requests.Response.raw>` 를 이용하여 읽을 수 있습니다.

request를 만들때 ``stream`` 을 ``True`` 로 설정했다면,
Requests는 당신이 모든 데이터를 소진할때까지 또는 :class:`Response.close <requests.Response.close>` 를
부를때까지 연결을 유지하고 있을 것입니다. 이것은 연결의 비효율을 야기합니다.
만약 ``stream=True`` 를 사용하는 동안에 requests의 body의 전체가 아닌 일부를 읽고 싶다면
아래와 같이 ``contextlib.closing`` (`documented here`_) 을 사용하시길 바랍니다.::

    from contextlib import closing

    with closing(requests.get('http://httpbin.org/get', stream=True)) as r:
        # Do things with the response here.

.. _`documented here`: http://docs.python.org/2/library/contextlib.html#contextlib.closing

.. _keep-alive:

Keep-Alive
----------

좋은 소식입니다 — urllib3 감사합니다, session 안에서 keep-alive를 100% 자동으로 지원합니다!
어떤 리퀘스트들이던 당신은 session을 이용하여 자동으로 적절한 연결을 재사용합니다.

연결들은 단지 전부다 읽혀진 바디를 재사용하기위해 다시 풀에 넣어둡니다.
``stream`` 을 ``False`` 로 설정하거나 ``Response`` 오브젝트의 ``content`` 를 읽는것을 확실하게 해두세요.

.. _streaming-uploads:

Streaming Uploads
-----------------

리퀘스트는 큰파일을 전송하거나 메모리에서 파일을 읽지않고 전송하는 스트리밍 업로드를 지원합니다.
스트림과 업로드를 위해, 간단하게 전송할 파일을 body에 추가하기만 하면 됩니다.::

    with open('massive-body', 'rb') as f:
        requests.post('http://some.url/streamed', data=f)

.. warning:: 파일을 열때 `binary mode`_ 로 여는 것을 권장합니다.
             Requests는 ``Content-Length`` 의 값이 파일의 bytes로 설정하여
             당신에게 ``Content-Length`` 헤더를 제공합니다.
             만약 당신이 파일을 *text mode* 로 열었다면 에러를 유발할 것입니다.

.. _binary mode: https://docs.python.org/2/tutorial/inputoutput.html#reading-and-writing-files


.. _chunk-encoding:

Chunk-Encoded Requests
----------------------

Requests는 또한 들어오는 requests에 맞춰 보내는 것을 지원합니다.
chunk-encoded 리퀘스트를 보내기 위해서는, 간단한 생성자(또는 길이에 상관없는 반복자)를 제공합니다.::

    def gen():
        yield 'hi'
        yield 'there'

    requests.post('http://some.url/chunked', data=gen())


chunked encoded 응답들을 위해, :meth:`Response.iter_content() <requests.models.Response.iter_content>` 를 사용하길 권장합니다.
가장 이상적인 상황은 당신이 리퀘스트에 ``stream=True`` 로 설정을 했고,
당신이 연속된 ``iter_content`` 를 chunk 사이즈 파라메터가 ``None`` 이 되기전까지 연속해서 부르는것 입니다.
또한 당신이 chunk의 최대 사이즈를 변경하기 원한다면, 원하는 크기로 chunk size 파라메터를 변경할 수 있습니다.

.. _multipart:

POST Multiple Multipart-Encoded Files
-------------------------------------

당신은 많은 파일을 하나의 request에 보낼 수 있습니다.
예를들어, 당신이  HTML폼에 여러 파일 필드로 image파일을 업로드를 원한다고 생각해보세요::

    <input type="file" name="images" multiple="true" required="true"/>

그러면 단지 파일들을  ``(form_field_name, file_info)`` 와 같이 리스트로 작성하시면 됩니다. ::

    >>> url = 'http://httpbin.org/post'
    >>> multiple_files = [
            ('images', ('foo.png', open('foo.png', 'rb'), 'image/png')),
            ('images', ('bar.png', open('bar.png', 'rb'), 'image/png'))]
    >>> r = requests.post(url, files=multiple_files)
    >>> r.text
    {
      ...
      'files': {'images': 'data:image/png;base64,iVBORw ....'}
      'Content-Type': 'multipart/form-data; boundary=3131623adb2043caaeb5538cc7aa0b3a',
      ...
    }

.. warning:: 파일을 열때 `binary mode`_ 로 여는 것을 권장합니다.
             Requests는 ``Content-Length`` 의 값이 파일의 bytes로 설정하여
             당신에게 ``Content-Length`` 헤더를 제공합니다.
             만약 당신이 파일을 *text mode* 로 열었다면 에러를 유발할 것입니다.

.. _binary mode: https://docs.python.org/2/tutorial/inputoutput.html#reading-and-writing-files


.. _event-hooks:

Event Hooks
-----------

Requests는 당신이 request 프로세스의 일부를 조정하거나 signal 이벤트를 제어하기 위한 hook 시스템을 갖고있습니다.
사용 가능한 hook들:

``response``:
    response는 Request에 의해 생성됩니다.

``{hook_name: callback_function}`` 와 같이 각 리퀘스트마다 ``hooks`` 리퀘스트 파라메터를 이용하여 훅함수를 배정할 수 있습니다.::

    hooks=dict(response=print_url)

이 ``callback_function`` 은 거대한 양의 데이터를 첫 번째 인자로 받을것입니다.::

    def print_url(r, *args, **kwargs):
        print(r.url)

만약 당신의 callback을 실행하는동안 에러를 유발한다면, 경고를 주는것입니다.
만약 callback function이 값을 return 한다면, 데이터가 안으로 들어왔다고 생각할 수 있습니다.
만약 함수가 아무것도 return하지 않았다면 아무일도 일어나지 않습니다.


Let's print some request method arguments at runtime::

    >>> requests.get('http://httpbin.org', hooks=dict(response=print_url))
    http://httpbin.org
    <Response [200]>

.. _custom-auth:

Custom Authentication
---------------------

리퀘스트는 사용자만의 인증 메커니즘을 명시해 사용할 수 있습니다.

``auth`` 인자가 필요한 request 메소드는 이것을 보내기전에 수정하는 기회를 갖을 것입니다.
인증 구현을 위해서 서브클래스로 ``requests.auth.AuthBase`` 를 사용하여 정의하면 됩니다.

리퀘스트는 ``requests.auth`` 안에 있는 ``HTTPBasicAuth`` 와 ``HTTPDigestAuth`` 두가지의 인증 스키마를 구현하면 됩니다
만약 ``X-Pizza`` 헤더에 패스워드 값을 설정하는 웹 서비스를 갖고 있다면 일반적이진 않지만 아래와 같이 해야할것입니다.::

    from requests.auth import AuthBase

    class PizzaAuth(AuthBase):
        """Attaches HTTP Pizza Authentication to the given Request object."""
        def __init__(self, username):
            # setup any auth-related data here
            self.username = username

        def __call__(self, r):
            # modify and return the request
            r.headers['X-Pizza'] = self.username
            return r


그뒤, 우리는 Pizza Auth를 이용해 request를 만들면 됩니다.::

    >>> requests.get('http://pizzabin.org/admin', auth=PizzaAuth('kenneth'))
    <Response [200]>

.. _streaming-requests:

Streaming Requests
------------------

:class:`requests.Response.iter_lines()` 를 이용하여 쉽게 많은 양이 반복되는 `Twitter Streaming
API <https://dev.twitter.com/streaming/overview>`_ 와 같은 streaming API들을 처리할 수 있습니다.
간단하게 ``stream``을 ``True``로 설정하고 반복되는 많은 응답들을 :class:`~requests.Response.iter_lines()` 로 처리하면 됩니다::

    import json
    import requests

    r = requests.get('http://httpbin.org/stream/20', stream=True)

    for line in r.iter_lines():

        # filter out keep-alive new lines
        if line:
            print(json.loads(line))

.. warning::

    :class:`~requests.Response.iter_lines()` 는 reentrant로부터 안전하지 않습니다.
    이 함수를 여러번 호출하는것은 받은 데이터의 일부를 잃어버릴 수 있습니다.
    이러한 경우 연속된 오브젝트를 사용하는 대신 당신이 필요한 곳에서 호출하세요::

        lines = r.iter_lines()
        # Save the first line for later or just skip it

        first_line = next(lines)

        for line in lines:
            print(line)

.. _proxies:

Proxies
-------

만약 당신이 프록시를 사용하기 원한다면, 당신은 개별적으로 ``proxies`` 인자를 request 메소드를 통해 requests의 환경을 설정해야합니다::

    import requests

    proxies = {
      'http': 'http://10.10.1.10:3128',
      'https': 'http://10.10.1.10:1080',
    }

    requests.get('http://example.org', proxies=proxies)


당신은 또한 환경에 맞게 ``HTTP_PROXY`` 와 ``HTTPS_PROXY`` 로 프록시를 설정할수있습니다.::

    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"

    $ python
    >>> import requests
    >>> requests.get('http://example.org')

`http://user:password@host/` 문법을 이용하여 설정한 프록시를 통하여 HTTP Basic 인증을 사용합니다::

    proxies = {'http': 'http://user:pass@10.10.1.10:3128/'}


프록시를 줄수있습니다 구체적으로 scheme 과 host를 `scheme://hostname` 를 이용하여 프록시를 만들수 있습니다?.
이것은 어떤 리퀘스트라도 scheme과 정확한 hostname을 받아 비교할 것입니다::

    proxies = {'http://10.20.1.128': 'http://10.10.1.10:5323'}


위의 프록시 URL들은 반드시 scheme을 포함해야합니다.

SOCKS
^^^^^

.. versionadded:: 2.10.0

기본 HTTP proxy들 뿐만아니라, 리퀘스트는 또한 SOCKS 프로토콜을 이용한 프록시를 지원합니다.
이것은 third-party 라이브러리를 설치해 사용해야하는 선택적인 옵션입니다.
``pip`` 을 이용해서 다음과 같이 설치할 수 있습니다.

.. code-block:: bash

    $ pip install requests[socks]

해당 라이브러리를 설치했다면, SOCKS 프록시를 HTTP를 이용하는 것과 같이 쉽게 이용할 수 있습니다. ::

    proxies = {
        'http': 'socks5://user:pass@host:port',
        'https': 'socks5://user:pass@host:port'
    }

.. _compliance:

Compliance
----------

Requests는 적절한 설계되어 설계를 따르고 있습니다.
그리고 유저에게 곤란을 만들지 않는 RFC를 준수합니다.
이렇게 명시된 사양을 따르는것은 그렇지 않은 사람들에게도 의미있는 일입니다.

Encodings
^^^^^^^^^

응답을 받았을때, Requests는 :attr:`Response.text <requests.Response.text>` 을 이용할때
decoding을 하기 위해 encoding을 추측해 만듭니다.
Requests는 처음으로 HTTP header안에 encoding을 확인합니다. 그리고 비어있다면
`chardet <http://pypi.python.org/pypi/chardet>`_ 을 이용하여 encoding을 추측할 것입니다.

만약 명쾌한 charset이 HTTP 헤더안에 있거나 ``Content-Type`` 헤더에 ``text`` 가 포함되어있다면
Requests는 그것에 따를것입니다.
그렇지 않은 경우 `RFC 2616 <http://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.7.1>`_ 를 따라 기본 charset을
``ISO-8859-1`` 로 설정합니다.
만약 요구하는 encoding이 다르다면, :attr:`Response.encoding <requests.Response.encoding>` 의 값을 수동으로 설정하거나,
:attr:`Response.content <requests.Response.content>` 를 이용하여 raw 응답을 이용할 수 있습니다.

.. _http-verbs:

HTTP Verbs
----------

Requests는 거의 모든 HTTP 구문(GET, OPTIONS,HEAD, POST, PUT, PATCH 그리고 DELETE)을 지원합니다.
다음은 다양한 구문을 Requests로 GitHub API를 이용하는 예제를 제공합니다.

우리는 우선 가장 흔하게 사용되는 구문인 GET을 시작합니다. HTTP의 GET은 주어진 URL에서 resource를 반환하는 메소드입니다.
그결과, 이 구문은 당신은 웹에서 데이터를 검색할때 사용 해야합니다.
사용 예로 우리는 Github에서 commit에 대한 정보를 가져오기 위해 사용할 것입니다.
우리가 requests의  ``a050faf`` 커밋을 가져와 봅시다.::

    >>> import requests
    >>> r = requests.get('https://api.github.com/repos/kennethreitz/requests/git/commits/a050faf084662f3a352dd1a941f2c7c9f886d4ad')

우리는 GitHub의 바른 응답을 확인할 수 있습니다.
만약 우리가 받은 컨텐츠의 종류가 무엇인지 알고 싶다면, 다음과 같이 할 수 있습니다. ::

    >>> if r.status_code == requests.codes.ok:
    ...     print(r.headers['content-type'])
    ...
    application/json; charset=utf-8


따라서, GitHub는 JSON을 보내주었습니다.
좋습니다. 우리는 :meth:`r.json <requests.Response.json>` 메소드를 이용하여 Python 오브젝트로 변환 하여 사용할 수 있습니다. ::

    >>> commit_data = r.json()

    >>> print(commit_data.keys())
    [u'committer', u'author', u'url', u'tree', u'sha', u'parents', u'message']

    >>> print(commit_data[u'committer'])
    {u'date': u'2012-05-10T11:10:50-07:00', u'email': u'me@kennethreitz.com', u'name': u'Kenneth Reitz'}

    >>> print(commit_data[u'message'])
    makin' history


매우 간단하죠? 자 그럼 GitHub의 API를 좀 더 사용해 봅시다.
이제 우리는 문서를 찾을것입니다.
그러나 우리는 만약 requests를 사용하지 않는대신 재미있는 방법을 통하여 찾아 볼 것입니다.
우리는 Requests의 OPTIONS 구문을 활용할수있습니다.
HTTP 메소드의 일종인 OPTIONS는 url을 지원하기 때문에 우리는 이용만 하면됩니다. ::

    >>> verbs = requests.options(r.url)
    >>> verbs.status_code
    500

음 뭐죠? 별 도움이 되지 않네요! GitHub는 많은 API를 제공하는것처럼 보이지만 사실상 OPTIONS 함수는 실행되지 않네요.
이것을 간과했네요. 그러나 괜찮아요, 우리는 이 지루한 문서를 이어가도록하죠.
만약 GitHub가 OPTIONS가 재대로 실행됐다면, 그들은 우리가 사용할 수 있는 메소드들을 헤더에 포함해서 보내줬을 것입니다.
예를들면 다음과 같이말이죠. ::

    >>> verbs = requests.options('http://a-good-website.com/api/cats')
    >>> print(verbs.headers['allow'])
    GET,HEAD,POST,OPTIONS

문서로 돌아와서,
우리는 커밋을 위한 메소드가 오로지 POST메소드만 있다는걸 확인 할수 있었습니다.
따라서 우리는 Requests의 repo를 사용하기위해, 우리는 서툴지만 POSTS 만들어 쓰는것을 피해야합니다.
대신, GitHub의 상태 Issue들을 확인해봅시다.
이제 Issue #482를 받아 볼 것입니다.
우리의 예제로 사용하기 위해 해당 이슈는 이미 존재하고 있습니다. 같이 해봅시다. ::

    >>> r = requests.get('https://api.github.com/repos/kennethreitz/requests/issues/482')
    >>> r.status_code
    200

    >>> issue = json.loads(r.text)

    >>> print(issue[u'title'])
    Feature any http verb in docs

    >>> print(issue[u'comments'])
    3

좋아요, 이슈에 3개의 코멘트가 달려있네요. 그중에 마지막 코멘트를 확인해 봅시다. ::

    >>> r = requests.get(r.url + u'/comments')
    >>> r.status_code
    200

    >>> comments = r.json()

    >>> print(comments[0].keys())
    [u'body', u'url', u'created_at', u'updated_at', u'user', u'id']

    >>> print(comments[2][u'body'])
    Probably in the "advanced" section


네, 아무것도 아닌 내용같네요.
글올린 사람에 대해서 이야기해보죠.
누가 올렸을까요?
::

    >>> print(comments[2][u'user'][u'login'])
    kennethreitz


네, Kenneth가 우리의 생각은 이 예제가 quickstart 가이드 대신 이곳에 적혀있어야 한다고 말하네요.
GitHub API 문서에 따르면, 스레드에 comment를 작성하는 것은 POST를 이용하는 방법이 있다고 하네요. 해봅시다. ::

    >>> body = json.dumps({u"body": u"Sounds great! I'll get right on it!"})
    >>> url = u"https://api.github.com/repos/kennethreitz/requests/issues/482/comments"

    >>> r = requests.post(url=url, data=body)
    >>> r.status_code
    404

허, 이거 이상하네요.
우리는 증명이 필요합니다.
이것때문에 우리가 이용 할수 없겠죠?
틀렸습니다. Requests는 가장 흔한 Basic Auth 를 포함해 많은 인증폼들을 이용해 사용할 수 있습니다. ::

    >>> from requests.auth import HTTPBasicAuth
    >>> auth = HTTPBasicAuth('fake@example.com', 'not_a_real_password')

    >>> r = requests.post(url=url, data=body, auth=auth)
    >>> r.status_code
    201

    >>> content = r.json()
    >>> print(content[u'body'])
    Sounds great! I'll get right on it.

대단합니다. 오 잠시만요!
음 잠시 시간을 줄수있나요? 고양이에게 밥을 줘야하기때문이죠.
coomment를 수정할수 있었으면 좋겠네요, GitHub가 우리에게 comment 수정을 위한 다른 HTTP 구문인 PATCH를 허락한다면 말이죠.
그럼 해봅시다. ::

    >>> print(content[u"id"])
    5804413

    >>> body = json.dumps({u"body": u"Sounds great! I'll get right on it once I feed my cat."})
    >>> url = u"https://api.github.com/repos/kennethreitz/requests/issues/comments/5804413"

    >>> r = requests.patch(url=url, data=body, auth=auth)
    >>> r.status_code
    200


좋아요!
이제 Kenneth를 괴롭히기위해 comment를 바꿀거에요 그리고 그에게 내가 이것을 했다는걸 말하지 않을 거에요.
그말은 이 comment를 삭제하고 싶단거죠.
GitHub 우리가 comment를 삭제하기위해 적절한 이름의 DELETE 메소드를 사용할수 있습니다.
따라오세요. ::

    >>> r = requests.delete(url=url, auth=auth)
    >>> r.status_code
    204
    >>> r.headers['status']
    '204 No Content'

좋아요. 다됐습니다
마지막으로 내가 사용한 ratelimit을 알기 원해요.
찾아봅시다.
GitHub는  헤더에 약간의 정보를 포함해 보내줍니다.
모든 다운로드를 받지 않았다면 HEAD 리퀘스트를 보내 헤더를 확인할 수 있습니다. ::

    >>> r = requests.head(url=url, auth=auth)
    >>> print(r.headers)
    ...
    'x-ratelimit-remaining': '4995'
    'x-ratelimit-limit': '5000'
    ...


좋아요.
파이썬으로 만들어진 프로그램을 이용해 GitHub API를 4995번이나 더 사용할수 있네요,

.. _link-headers:

Link Headers
------------

많은 HTTP API들이 Link 헤더를 갖고 있습니다. API들을 만들때 언급하거나 언급되고 있습니다?

GitHub 는 이것을 `pagination <http://developer.github.com/v3/#pagination>`_ 에 사용합니다.
그들의 API입니다 확인해 봅시다. ::

    >>> url = 'https://api.github.com/users/kennethreitz/repos?page=1&per_page=10'
    >>> r = requests.head(url=url)
    >>> r.headers['link']
    '<https://api.github.com/users/kennethreitz/repos?page=2&per_page=10>; rel="next", <https://api.github.com/users/kennethreitz/repos?page=6&per_page=10>; rel="last"'


Requests 는 자동으로 이 link 헤더들을 분석하고 사용하기 쉽게 만들어줍니다. ::

    >>> r.links["next"]
    {'url': 'https://api.github.com/users/kennethreitz/repos?page=2&per_page=10', 'rel': 'next'}

    >>> r.links["last"]
    {'url': 'https://api.github.com/users/kennethreitz/repos?page=7&per_page=10', 'rel': 'last'}

.. _transport-adapters:

Transport Adapters
------------------

v1.0.0 이후로 Requests는 내부 디자인이 모듈러로 바뀌었습니다.
이러한 이유로 이것은 되었습니다 전송 어댑터를 구현했습니다.  `described here`_ 에 자세히 적혀있습니다.
전송 어뎁터는 HTTP 서비스를 위한 상호작용 메소드를 정의하는 메카니즘으로 제공됩니다.
특히, 전송 어댑터는 당신이 서비스별로 환경을 적용할수 있게 해줍니다.

Requests는 단일 전손 어댑터 :class:`HTTPAdapter <requests.adapters.HTTPAdapter>` 를 포함했습니다.
이 어댑터는 기본적으로 Requests의 HTTP와 HTTPS가 강력한 `urllib3`_ 라이브러리를 이용할 수 있게 상호작용을 제공합니다.
Requests의 :class:`Session <requests.Session>` 을 초기화할때,
HTTP와 HTTPS를 위해 각각 :class:`Session <requests.Session>` 오브젝트 하나를 부여합니다.

Requests는 유저가 만든 전송 어뎁터를 특정한 기능에 사용할 수 있습니다.
한번 만들어진 전송 어댑터는 Session 오브젝트에 탑재하여 ,
마찬가지로 웹 서비스에 적용할 수 있음을 보여줍니다.::

    >>> s = requests.Session()
    >>> s.mount('http://www.github.com', MyAdapter())

이전에 탑재된 전송 어댑터 인스턴스는 불러올 수 있습니다.
한번 탑재되면, 어떤 HTTP 리퀘스트라도 session을 어떤 URL로 시작하던 사전에 등록한 전송 어댑터를 사용할수있습니다.
전송 어댑터의 실행에 관한 자세한 사항은 이문서에 포함되어 있지 않습니다.
그러나 다음 예제에서 간단하게 SSL을 사용하는 방법을 볼 수 있습니다.
다음 ``requests.adapters.BaseAdapter`` 를 확인해 보시길 바랍니다.

Example: Specific SSL Version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Requests 팀은 어떤 SSL버전이던 기본적으로 `urllib3`_ 를 이용해 선택합니다.
보통은 이것은 괜찮습니다. 그러나 실시간으로 당신이 기본값으로 호환되지 않는 서비스에 연결이 필요할 수 있습니다.

당신은 현재 실행중인 HTTPAdapter를 가져오기오기 위해 전송어댑터를 이용할수있습니다.
그리고 *ssl_version*을 통과하게 할 수 있습니다.
우리는 SSLv3를 이용해 사용할수 있게 만들 수 있습니다. ::

    import ssl

    from requests.adapters import HTTPAdapter
    from requests.packages.urllib3.poolmanager import PoolManager


    class Ssl3HttpAdapter(HTTPAdapter):
        """"Transport adapter" that allows us to use SSLv3."""

        def init_poolmanager(self, connections, maxsize, block=False):
            self.poolmanager = PoolManager(
                num_pools=connections, maxsize=maxsize,
                block=block, ssl_version=ssl.PROTOCOL_SSLv3)

.. _`described here`: http://www.kennethreitz.org/essays/the-future-of-python-http
.. _`urllib3`: https://github.com/shazow/urllib3

.. _blocking-or-nonblocking:

Blocking Or Non-Blocking?
-------------------------

Requests는 기본적인 전송 어댑터를 이용해 어떠한 non-blocking IO도 지원하지 않습니다.
:attr:`Response.content <requests.Response.content>` 아마 다운로드가 다될때 까지 막혀있습니다.
만약 non-blocking 을 원한다면,스트림 형태의 라이브러리(:ref:`streaming-requests`)가 많은 시간을 절약해 줄수 있을 것입니다.
그러나, 이 호출은 여전히 스트림형태가아닙니다.

만약 blocking IO를 사용하는것이 걱정된다면,
여기 많은 Python의 asynchronicity 프레임워크와 Requests를 이용한 프로젝트가 있습니다.
그중 두가지로 `grequests`_ 와 `requests-futures`_ 가 있습니다.

.. _`grequests`: https://github.com/kennethreitz/grequests
.. _`requests-futures`: https://github.com/ross/requests-futures
.. _timeouts:

Timeouts
--------

대부분 외부서버는 requests에 대해 timeout을 갖고 있습니다. 이러한경우 서버는 적절한 방식으로 응답을 하지 않습니다.
timeout이 없다면, 당신의 코드가 몇분동안 연결되어 있을 수 있습니다.
**연결** 타임아웃은 Requests가 클라이언트가 원격머신의 소켓에 얼마나 연결되어 기다릴지 나타내는 숫자입니다.
`TCP packet retransmission window <http://www.hjp.at/doc/rfc/rfc2988.txt>`_ 에 따르면
타임아웃은 3배보다 약간 크게 설정하는것은 좋은 설정입니다.

한번 클라이언트가 서버에 연결되면 HTTP 리퀘스트를 보냅니다.
그러면 클라이언트가 서버가 응답을 보낼때까지 기다리는 시간인 timeout에 설정된 숫자를 확인합니다.
(분명히, 이 시간은 클라이언트가 서버로부터 보내질 byte들을 기다릴 시간이다.
99.9%로, 이것은 서버가 첫바이트를 보내는 것보다 짧을 것입니다.)

다음과 같이 timeout을 설정한다면::

    r = requests.get('https://github.com', timeout=5)

타임아웃 값은 ``connect`` 와 ``read`` 타임아웃에 둘다 적용될것입니다.
만약 두 값을 분리하고싶다면 튜플을 사용해 사용할 수 있습니다. ::

    r = requests.get('https://github.com', timeout=(3.05, 27))

만약 원격 서버가 매우 느리다면
타임아웃 값을 None으로 설정하고 커피한잔의 여유를 즐기며 Requests에게 응답이 올때까지 기다리라고 할수있습니다.

.. code-block:: python

    r = requests.get('https://github.com', timeout=None)

.. _`connect()`: http://linux.die.net/man/2/connect
