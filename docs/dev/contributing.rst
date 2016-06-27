.. _contributing:

Contributor's Guide
===================

만약 이글을 읽는 당신은 아마도 Requests에 기여하는데 흥미를 가진 사람일것입니다.
고맙습니다. 오픈소스 프로젝트들의 생사는 다른사람들의 기여에 달려있습니다,
그리고 사실 당신의 기여에 관해 Requests 프로젝트는 당신에게 매우 관대합니다.
이 프로젝트에 기여하기 위해 이 문서의 가이드라인과 충고를 읽어보세요.
만약 당신이 기여를 생각한다면
이 문서를 읽는것을 시작하세요 그리고 계속하여 이 프로젝트에 기여가 어떻게 이루어지는지 느껴보세요
만약 질문이 있다면, 자유롭게 primary maintainer인 `Ian Cordasco`_ 또는 `Cory Benfield`_ 에게 연락해보세요.
.. _Ian Cordasco: http://www.coglib.com/~icordasc/
.. _Cory Benfield: https://lukasa.co.uk/about

만약 당신이 비기술적인 피드백이나, 철학적인 생각, 뛰어난 아이디어 또는 다른 일반적인 리퀘스트에 관한 생각 또는 Python의 ecosystem에 관한생각,
BDFL들은 `Kenneth Reitz`_ 가 잘 들어줄 것입니다.

이 가이드는 당신이 만드는것의 생각 기여방법에 따라 나누어져있습니다.
섹션에 모든 기여자들에게 적용되는 일반적인 가이드 라인이 포함되어있습니다

.. _Kenneth Reitz: mailto:me@kennethreitz.org

Be Cordial
----------

    **Be cordial or be on your way**. *—Kenneth Reitz*

Requests 는 버그리포팅 혹은 특색에 대한 요구던지 모든 기여에 엄격한 중요한 룰이 있습니다.
중요한 룰은 "`be cordial or be on your way`_" 입니다.


**모든 기여는 환영합니다**, 관련된 모든 사람들을 존중합니다.

.. _be cordial or be on your way: http://kennethreitz.org/be-cordial-or-be-on-your-way/

.. _early-feedback:

Get Early Feedback
------------------

만약 당신이 기여를 했다면, 이것은 완벽하게 다듬고 완성할때까지 당신의 기여가 끝났다고 생각하지 마세요.
모든 사람들이 도와 가능한 많은 피드백을 줄것입니다. 당신은 그 피드백을 가능한 빨리 처리해주세요.
당신의 기여가 편견없는 피드백을 받기위해 마무리되지 않은버전을 올려 운좋게 기여가 받아지길 원하거나,
기여를 위해 많은 일을 하는것을 아끼기위해 빨리 제출하는것은 프로젝트에 맞는 기여 방식이 아닙니다.

Contribution Suitability
------------------------

우리 프로젝트의 메인테이너는 Requests의  적합한 기여인지 아닌지를 확인합니다.
모든 기여는 조심히 생각한후,그러나 가끔, 기여는 거절당할것입니다.
왜냐하면 그들은 프로젝트의 궁극적인 목표와 필요에 맞지 않기때문입니다.
만약 당신의 기여가 거절당한다면 실망하지마세요! 당신이 이 가이드라인을 따른다면, 당신은 다음 기여는 받아들여질 확률이 더욱더 늘어날 것입니다.


Code Contributions
------------------

Steps for Submitting Code
~~~~~~~~~~~~~~~~~~~~~~~~~

코드를 기여할때, 당신은 다음 체크리스트를 따라야 합니다.

1. GitHub에서 repository를 Fork합니다.
2. 자신의 시스템에서 all pass가 나오는지 컨펌을 위한 테스트를 진행합니다.
   자신의 시스템에서 all pass가 나오지 않는다면 왜 실패가 나오는지 확인해 보십시오.
   만약 스스로 원인을 찾지 못한다면, 이 문서의 가이드 라인을 따라서 버그 리포트를 올려보세요. :ref:`bug-reports`.
3. 버그를 입증할 만한 테스트에 대해서 작성하세요. 그게 틀렸다는 확신이 있다면???
4. 그리고 그것을 수정하세요.
5. 자신이 추가한것을 포함한 모든 테스트가 패스되는지 확인하기 위해 전체적인 테스트를 다시 진행하세요.
6. 메인 레포지토리의 ``master`` 브런치에 GitHub Pull Request를 보내세요
   GitHub Pull Requests 는 이 프로젝트와 협업할수있는 방법입니다.

따라야 하는 세부 항목은 아래에 적혀있습니다.

Code Review
~~~~~~~~~~~

In the event that you object to the code review feedback, you should make your case clearly and calmly.
If, after doing so, the feedback is judged to still apply, you must either apply the feedback or withdraw your contribution.

