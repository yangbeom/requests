.. _api:

Developer Interface
===================
.. module:: requests

이곳은 Requests의 모든 인터페이스를 나타내고있습니다.
Requests가 신뢰하는 주요 외부 라이브러리들을 위해 공식문서의 링크를 제공합니다.

Main Interface
--------------

모든 Requests 함수들은 다음 7가지 methods에 접근할 수 있습니다.
:class:`Response <Response>` object 를 return 해줍니다.

.. autofunction:: request

.. autofunction:: head
.. autofunction:: get
.. autofunction:: post
.. autofunction:: put
.. autofunction:: patch
.. autofunction:: delete

Exceptions
----------

.. autoexception:: requests.RequestException
.. autoexception:: requests.ConnectionError
.. autoexception:: requests.HTTPError
.. autoexception:: requests.URLRequired
.. autoexception:: requests.TooManyRedirects
.. autoexception:: requests.ConnectTimeout
.. autoexception:: requests.ReadTimeout
.. autoexception:: requests.Timeout


Request Sessions
----------------

.. _sessionapi:

.. autoclass:: Session
   :inherited-members:


Lower-Level Classes
-------------------

.. autoclass:: requests.Request
   :inherited-members:

.. autoclass:: Response
   :inherited-members:


Lower-Lower-Level Classes
-------------------------

.. autoclass:: requests.PreparedRequest
   :inherited-members:

.. autoclass:: requests.adapters.HTTPAdapter
   :inherited-members:

Authentication
--------------

.. autoclass:: requests.auth.AuthBase
.. autoclass:: requests.auth.HTTPBasicAuth
.. autoclass:: requests.auth.HTTPProxyAuth
.. autoclass:: requests.auth.HTTPDigestAuth



Encodings
---------

.. autofunction:: requests.utils.get_encodings_from_content
.. autofunction:: requests.utils.get_encoding_from_headers
.. autofunction:: requests.utils.get_unicode_from_response


.. _api-cookies:

Cookies
-------

.. autofunction:: requests.utils.dict_from_cookiejar
.. autofunction:: requests.utils.cookiejar_from_dict
.. autofunction:: requests.utils.add_dict_to_cookiejar

.. autoclass:: requests.cookies.RequestsCookieJar
   :inherited-members:

.. autoclass:: requests.cookies.CookieConflictError
   :inherited-members:



Status Code Lookup
------------------
.. autoclass:: requests.codes

::

    >>> requests.codes['temporary_redirect']
    307

    >>> requests.codes.teapot
    418

    >>> requests.codes['\o/']
    200



Migrating to 1.x
----------------
0.x버전과 1.x버전사이의 차이점을 설명하고 쉽게 개선하게 도와줄 것입니다.


API Changes
~~~~~~~~~~~

* ``Response.json`` 를 사용할수 있습니다. 이 ``Response.json``은 응답 속성이 아닙니다.
  ::

      import requests
      r = requests.get('https://github.com/timeline.json')
      r.json()   # This *call* raises an exception if JSON decoding fails

* ``Session`` API가 변했습니다. Session objects는 더 이상 매개변수를 갖지 않습니다.
  ``Session``은 또한 대문자로 사용합니다. 그러나 이전의 ``session``과 같은 역할을 합니다.


  ::

      s = requests.Session()    # formerly, session took parameters
      s.auth = auth
      s.headers.update(headers)
      r = s.get('http://httpbin.org/headers')

* All request hooks have been removed except 'response'.

* 모든 request hook들은 'response'의 예외에서 제거되었습니다.

* Authentication helpers have been broken out into separate modules.

* 인증을 도와주던 모듈인 requests-oauthlib_ 와 requests-kerberos_ 는 따로 분리되었습니다.

.. _requests-oauthlib: https://github.com/requests/requests-oauthlib
.. _requests-kerberos: https://github.com/requests/requests-kerberos

