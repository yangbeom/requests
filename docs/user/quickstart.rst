.. _quickstart:

Quickstart
==========

.. module:: requests.models

Requests를 시작하기 원하나요?

이 페이지는 Requests를 사용하는 방법에 대해 적혀있습니다.

먼저, 설치와 업데이트를 진행하세요 :

* Requests is :ref:`installed <install>`
* Requests is :ref:`up-to-date <updates>`

간단한 예제들과 함께 시작해 봅시다.

Make a Request
--------------

Requests를 이용하여 리퀘스트를 만드는것은 매우 간단합니다.

우선 Requests 모듈을 import 해봅시다. ::

    >>> import requests

이제 웹페이지를 접속해봅시다. GitHub의 타임라인에 접속하는 예제입니다. ::

    >>> r = requests.get('https://api.github.com/events')


이제 :class:`Response <requests.Response>` 오브젝트인 r 을 부를 수 있습니다.
이 오브젝트에서 필요한 모든 정보를 가져올 수 있습니다.

Requests의 간단한 API는 각각의 HTTP 리퀘스트 폼을 의미합니다.

예를 들어 HTTP의 POST 리퀘스트를 만드는 방법입니다. ::

    >>> r = requests.post('http://httpbin.org/post', data = {'key':'value'})

쉽죠? 다른 HTTP 리퀘스트인 PUT, DELETE,HEAD 그리고 OPTIONS는 어떨까요?

예제를 보시죠. ::

    >>> r = requests.put('http://httpbin.org/put', data = {'key':'value'})
    >>> r = requests.delete('http://httpbin.org/delete')
    >>> r = requests.head('http://httpbin.org/get')
    >>> r = requests.options('http://httpbin.org/get')

모두 좋습니다. 그러나 이것은 Requests로 할 수 있는 것중 일부입니다.

Passing Parameters In URLs
--------------------------

종종 URL 쿼리를 통해 데이터를 보내길 원합니다.
만약 구성된 URL을 다룰줄 안다면, URL 뒤 물음표에 키와 값을 넣어 전송하는 방법을 알것입니다. e.g. ``httpbin.org/get?key=val``.

Requests는 당신이 인자값을 ``params`` 키워드를 이용하여 dictionary로 보내는 것이 가능합니다.
예를 들어 당신이 ``key1=value1`` 와 ``key2=value2`` 를 ``httpbin.org/get`` 에 보내고 싶다면 아래 코드와 같이 작성하면 됩니다. ::

    >>> payload = {'key1': 'value1', 'key2': 'value2'}
    >>> r = requests.get('http://httpbin.org/get', params=payload)

URL을 출력했을때 올바르게 적용되었음을 확인할 수 있습니다. ::

    >>> print(r.url)
    http://httpbin.org/get?key2=value2&key1=value1

키 값이 ``None`` 이면 URL 쿼리문자에 포함되지 않습니다.

당신은 또한 list를 값으로 갖을 수도 있습니다. ::

    >>> payload = {'key1': 'value1', 'key2': ['value2', 'value3']}

    >>> r = requests.get('http://httpbin.org/get', params=payload)
    >>> print(r.url)
    http://httpbin.org/get?key1=value1&key2=value2&key2=value3

Response Content
----------------

서버가 보내온 응답의 컨텐츠를 읽을 수 있습니다.
여기 다시한번 GitHub timeline을 봅시다. ::

    >>> import requests

    >>> r = requests.get('https://api.github.com/events')
    >>> r.text
    u'[{"repository":{"open_issues":0,"url":"https://github.com/...

Requests는 서버에서 온 컨텐츠를 자동으로 decode 합니다.
대부분의 유니코드 charset들은 자연스럽게 decode 될것입니다.

리퀘스트를 만들었을때, Requests는 응답의 HTTP 헤더를 기반으로 encode을 알아냅니다
``r.text`` 를 사용할 때 Requests를 통해 알아낸 인코딩을 합니다.
당신은 ``r.encoding`` 을 통해 Requests에서 무엇으로 encoding 했는지 찾을수 있고 바꿀수도 있습니다. ::

    >>> r.encoding
    'utf-8'
    >>> r.encoding = 'ISO-8859-1'

만약 encoding을 바꾸고 싶으면, Requests는 ``r.encoding`` 에 새로운 값을 넣어 바꿀수 있습니다.
당신이 컨탠츠에 새로운 인코딩 로직을 적용하는것을 원할 수 있습니다.
예를들어, HTTP나 XML은 body안에 명시할 수 있습니다.
이와 같은경우 당신이 ``r.content`` 를 이용해 encoding을 찾고 ``r.encoding`` 을 설정할 수 있습니다.
당신이 ``r.text`` 를 바른 encoding을 사용 할 수 있게 해줍니다.