그들이 코드리뷰를 마칠때까지 기여된 코드는 merge 되지 않습니다.
당신의 코드를 강하게 반대하더라도 어떠한 코드리뷰던 피드백을 하세요.
결국 코드 리뷰의 피드백을 거절한다면, 당신의 상황을 명확하게 해야할것입니다.
만약, 그뒤에, 피드백 판단이 여전히 진행된다면, 피드백을 수용하거나 당신의 기여를 철수해야 할것입니다.

New Contributors
~~~~~~~~~~~~~~~~

만약 당신이 오픈소스를 처음 접하거나 이곳에 처음이여도 환영합니다.
Requests는 오픈소스의 세계에 쉽게 접할수있게 초점이 맞춰져있습니다.
만약 니가 어떻게 최고의 기여에 대해서 흥미를 갖고있다면, 메인테이너에게 메일을 보내는것을 고려해보거나 도움을 요청하세요.

:ref:`early-feedback` 를 확인해 보시기 바랍니다.

Kenneth Reitz's Code Style™
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Requests는 `PEP 8`_ 코드스타일을 기반으로 하고있습니다.
우리는 약간의 가이드 라인을 알려주고 있으며, 이 가이드라인은 PEP 8의 표준 윤곽을 보여줍니다.

- 편의 성을 위해 라인의 길이는 79자를 넘어 100자까지 적을수 있습니다.
- 100자를 넘기지 않았을때 너무 불편할경우 라인의 길이는 100자를 넘을수 있습니다.
- 작은따옴표가 문자열에 있지 않다면 언제나 문자열에서는 작은따옴표를 사용합니다.(e.g. ``'#flatearth'``)

추가적으로, PEP8이 제안하는 미각적으로 완벽히 부족한 `line continuations`_의 하나의 스타일입니다.
Requests의 코드에 허락하지 않습니다.::

    # Aligned with opening delimiter.
    foo = long_function_name(var_one, var_two,
                             var_three, var_four)

제발 하지마세요.

Docstring은 다음과 같은 문법을 따릅니다 ::

    def the_earth_is_flat():
        """NASA divided up the seas into thirty-three degrees."""
        pass

::

    def fibonacci_spiral_tool():
        """With my feet upon the ground I lose myself / between the sounds
        and open wide to suck it in. / I feel it move across my skin. / I'm
        reaching up and reaching out. / I'm reaching for the random or
        whatever will bewilder me. / Whatever will bewilder me. / And
        following our will and wind we may just go where no one's been. /
        We'll ride the spiral to the end and may just go where no one's
        been.

        Spiral out. Keep going...
        """
        pass

모든 함수, 메소드, 그리고 클래스들은 docstring을 포함하고있습니다.
오브젝트 데이타 모델 메소드는 (e.g. ``__repr__``) 보통 이룰에 적용되지 안습니다.
Requests를 더욱 좋게 만드는데 도움을 주셔서 감사합니다.

.. _PEP 8: http://pep8.org
.. _line continuations: https://www.python.org/dev/peps/pep-0008/#indentation

Documentation Contributions
---------------------------

문서의 향상은 언제나 환영합니다!
문서파일들은 ``docs/`` 디렉토리안에 있습니다.
문서들은 `reStructuredText`_를 이용해 쓰여있으며, `Sphinx`_를 이용해 만들어 졌습니다.
문서를 기여할때, 문서 파일의 스타일을 따라가는데 최선을 다하세요.
이것은 당신의 문서안에 79자를 넘기는것, 어느정도 격식을 갖추면서 친근하게하고, 이해하기 쉽게 산문체로 작성해주세요.

Python 코드를 설명할때 작은 따옴표를 사용하여 문자열을 나타내 주세요.(``'hello'`` instead of ``"hello"``)

.. _reStructuredText: http://docutils.sourceforge.net/rst.html
.. _Sphinx: http://sphinx-doc.org/index.html


.. _bug-reports:

Bug Reports
-----------

버그리포트는 매우 중요합니다
버그 리포트를 하기전에 open된 이슈던 closed된 이슈던지 `GitHub issues`_ 를 통해 버그가 이전에 알려진것이 아닌지 확인하세요.
가능하다면 다른 기여자들이 많은 시간을 소비한 버그리포트를 복사하여 사용하여도 괜찮습니다.

.. _GitHub issues: https://github.com/kennethreitz/requests/issues


Feature Requests
----------------

Requests는 아직 feature freeze 상태에 있습니다.
단지 BDFL만이 추가하거나 새로운 특징에 대해 찬성할수 있습니다.
메인테이너들은 Requests는 이 시기에 소프트웨어의 최상의 일부가 될것으로 믿고있습니다.

오픈소스 프로젝트를 활발하게 유지하기 위해 중요한 기술중 하나인 기능추가에 대해서는 no라고 답할것입니다.
그러나 언제나 귀를 열어두고 마음에 담아 둘것입니다.

만약 당신이 이러한 특징을 놓친다면
자유롭게 특징에 대해 질문하세요,
그러나 당신의 특징있는 압도적인 가능성은  제안들은 받아들여지지 않는 다는걸 알아두세요.