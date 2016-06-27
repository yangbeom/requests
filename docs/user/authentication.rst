.. _authentication:

Authentication
==============
이문서는 Requests를 이용한 여러종류의 인증에 관해 이야기합니다.

많은 웹서비스는 여러가지 방법의 인증을 요구합니다.
우리는  Requests에서 쉬운것부터 복잡한 인증까지 다양한 인증폼을 제시합니다.

Basic Authentication
--------------------
많은 웹서비스들이 HTTP 기본 인증을 요구합니다.
이것은 가장 간단한 종류입니다, 그리고 Requests는 독창적인 방법으로 지원합니다.

HTTP 기본 인증을 필요로하는 리퀘스트를 만드는것은 매우 간단합니다::

    >>> from requests.auth import HTTPBasicAuth
    >>> requests.get('https://api.github.com/user', auth=HTTPBasicAuth('user', 'pass'))
    <Response [200]>

사실 , HTTP Basic 인증은 Requests를 이용해 쉽게 처리할수 있는 인증입니다. ::

    >>> requests.get('https://api.github.com/user', auth=('user', 'pass'))
    <Response [200]>


auth파라메터에 튜플로 넣는 것은 위의 예제와 같이 ``HTTPBasicAuth``로 인증하는것과 같습니다.



netrc Authentication
~~~~~~~~~~~~~~~~~~~~
만약 인증 메소드가 없고 ``auth``인자가 주어진, Requests는 URL의 hostname 에 인증을 위한 user's netrc 파일으로부터 시도할것입니다.
netrc 파일은 HTTP 인증 헤더를 무시합니다?

만약 제어를 위한 호스트네임을 찾는다면, 그 리퀘스트는 HTTP Basic Auth를 포함하여 전송될것입니다.



Digest Authentication
---------------------
HTTP 인증폼에서 유명한것중 하나인 Digest Authentication은
그리고 Requests는 이것을 다음과 같이 지원합니다. ::

    >>> from requests.auth import HTTPDigestAuth
    >>> url = 'http://httpbin.org/digest-auth/auth/user/pass'
    >>> requests.get(url, auth=HTTPDigestAuth('user', 'pass'))
    <Response [200]>


OAuth 1 Authentication
----------------------
OAuth는 여러 web API들을 이용하기 위한 흔한 인증폼입니다.
``requests-oauthlib`` 라이브러리는 Requests 유자가 쉽게 OAuth 인증 리퀘스트 만드는것을 도와줍니다::

    >>> import requests
    >>> from requests_oauthlib import OAuth1

    >>> url = 'https://api.twitter.com/1.1/account/verify_credentials.json'
    >>> auth = OAuth1('YOUR_APP_KEY', 'YOUR_APP_SECRET',
                      'USER_OAUTH_TOKEN', 'USER_OAUTH_TOKEN_SECRET')

    >>> requests.get(url, auth=auth)
    <Response [200]>


OAuth가 어떻게 동작하는지 더 알고싶다면 `OAuth`_ 공식 사이트를 확인해보세요.
requests-oauthlib에 관한 예제와 문서는 GitHub의 `requests_oauthlib`_ 레포지토리에 올라와있습니다.



Other Authentication
--------------------
리퀘스트는 다른 인증폼을 쉽고 빠르게 연결시킬수 있게 디자인 되었습니다.
오픈소스 커뮤니티 멤버들은 복잡하거나 잘 사용하지 않는인증 핸들러를 자유롭게 만들 수 있습니다.
`Requests organization`_에서 다음과 같은것들을 가져와 사용할 수 있습니다.

- Kerberos_
- NTLM_

만약 당신이 어떠한 인증폼을 사용하고 싶다면, 그들의 GitHub page에 접속해 따라해보세요.

New Forms of Authentication
---------------------------
만약 원하는 인증폼을 실행하는것을 찾지 못했을땐, 직접 만들어 실행하게 만들 수 있습니다.
Requests는 당신만의 인증폼을 쉽게 추가할수 있습니다.

서브클래스인 :class:`AuthBase <requests.auth.AuthBase>` 과 ``__call__()`` 메소드를 이용하면 됩니다. ::

    >>> import requests
    >>> class MyAuth(requests.auth.AuthBase):
    ...     def __call__(self, r):
    ...         # Implement my authentication
    ...         return r
    ...
    >>> url = 'http://httpbin.org/get'
    >>> requests.get(url, auth=MyAuth())
    <Response [200]>

리퀘스트에 인증 핸들러를 붙일때, 리퀘스트 설정에 자동으로 추가됩니다.
``__call__`` 메소드는 어떠한 인증을 위한 요구사항에도 작동합니다.
인증을 위한 어떤 폼들은 추가적으로 hook을 추가하여 추가적인 기능을 제공합니다.

추가적인 예제들은 `Requests organization`_와 ``auth.py``파일에서 찾을 수 있습니다.

.. _OAuth: http://oauth.net/
.. _requests_oauthlib: https://github.com/requests/requests-oauthlib
.. _Kerberos: https://github.com/requests/requests-kerberos
.. _NTLM: https://github.com/requests/requests-ntlm
.. _Requests organization: https://github.com/requests
