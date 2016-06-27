.. _faq:

Frequently Asked Questions
==========================

Requests에 관한 자주 나오는 질문과 답을 모아놓았습니다.

Encoded Data?
-------------

Requests는 자동적으로 gzip으로 encode된 응답들을 해제 합니다.
그리고 최적으로 unicode에 대한 컨텐츠를 decode된 응답으로 바꿔줍니다.
만약 필요하다면 소켓을 포함한 raw 응답을 받을수도 있습니다.

Custom User-Agents?
-------------------

Requests는 User-Agent를 쉽게 변경할수 있습니다. 물론 어떠한 HTTP Header들도 마찬가지 입니다.

Why not Httplib2?
-----------------
Chris Adams 가 `Hacker News <http://news.ycombinator.com/item?id=2884406>`_ 에 요약해 두었습니다.

    httplib2 is part of why you should use requests: it's far more respectable
    as a client but not as well documented and it still takes way too much code
    for basic operations. I appreciate what httplib2 is trying to do, that
    there's a ton of hard low-level annoyances in building a modern HTTP
    client, but really, just use requests instead. Kenneth Reitz is very
    motivated and he gets the degree to which simple things should be simple
    whereas httplib2 feels more like an academic exercise than something
    people should use to build production systems[1].

    Disclosure: I'm listed in the requests AUTHORS file but can claim credit
    for, oh, about 0.0001% of the awesomeness.

    1. http://code.google.com/p/httplib2/issues/detail?id=96 is a good example:
    an annoying bug which affect many people, there was a fix available for
    months, which worked great when I applied it in a fork and pounded a couple
    TB of data through it, but it took over a year to make it into trunk and
    even longer to make it onto PyPI where any other project which required "
    httplib2" would get the working version.


Python 3 Support?
-----------------
물론입니다! 다음은 공식적으로 지원하는 Python 버전들입니다

* Python 2.6
* Python 2.7
* Python 3.1
* Python 3.2
* Python 3.3
* Python 3.4
* PyPy 1.9
* PyPy 2.2

What are "hostname doesn't match" errors?
-----------------------------------------

다음 에러들은 서버의 인증서와 Requests가 접속한 hostname이 일치하지 않는
:ref:`SSL certificate verification <verification>` 실패에서 에서 일어납니다.

만약 서버의 SSL 설정이 정확히 이루어졌으며,
Python 2.6 혹은 Python 2.7을 이용하고 있다면 Server-Name-Indication 을 별도로 알려주어야 합니다.

`Server-Name-Indication`_, 혹은 SNI, 클라이언트가 호스트네임에 접속해서 서버를 부르는 공식적인 SSL 확장기능입니다.
이것은 서버에서 `Virtual Hosting`_ 를 사용한다면 중요합니다.
서버에서 하나 이상의 SSL사이트를 호스팅중일때 적절한 인증서를 클라이어트가
접속한 hostname을 기반으로 반환할수 있도록 해야합니다.
Python3 와 Python 2.7.9+ 에서는 SSL modules에서 SNI에 관한 기능을 지원하고 있습니다.
Python 2.7.9 이전 버전에서 Requests를 사용할때 SNI를 이용하는 방법은  `Stack Overflow answer`_ 를 참고하시기 바랍니다.


.. _`Server-Name-Indication`: https://en.wikipedia.org/wiki/Server_Name_Indication
.. _`virtual hosting`: https://en.wikipedia.org/wiki/Virtual_hosting
.. _`Stack Overflow answer`: https://stackoverflow.com/questions/18578439/using-requests-with-tls-doesnt-give-sni-support/18579484#18579484
