---
Date: 2024-01-25
Title: 멘땅에 헤딩 - 공식문서 탐색!
---
# Redis에서 JSON
Redis에서 JSON은 기본적으로 4가지 역할을 지원한다.

1. Full support for the JSON Standard
	-[표준 규약](https://datatracker.ietf.org/doc/rfc7159/?include_text=1)
2. 도큐먼트 내부 요소의 선택/수정을 위한 JSONPath 문법을 지원함
3. 도큐먼트는 트리 구조 내부에 바이너리 데이터로 저장되어 빠른 하위 요소 접근이 허가됨
4. 모든 JSON 벨류 타입에 대한 연산의 원자성이 보장됨
	- JSON 값 연산에 대해서 모두 성공하거나, 실패함을 보장한다는 의미
	 >JSON에 연산을 진행하는 과정은 하나의 트랜젝션으로 취급된다는 의미이다.
	 >즉 JSON타입도 값의 정합성을 보장한다는 거임
	 
## JSON 메소드
### JSON.SET
JSON 객체를 만들기 위해선, 일단 도큐먼트 루트 $를 생성해야한다.

```redis
JSON.SET doc $ '{"a": 2}'

JSON.SET doc $.a '3' // equals at js foo = {a: '3'} foo.a
```

JSON.SET으로 path를 지정하면 해당되는 path는 입력한 value로 모두 바뀌게된다.


### JSON.GET
JSON 객체를 배열 형태로 반환한다.

```redis-cli
JSON.SET doc $ '{"a":2, "b": 3, "nested": {"a": 4, "b": null}}'

JSON.GET doc ..a $..b
"{\"$..b\":[3,null],\"..a\":[2,4]}"
```

JSON.GET에서 
..a는 $으로부터 하위에 key로 a를 갖는 값을 모아 배열로 반환한다.
$..b역시 $부터 하위의 key가 b인 value를 배열로 모아 반환한다.

GET은 ..a, $..b를 모아 하나의 배열로 처리함

path에 대해서는 [JSON Path](https://goessner.net/articles/JsonPath/)를 따른다. 


## JSON Limitation
최대 depth는 128까지 지원한다. 