Requests는 필요하다면 커스텀 encoding을 이용할 수 있습니다.
``codec`` 모듈로 자신만의 encoding을 만들고 등록한다면,
당신은 쉽게 ``r.encoding`` 값에 codec 이름으로 설정할수 있습니다. 그리고 Requests는 설정된 것을 이용해 decoding 할 것 입니다.

Binary Response Content
-----------------------

non-text 응답을 받을때 바디의 byte에 접근이 가능합니다. ::

    >>> r.content
    b'[{"repository":{"open_issues":0,"url":"https://github.com/...

``gzip`` 이나 ``deflate`` 와같은 전송인코딩을 자동적으로 디코드해줍니다.

다음은 응답에서 받은 바이너리 데이터에서 이미지를 만드는 예제입니다. ::

    >>> from PIL import Image
    >>> from StringIO import StringIO

    >>> i = Image.open(StringIO(r.content))


JSON Response Content
---------------------

JSON 디코더도 포함되어 있어 JSON 데이터를 처리 할 수 있습니다. ::

    >>> import requests

    >>> r = requests.get('https://api.github.com/events')
    >>> r.json()
    [{u'repository': {u'open_issues': 0, u'url': 'https://github.com/...

JSON 디코딩이 실패했을때 ``r.json`` 은 예외를 발생합니다.
예를 들어, 만약 응답에서 204(컨텐츠 없음)을 받는다던지, 유효하지 않는 JSON을 받는다면
``r.json`` 은 ``ValueError: No JSON object could be decoded`` 를 발생합니다.

특히 ``r.json`` 은 응답의 성공을 알려주지 않으니 주의하세요.
몇 서버들은 failed response(에러코드 HTTP 500)와 함께JSON object를 전송합니다.
어떤 JSON들은 decod되어 반환되기도 합니다.
리퀘스트의 성공을 확인하기위해, ``r.raise_for_status()`` 나 ``r.status_code`` 를 체크하세요.

Raw Response Content
--------------------

드문 경우로 서버에서 raw socket 응답을 받을수 있습니다. 이때 ``r.raw`` 로 접근할 수 있습니다.

raw socket 응답을 받길 원한다면 ,리퀘스트를 보낼때 ``stream=True`` 를 설정하세요. 한번 해보면 쉽게 할수 있습니다. ::

    >>> r = requests.get('https://api.github.com/events', stream=True)

    >>> r.raw
    <requests.packages.urllib3.response.HTTPResponse object at 0x101194810>

    >>> r.raw.read(10)
    '\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x03'

일반적으로, 스트림파일을 다음과 같은 방법으로 저장할 수 있습니다. ::

    with open(filename, 'wb') as fd:
        for chunk in r.iter_content(chunk_size):
            fd.write(chunk)

대부분을 ``Response.iter_content`` 를 이용하여 관리할 수 있습니다 그렇지만 ``Response.raw`` 를 직접 이용하여 관리해도 됩니다.
스트리밍 다운로드를 할때 , 컨텐츠를 가져오는 방법은 ``Response.iter_content`` 를 이용하는것을 권장합니다.

Custom Headers
--------------

만약 리퀘스트에 HTTP 헤더를 추가하고싶다면, 간단하게 ``dict`` 로 작성하여 ``headers`` 파라메터를 이용하시면 됩니다.

예를 들어, 우리는 이전 예제에서 우리의 user-agent를 명시하기 싫다면 다음과 같이 변경할수 있습니다. ::

    >>> url = 'https://api.github.com/some/endpoint'
    >>> headers = {'user-agent': 'my-app/0.0.1'}

    >>> r = requests.get(url, headers=headers)


Note: 커스텀 헤더들은 더 많은 정보를 설정하는것보다 우선순위가 떨어집니다??. 예를 들면 :

* Authorization headers set with `headers=` will be overridden if credentials
  are specified in ``.netrc`` , which in turn will be overridden by the ``auth=``
  parameter.
* Authorization headers will be removed if you get redirected off-host.
* Proxy-Authorization headers will be overridden by proxy credentials provided in the URL.
* Content-Length headers will be overridden when we can determine the length of the content.

게다가, Requests는 모든 기반 custom 헤더로 바꾸지 않습니다.
헤더는 간편하게 마지막 리퀘스트에 넣을 수 있습니다.

More complicated POST requests
------------------------------

일반적으로, 당신은 HTML폼과 같은 폼에 encoded 데이터를 보내고 싶어 할껍니다.
이것을 위해, dictionary로 ``data`` 인자에 넣을 수 있습니다.
리퀘스트를 만들때 데이터를 적은 dictionary 들은 자동적으로 폼에 맞게 수정될 것입니다. ::

    >>> payload = {'key1': 'value1', 'key2': 'value2'}

    >>> r = requests.post("http://httpbin.org/post", data=payload)
    >>> print(r.text)
    {
      ...
      "form": {
        "key2": "value2",
        "key1": "value1"
      },
      ...
    }

form-encoded 가아닌 데이터를 보내고 싶을 수 있습니다.
만약 ``dict`` 대신 ``string`` 을 넣고싶다면, 다음과 같이 넣을 수 있습니다.

예를들어, GitHub API v3는 POST/PATCH로 JSON 형식을 이용해 데이터를 전송하는것을 허락합니다. ::

    >>> import json

    >>> url = 'https://api.github.com/some/endpoint'
    >>> payload = {'some': 'data'}

    >>> r = requests.post(url, data=json.dumps(payload))

``dict`` 로 인코딩하는 대신 2.4.2버전 이상에서는 바로 ``json`` 파라메터를 이용하여 직접 넣을 수있습니다. ::

    >>> url = 'https://api.github.com/some/endpoint'
    >>> payload = {'some': 'data'}

    >>> r = requests.post(url, json=payload)

POST a Multipart-Encoded File
-----------------------------

Requests는 쉽게 Multipart-encoded 파일을 올릴 수 있습니다. ::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': open('report.xls', 'rb')}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "<censored...binary...data>"
      },
      ...
    }

파일 이름과 컨텐츠 타입을 설정하고 헤더에 추가하면 됩니다. ::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': ('report.xls', open('report.xls', 'rb'), 'application/vnd.ms-excel', {'Expires': '0'})}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "<censored...binary...data>"
      },
      ...
    }

