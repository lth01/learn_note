---
Date: 2024-01-25
Title: 멘땅에 헤딩 - 공식문서 탐색!
---
# Redis의 기본 자료형 String
Redis의 String은 바이트 시퀀스, 텍스트, 시리얼화된 Object, 바이너리 배열을 포괄한다.
>바이너리 배열 역시 String으로 포함된다는 것은 redis의 키로는 어떤 바이너리 데이터도 가능하다는 것이다! (binary safe)

주로 캐싱을 위해 사용하지만, 카운터, 비트 연산등의 추가적인 기능도 지원한다!

String의 크기는 512MB이상 불가하다.

## 성능
대부분의 연산은 O(1)을 보장한다. 하지만, SUBSTR, GETRANGE, SETRANGE등의 연산들은 O(n)의 성능을을 보장하므로, 성능이슈가 발생할 수 있다.
>❓O(n)이면 빠른거아닌가..?
>✅ Redis는 레이스 컨디션 방지를 위해 싱글 스레드로 동작한다. 이 말은 여러 서버가 요청을 보내왔을때, 이를 순차적으로 처리할 수 밖에 없다는 것이다. 즉, O(n)의 동작을 수행할 동안 다른 요청은 모두 멈춘다...



