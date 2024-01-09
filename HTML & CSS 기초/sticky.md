---
Date: 2024-01-09
Title: Position Sticky
reference: https://developer.mozilla.org/ko/docs/Web/CSS/position
---
# Sticky
우리가 서비스를 운영하는 홈페이지에 접속하면 주로 *navigation bar*를 목격하게된다.

네비게이션 바는 사용자가 위아래로 스크롤을 하더라도, 보통 화면 위에 붙어있도록 설계되어있다.

더 나아가 [카카오 보도자료](https://www.kakaocorp.com/page/news/pressRelease)처럼 사용자가 일정 페이지 아래로 스크롤을 할 때부터 화면 상단에 붙어있도록 설정된 네비게이션 바를 본적 있을 것이다.

이는 어떻게 관리되는 걸까?

두가지 방법이 있을 것이다

### position fixed 와 javascript를 같이 쓴다..?
네비게이션 바가 고정되야할 스크롤 범위를 지정하고, 범위 내에서 스크롤 이벤트가 발생했을 때 자바스크립트 이벤트로 네비게이션 바의 포지션을 fixed로 바꾸고, 포지션도 같이 지정하는 방법이 있을 것이다.
- 이거... 어렵다! 실제로 부드럽게 동작하는 것도 문제지만, 매번 스크롤해야할 범위를 지정해야하는 등 문제점이 많다.(옵저버로 어느정도 해결가능하지만)

**그럼 어떻게하냐?**
- CSS가 발전하는 방향은 개발자들이 힘들게 구현해야하는 ui를 쉽게 사용할 수 있는 방향으로 발전한다.

sticky역시 동일하다.

개발자들의 사용 편의성을 위해 만들어졌다 볼 수 있다.

## Sticky
sticky는 속성값이 적용된 요소의 조상에 스크롤이 있다면 가장 가까운 부모 요소의 컨텐츠 영역에 달라붙는 특성이 있다.

즉 sticky 요소는 조상의 스크롤 요소를 기준으로 top, left가 적용된 위치에 고정된다!

<img src="../리소스/sticky_example.gif" width="200" height="200">
출처: [npm: fixed-sticky](https://www.npmjs.com/package/fixed-sticky)
다만, 아직 웹 표준이 아니며 IE(XXX)에는 적용이 불가능해 폴리필이 필요하다.
