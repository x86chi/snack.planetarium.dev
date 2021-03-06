---
title: 플라네타리움에서 한 달동안 경험한 이야기
date: 2020-06-02
authors: [suho.lee]
---

안녕하세요. 이번에 [플라네타리움]에 합류하게 된 이수호입니다. 합류한 지 한 달이 된 기념(?)으로 제가 이 회사에 어떤 기대를 하고 입사하였는지, 한 달 동안 무엇을 했고 플라네타리움은 어떻게 일하는지 간단하게 소개해 보는 시간을 가지고자 합니다.

[플라네타리움]: https://planetariumhq.com/

입사 계기
-------

작년 6월경, [2019 스프린트 서울]이라는 행사에 참여하게 되었습니다. 무슨 오픈 소스에 기여할까 고민하던 저에게 당시 플라네타리움 팀원이셨던 홍민희 님이 [Libplanet]에 기여를 해 보면 어떻겠냐는 제안을 해 오셨습니다. 그렇게 즐겁게 기여를 마치고 나서 잊지 않고 있다가, 플라네타리움 구인 기간에 지원하여 입사하게 되었습니다. 이 곳이라면 내가 얻었던 즐거운 경험을 남들에게도 줄 수 있고, 나 자신도 그런 경험을 계속 쌓아나갈 수 있을 것이라고 판단하였기 때문입니다.

[2019 스프린트 서울]: https://www.sprintseoul.org/2019-06-29/
[Libplanet]: https://libplanet.io/ 

한 달 동안 한 것
------------
한 달 동안은 기본적으로 Libplanet의 구조와 C#에 대한 이해를 끌어올렸습니다. 개념 증명(<abbr title="proof of concept">PoC</abbr>) 수준의 Libplanet을 새로 작성해 보고, 각 모듈별 설명을 들을 때마다 그 모듈에 대한 이슈를 하나씩 제거해 나갔습니다.

{{<
figure
  src="images/resolved-issue.png"
  caption="한 달 동안 제거해나간 이슈 목록"
>}}

개인적으로 매우 효과를 본 방법이라고 생각합니다. 설명을 들을 때는 높은 문맥의 이야기만 대부분 듣기 때문에, 이 높은 문맥에서 일어나는 일들이 낮은 문맥에서 어떻게 일어나는지 직접 알아가며 체득하고, 실제로 문제를 해결하기 위해 분석하다 보니 이해도가 높았습니다.

또한 일일 업무일지를 저 개인적으로 적어나갔습니다.

{{<
figure
  src="images/daily-reports.png"
  caption="업무일지 목록"
>}}

{{<
figure
  src="images/daily-report.png"
  caption="업무일지"
>}}

이렇게 간단하게 독백하듯 막히는 것을 적어놓는 습관을 들이게 된 이유는 바로 *데일리 미팅* 때문입니다.

회의 문화
-------
Libplanet팀에서 하는 회의 종류에는 총 3가지 있습니다.

- 데일리 미팅
- 주간 미팅
- 분기별 회고

이 중 <dfn>데일리 미팅</dfn>(줄여서 <dfn>데일리</dfn>)은 간단하게 현재 하고 있는 일을 오후 2시에 공유하는 미팅입니다. 보통 자기가 '집중 모드'에 들어간 경우, 누군가 깨워 주거나 아이디어를 주기 전까지는 계속 오버엔지니어링 혹은 이른바 삽질을 계속 하게 됩니다. 하지만 데일리 미팅을 진행하므로써 그것을 미연에 방지하고 서로의 일을 조금씩 도와줄 수 있다는 점이 좋았습니다. 또한 저 개인적으로는 데일리용 업무일지를 적으면서 자기객관화가 쉽게 이루어져, 업무 자체에도 도움이 많이 되었습니다.