만약 파일과 설명하는 문자열을 보내고싶다면 ::

    >>> url = 'http://httpbin.org/post'
    >>> files = {'file': ('report.csv', 'some,data,to,send\nanother,row,to,send\n')}

    >>> r = requests.post(url, files=files)
    >>> r.text
    {
      ...
      "files": {
        "file": "some,data,to,send\\nanother,row,to,send\\n"
      },
      ...
    }

stream을 이용하여 ``multipart/form-data`` 리퀘스트로 큰 파일을 보낼 수 있습니다.
기본적으로, ``requests`` 는 이것을 지원하지 않습니다. 그러나 ``requests-toolbelt`` 에서 지원하고 있습니다.
`the toolbelt's documentation <https://toolbelt.readthedocs.io>`_ 에서 사용법을 읽고 사용해 보세요.
하나의 리퀘스트에서 여러파일을 보내는 것은 :ref:`advanced <advanced>` 에서 확인 할 수있습니다.

.. warning:: It is strongly recommended that you open files in `binary mode`_.
             This is because Requests may attempt to provide the
             ``Content-Length`` header for you, and if it does this value will
             be set to the number of *bytes* in the file. Errors may occur if
             you open the file in *text mode*.

.. _binary mode: https://docs.python.org/2/tutorial/inputoutput.html#reading-and-writing-files


Response Status Codes
---------------------

응답 상태 코드를 다음과 같이 확인 할 수 있습니다. ::

    >>> r = requests.get('http://httpbin.org/get')
    >>> r.status_code
    200

또한 Requests는 상태코드를 포함하고 있어 다음과 같이 쉽게 비교할 수 있습니다. ::

    >>> r.status_code == requests.codes.ok
    True

만약 4XX 클라이언트 에러나 5XX 서버 에러와 같은 응답을 받는다면,
:meth:`Response.raise_for_status() <requests.Response.raise_for_status>` 가 일어날 것입니다. ::


    >>> bad_r = requests.get('http://httpbin.org/status/404')
    >>> bad_r.status_code
    404

    >>> bad_r.raise_for_status()
    Traceback (most recent call last):
      File "requests/models.py", line 832, in raise_for_status
        raise http_error
    requests.exceptions.HTTPError: 404 Client Error

그러나 ``raise_for_status()`` 를 사용하여 ``r`` 의 ``status_code`` 가 ``200`` 임을 알수 있습니다. ::

    >>> r.raise_for_status()
    None

Response Headers
----------------

우리는 서버의 응답 해더를 Python dictionary로 볼수 있습니다. ::

    >>> r.headers
    {
        'content-encoding': 'gzip',
        'transfer-encoding': 'chunked',
        'connection': 'close',
        'server': 'nginx/1.0.4',
        'x-runtime': '148ms',
        'etag': '"e1ca502697e5c9317743dc078f67693f"',
        'content-type': 'application/json'
    }

