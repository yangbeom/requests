.. Requests documentation master file, created by
   sphinx-quickstart on Sun Feb 13 23:54:25 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Requests: HTTP for Humans
=========================

Release v2.10.0. (:ref:`Installation <install>`)

Requests 는 사람이 이해하기 쉬운 순수한 HTTP library입니다.
**경고:** Recreational 다른 HTTP라이브러리의 사용은 다음과 같은 위험성을 내포할수 있습니다.
보안 취약점, 장황한 코드, 쓸데없는 시간낭비,
끊임없는 문서를 읽기, 짜증, 두통, 심지어 죽음까지 포함되어 있을 수 있습니다.

다음은 만능 Requests 사용법입니다.::

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
    {u'private_gists': 419, u'total_private_repos': 77, ...}

Requests를 사용하지 않은 유사코드는 `여기 <https://gist.github.com/973705>`_ 에서 보실 수 있습니다.

Requests는 당신이 손쉽게 거대한 파일을 HTTP/1.1 requests로 보내는것을 가능케 해줍니다.
당신이 원하는 url에 쿼리를 추가하거나 form-encode된 POST 데이터를 수동으로 추가할 필요가 없습니다.
Requests를 이용하여 HTTP connection pooling 을 자동으로 유지할수있습니다.
HTTP connection pooling과 Keep-alive는 Requests가 urllib3를 이용하여 100% 자동으로 이루어집니다.


User Testimonials
-----------------

Her Majesty's Government, Amazon, Google, Twilio, Runscope, Mozilla, Heroku,
PayPal, NPR, Obama for America, Transifex, Native Instruments, The Washington
Post, Twitter, SoundCloud, Kippt, Readability, Sony, and Federal U.S.
Institutions that prefer to be unnamed claim to use Requests internally.

**Armin Ronacher**
    Requests is the perfect example how beautiful an API can be with the
    right level of abstraction.

**Matt DeBoard**
    I'm going to get @kennethreitz's Python requests module tattooed
    on my body, somehow. The whole thing.

**Daniel Greenfeld**
    Nuked a 1200 LOC spaghetti code library with 10 lines of code thanks to
    @kennethreitz's request library. Today has been AWESOME.

**Kenny Meyers**
    Python HTTP: When in doubt, or when not in doubt, use Requests. Beautiful,
    simple, Pythonic.

Requests is one of the most downloaded Python packages of all time, pulling in
over 7,000,000 downloads every month. All the cool kids are doing it!

Supported Features
------------------
Requests는 최신 web에 대한 준비가 되어 있습니다.

- 국제적 도메인과 URLs
- Keep-Alive & Connection Pooling
- 세션과 쿠키
- 브라우저와 같은 SSL을 이용한 연결
- 기존/경량화된 인증
- 우아한 Key/Value 쿠키
- 컨텐츠 자동 디코딩
- Respone 바디의 유니코드화
- Multipart 파일 업로드
- HTTP(S) Proxy 지원
- Connection Timeouts
- 스트리밍 다운로드
- ``.netrc`` 지원
- 파편화된 Requests
- 스레드 안정성

Requests는 Python 2.6 — 3.5, 그리고 PyPy를 지원합니다.

The User Guide
--------------

이 문서는 Requests에 관한 배경 지식 등이 포함되어 있으며, Requests를 최대한으로 활용할수 있도록 단계별로 진행합니다.

.. toctree::
   :maxdepth: 2

   user/intro
   user/install
   user/quickstart
   user/advanced
   user/authentication


The Community Guide
-------------------
Requests의 ecosystem과 커뮤니티를 담고 있는 문서입니다.

.. toctree::
   :maxdepth: 1

   community/faq
   community/recommended
   community/out-there
   community/support
   community/vulnerabilities
   community/updates
   community/release-process

The API Documentation / Guide
-----------------------------
function, class 혹은 method에 관한 상세한 정보를 알기 원하는 이용자를 위한 문서입니다.

.. toctree::
   :maxdepth: 2

   api


The Contributor Guide
---------------------
이 프로젝트의 컨트리뷰터가 되고싶은 이용자를 위한 문서입니다.

.. toctree::
   :maxdepth: 3

   dev/contributing
   dev/philosophy
   dev/todo
   dev/authors


더이상의 가이드는 없습니다. 당신은 이미 가이드가 필요 없기 때문이죠 =)