데일리는 많은 곳에서 진행하지만, 이걸 자신의 *일 했다는 알리바이 증명 용도*로 사용하게 되는 경우가 많습니다 (예: 수직적 의사결정 구조에서 하급자가 상급자에게 보고하는 형태). 하지만 저희 팀은 데일리 미팅 때 내가 하는 일을 부풀리거나 보고하는 게 아닌 막히고 있는 부분을 솔직하게 밝히고, 모르는 부분이 있으면 물어보는 등 서로에게 정보를 공유하는 점이 매우 마음에 들었습니다.


실천하는 팀원들
----------------------------
의사 결정에 있어 실패에 대한 책임을 묻는 것은 아무 의미도 없다는 말은 진부할 정도로 누구나 잘 알고 있습니다. 실패하면 빠르게 공유·피드백하고, 이를 통해 대안책을 찾아 나가는 것이 좋다는 것도 누구나 알고 있습니다. 하지만 실천하는 것은 말처럼 쉽지가 않습니다.

제가 들어온 지 얼마 안 됐을 때, Windows와 macOS에서 모두 동작하는 작은 GUI 앱을 만들 일이 있었는데, 보름 전에 쓰기로 결정했던 크로스플랫폼 GUI 프레임워크가 저희가 풀려는 문제 상황에 적합하지 않다는 것을 뒤늦게 깨닫게 되었습니다. 그렇지만 해당 작업을 진행하던 사람은 어떻게든 문제를 해결하려고 집중하다 보니 이미 일주일도 넘는 시간이 훌쩍 지나 있었고, 결과적으로 GUI 프레임워크를 교체하자는 결정을 늦게 내리게 되어서 이로 인해 일정이 뒤로 밀리게 되었습니다. 이러한 실패는 보통 회고 시간에 당연히 에둘러서라도 아쉬운 소리를 한다든가, 여태 얼마나 오랜 시간을 허비하였는지에 대해 (설령 그럴 의지가 없었다 하더라도) 비난하게 됩니다.

하지만 오히려 <q>우리는 이런 선택이 잘못되었음을 알았다. 얻은 게 있으니 상관없다</q>라고 말하며 앞으로 이런 결정을 하지 않을 방법을 찾았고, <q>좀 더 자주 모여 얘기하면 문제를 해결하는 데 집중하느라 놓치는 것들을 알 수 있을 것</q>이라는 결론에 도달했습니다. 그리고 구체적인 방안으로, 데일리 이외에 저녁의 짧은 공유 세션을 하나 더 만들었습니다.

이후 팀 내에서 오전에는 작업이 순조롭다 생각했으나 몰입하다 보니 오후까지 같은 걸 붙잡고 늘어지고 있었단 걸 저녁 공유 세션을 통해 깨닫게 되어서 좋았다는 피드백이 있었습니다.

이런 식으로 무슨 일이 순조롭게 되지 않았을 때 <q>왜 계획대로 하지 못했냐</q>고 다그치는 게 아닌, <q>그럼 다른 방법으로 해보자</q>라는 팀 분위기가 매우 좋았습니다.

그 외 복지
--------
여러 복지가 있지만, 그 중에서도 기억에 남는 복지들입니다.

- 리모트 근무
- (진짜) 수평적 의사결정
- 멋지고 뛰어난 사람들

마치며
----
이 글을 쓰고 있던 도중, 엔지니어링 워크샵에서 이런 발언이 나왔습니다.

> 우리 이름을 스낵이라고 지어 놓고, 너무 무거운 글들이 올라오는 거 아니에요? (웃음)

생각해보니 그렇습니다. 그래서 최대한 글 양을 줄이고 스크린숏을 넣는 등 여러 시도를 해 보았는데 어떠셨을지 모르겠네요. 읽는 데 편안했었으면 좋겠습니다.

마지막으로 [Libplanet]은 여러분의 기여를 언제나 기다리고 있습니다! 오픈 소스에 관심있는 분들은 저희 [디스코드 채널]에도 놀러오세요. 감사합니다!

[디스코드 채널]: https://discord.gg/planetarium