HTTP 헤더를 만들때 dictionary는 특별합니다.
`RFC 7230 <http://tools.ietf.org/html/rfc7230#section-3.2>`_,에 따르면 HTTP Header 이름은 대소문자를 구분하지 않습니다.
따라서 우리는 어떠한 문자를 사용해서도 원하는 헤더에 접근할 수 있습니다. ::

    >>> r.headers['Content-Type']
    'application/json'

    >>> r.headers.get('content-type')
    'application/json'

이것은 서버가 같은 헤더를 어러번 다른값을 전송하는 특별한 경우가 있습니다,
`RFC 7230 <http://tools.ietf.org/html/rfc7230#section-3.2>`_에 따라서 Requests는 single mapping을 이용하여 dictionary로 나타냅니다. :

    A recipient MAY combine multiple header fields with the same field name
    into one "field-name: field-value" pair, without changing the semantics
    of the message, by appending each subsequent field value to the combined
    field value in order, separated by a comma.

Cookies
-------

만약 Cookie를 포함한 응답을 받았다면 다음과 같이 접근할 수 있습니다. ::

    >>> url = 'http://example.com/some/cookie/setting/url'
    >>> r = requests.get(url)

    >>> r.cookies['example_cookie_name']
    'example_cookie_value'


``cookies`` 파라메터를 이용하여 자신만의 쿠키를 서버에 전송할수 있습니다. ::

    >>> url = 'http://httpbin.org/cookies'
    >>> cookies = dict(cookies_are='working')

    >>> r = requests.get(url, cookies=cookies)
    >>> r.text
    '{"cookies": {"cookies_are": "working"}}'


Redirection and History
-----------------------

기본적으로 리퀘스트는 HEAD 구문을 제외한 모든 구문은 redirect를 시행합니다.
우리는 ``history`` 를 이용하여 redirect를 추적할수 있습니다.

:meth:`Response.history <requests.Response.history>` 리스트 는 완벽한 리퀘스트에 의해 생성된
:class:`Response <requests.Response>` 오브젝트를 포함합니다.
이 리스트는 오래된것부터 최신순으로 정렬되어 있습니다.

예를 들어, GitHub 의 모든 HTTP 리퀘스트는 HTTPS로 리다이렉트됩니다. ::

    >>> r = requests.get('http://github.com')

    >>> r.url
    'https://github.com/'

    >>> r.status_code
    200

    >>> r.history
    [<Response [301]>]


만약 GET, OPTIONS, POST,PUT, PATCH 혹은 DELETE를 사용한다면, 당신은 ``allow_redirects`` 파라메터를 이용하여
자동으로 리다이렉트되는것을 막을 수 있습니다. ::

    >>> r = requests.get('http://github.com', allow_redirects=False)

    >>> r.status_code
    301

    >>> r.history
    []


만약 HEAD를 사용한다면 당신은 reddirection을 활성화 시킬수 있습니다. ::

    >>> r = requests.head('http://github.com', allow_redirects=True)

    >>> r.url
    'https://github.com/'

    >>> r.history
    [<Response [301]>]


Timeouts
--------

Requests에게 응답을 주어진 ``timeout`` 파라메터 시간만큼만 기다리고 멈추라고 말할수 있습니다. ::

    >>> requests.get('http://github.com', timeout=0.001)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    requests.exceptions.Timeout: HTTPConnectionPool(host='github.com', port=80): Request timed out. (timeout=0.001)


.. admonition:: Note

    ``timeout`` is not a time limit on the entire response download;
    rather, an exception is raised if the server has not issued a
    response for ``timeout`` seconds (more precisely, if no bytes have been
    received on the underlying socket for ``timeout`` seconds).


Errors and Exceptions
---------------------

네트워크 문제(DNS failure, refused connection, etc)가 발생한다면, Requests 는 :class:`~requests.exceptions.ConnectionError` 를 발생됩니다.


유효하지 않은 HTTP 응답을 받는다면, Requests는 :class:`~requests.exceptions.HTTPError` 를 발생됩니다.

리퀘스트가 타임아웃된다면, :class:`~requests.exceptions.Timeout` 이 발생됩니다.

만약 리퀘스트가 설정한 리다이렉트의 최대한도 넘었다면, :class:`~requests.exceptions.TooManyRedirects`이 발생됩니다.

모든 Requests의 예외들은 :class:`requests.exceptions.RequestException`에서 상속받습니다.


-----------------------
더 많은 정보를 원하시면 :ref:`advanced <advanced>` 를 확인해 보세요.