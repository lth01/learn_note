---
Date: 2024-01-02
reference: 컴퓨터 네트워킹 하향식 접근
Subject: DNS
---
# 현실과 기술의 괴리

두가지의 문장중 더 외우기 쉬운 것을 골라보자.

1. 밀이 바람에 흔들리는 것을 "늑대가 달린다"라고 한다.
2. 102938928392098102938100

두가지를 반드시 외워야한다고 할때, 보통의 사람이라면 첫번째가 더 외우기 쉬울 것이다.

사람은 패턴화된 단어를 무작위로 나열된 글자보다 쉽게 이해한다. 이는 인터넷을 돌아다니다 흔히 볼 수 있는 "단어 우월 효과" [(1)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%96%B4_%EC%9A%B0%EC%9B%94_%ED%9A%A8%EA%B3%BC) 에서도 확인할 수 있다.

그렇다면, 우리가 웹 사이트를 방문할 때에도 이와 같은 원리가 적용된다. 라우터는 IP주소를 받고있으나 사용자가 기억하는 주소는 "www.abc.co.kr"와 같이 단어의 형태를 띈다.

여기서 하나의 의문이 생긴다. 그러면, 어떻게 웹 사이트의 IP주소를 알 수 있지?
> 이런 의문을 일찍히 머리 좋은 사람들은 생각했었다. 그래서 **도메인주소를 IP주소로 바꿔주는 Domain Name System**을 개발한 것이다.

# DNS가 어떻게 동작하는가?
DNS의 동작을 살펴 보기 전에 한가지 생각해보자.

DNS도 일종의 서버 형태일 것이고, domain을 요청하면 ip형태로 반환해줄것이다.

그렇다면 서버가 어떤 형태로 존재해야할까..?

## 단일서버
가장 간단한 것은 엄청나게 성능이 좋고 엄청나게 효율이 좋은 슈퍼메가울트라 서버가 있으면 될 것이다.

하지만 서버는 분명히 물리적인 한계가 존재하고, 우리가 정보를 요청하는 위치에 따라 통신 시간도 차이가 나므로(미국에 단일 서버가 있을때, 미국에 사는 사람이 미국서버에 통신하는 것과 한국 사람이 미국서버에 통신하는 것중 당연 전자가 빠를 것이다) 

따라서 단일보다는 분산된 서버 구조를 갖는것이 현명하다.

## 분산서버 구조

![[Pasted image 20240102181242.png]]
[원문 CloudFare](https://www.cloudflare.com/ko-kr/learning/dns/glossary/dns-root-server/)

위의 사진을 보자. 가장 큰 Root로부터, Tree구조를 가지며 하위로 갈수록 세부 도메인 주소를 표현한다.

우편물의 세부 주소를 표현한다고 보면 된다.

가장 범위가 넓은 주소지부터 하나하나씩 돌아가며 하위 주소를 찾는 것이다.

실제로 위의 트리의 노드들은 마지막 depth를 제외하면 모두 dns 서버인데 종류는 아래와 같다.

1. **Root DNS Server**
2. **TLD(Top Level Domain) DNS Server
3. **Authoritative Name Server**
4. **Local DNS Server**
### Root DNS Server
가장 큰 서버로 TLD서버에 대한 주소를 저장하고있다. 세계의 13개만 존재한다.

### TLD(Top Level Domain) DNS Server
Authoriatative Name Server의 주소를 저장하고있다.

### Authoriative Name Server
실제 도메인과 IP를 매핑한 데이터가 저장된 서버. 도메인 호스팅 업체의 네임서버를 의미한다.

### Local DNS Server
사용자가 가장 먼저 접근하는 서버 도메인주소를 찾기위해 DNS 쿼리를 할때, 여기를 제일 먼저 거친다.
보통 DHCP, 혹은 랜선을 꽂을때, Local DNS서버 주소를 자동으로 할당받거나 회사 내부망의 경우 수동으로 설정한다.

전형적인 Top Down 방식으로 IP주소를 가져옴을 알 수 있다.(로컬서버 제외)

# DNS 쿼리 과정
## 1. 반복적인 질의(iterative query)
로컬 DNS 서버가 Root DNS Server로부터 응답을 받고 TLD DNS 서버에 다시 질의를 하여 Authoritative DNS Server의 주소를 가져온다. 최종적으로 Authoritative DNS Server에게 도메인 주소를 질의하여 IP를 가져온다. 

![[Pasted image 20240102183350.png]]
보면 Local DNS 서버가 직접 서버별로 질의를 하여 최종적으로 호스트에게 IP주소를 전달한다.

이럴 경우 Root DNS Server, TLD DNS Server, Authoritative DNS Server는 서로 통신하지 않으므로 각각이 가진 정보를 공유할 수 없다.

즉, **Local DNS Server를 제외하고는 도메인 정보를 캐시할 수 없다!**
## 2. 재귀적 질의(recursive query)
![[Pasted image 20240102183440.png]]
반복적 질의와 다르게 Local DNS Server가 Root DNS Server로 요청을 던지면 Root는 TLD에게 TLD는 Auth서버에게 응답을 요청하고 다시 재귀적으로 응답을 받게된다.

반복적인 질의는 Local Server가 한명한명 물어가며 정보를 가져왔다면, 재귀적 질의는 한명에게 물어보고 취합된 정보를 가져오는 방식이다.

이와같은 방법은 단독 서버와 유사한 구조이므로(요청이 특정 Root DNS Server로 집중되므로) 서버의 부하가 문제가 될 수 있으나, 응답되는 IP주소를 DNS 서버가 캐시하는 경우 빠르게 정보 공유가 가능하다는 장점이 존재한다.

### 그래서 뭘쓰는데?
둘다 장단점이 있어, 실제로는 둘다 섞어서 쓴다.