* 스트리밍 관련 환경변수가 ``prefetch``에서 ``stream``으로 변경되었으며 로직을 바꾸었습니다.
  또한, ``stream``은 raw response 를 읽는데 요구됩니다.
  ::

      # in 0.x, passing prefetch=False would accomplish the same thing
      r = requests.get('https://github.com/timeline.json', stream=True)
      for chunk in r.iter_content(8192):
          ...

* ``config`` 변수는 requests method에서 제거되었습니다. keep-alive 나 최대 redirects와 같은 몇몇 옵션들은 ``Session``을
  사용하시면 됩니다.
  여러 옵션은 환경변수 로깅에 의해 제어됩니다.
  ::

      import requests
      import logging

      # Enabling debugging at http.client level (requests->urllib3->http.client)
      # you will see the REQUEST, including HEADERS and DATA, and RESPONSE with HEADERS but without DATA.
      # the only thing missing will be the response.body which is not logged.
      try: # for Python 3
          from http.client import HTTPConnection
      except ImportError:
          from httplib import HTTPConnection
      HTTPConnection.debuglevel = 1

      logging.basicConfig() # you need to initialize logging, otherwise you will not see anything from requests
      logging.getLogger().setLevel(logging.DEBUG)
      requests_log = logging.getLogger("requests.packages.urllib3")
      requests_log.setLevel(logging.DEBUG)
      requests_log.propagate = True

      requests.get('http://httpbin.org/headers')



Licensing
~~~~~~~~~

One key difference that has nothing to do with the API is a change in the
license from the ISC_ license to the `Apache 2.0`_ license. The Apache 2.0
license ensures that contributions to Requests are also covered by the Apache
2.0 license.

.. _ISC: http://opensource.org/licenses/ISC
.. _Apache 2.0: http://opensource.org/licenses/Apache-2.0


Migrating to 2.x
----------------

비교적 몇가지 변경사항은 1.0버전과 완전히 다릅니다. 그러나 여전히 알고있는 몇가지의 이슈들이 이번릴리즈에 포함되어 있습니다.
이번 릴리즈에 포함되어 있는 새로운 API들의 상세한 변경점은 Cory의 blog_ 에서 확인 할 수 있습니다.
GitHub 이슈에서 새로운 API와 관련된 논의가 되고 있으며 몇 가지의 버그 픽스가 있었습니다.

.. _blog: http://lukasa.co.uk/2013/09/Requests_20/


API Changes
~~~~~~~~~~~
* 다음은 Requests의 예외를 처리하는 방법에 대해 두가지의 변경이 있습니다.
  ``RequestException``은 더 이상 error 타입의 정확하고 세분화시키기 위하여
  ``RuntimeError``가 아닌 ``IOError``를 subclass로 갖고 있습니다.
  또한 인식 불가능한 URL escape sequence에 대해서는 ``ValueError``이 아닌 ``RequestException``가 출력됩니다.

  ::

      requests.get('http://%zz/')   # raises requests.exceptions.InvalidURL

  마지막으로, 정확하지 않은 encoding으로 발생되던 ``httplib.IncompleteRead`` 예외는 ``ChukedEncodingError``가 대신 발생합니다.

* proxy API는 약간 변경 되었습니다. proxy URL을 위한 scheme이 요구됩니다.

  ::

      proxies = {
        "http": "10.10.1.10:3128",    # use http://10.10.1.10:3128 instead
      }

      # In requests 1.x, this was legal, in requests 2.x,
      #  this raises requests.exceptions.MissingSchema
      requests.get("http://example.org", proxies=proxies)


Behavioural Changes
~~~~~~~~~~~~~~~~~~~

* ``headers``의 dictionary키는 이제 모든 Python 버전에서 기본 스트링으로 적용됩니다.
  예를 들면 Python 2에서는 bytestring으로 Python 3에서는 unicode로 적용이됩니다.
  만약 key들을 기본 문자열로 사용하지 않는다면 UTF-8 인코딩을 기본으로 변환하게 됩니다.