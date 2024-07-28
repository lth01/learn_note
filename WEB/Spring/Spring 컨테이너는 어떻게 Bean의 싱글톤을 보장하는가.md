---
Date: 2024-03-11
reference: 스프림 핵심 원리 - 기본편
---
## 강의를 듣기 전
AppConfig작성을 따라하면서 두가지 의문이 들었다.

1. Bean은 스프링이 관리하는 최소 범위인데, 어떻게 메서드를 객체처럼 관리하지?
	- java도 메서드를 객체처럼 관리할 수 있나?
2. MemberRepository를 new로 반환하는 메서드가 다른 서비스를 생성할 때 호출되는데, 어떻게 싱글톤을 보장하지?

오늘은 2번 질문의 답을 강의에서 알려줘 다시 복습하려한다.

## Configuration과 바이트코드 조작
스프링은 프레임워크이므로, 자바 메서드 호출까지 조작하기는 어렵다.

그렇다면 어떻게 객체의 단일성을 보장할까?

Configuration 어노테이션이 붙은 클래스를 출력해보자

```java
void ConfigPrint(){
	ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

	System.out.println("bean = " + ac.getBean(AppConfig.class).getClass());
}
```

여기서 순수할 클래스일경우 출력이 아래와 같이 나와야한다.
 > class 패키지명.AppConfig
 
 하지만 스프링의 클래스 명에는 xxxCGLIB가 추가로 붙어 있음을 확인할 수 있다.

### 왜?
스프링은 CGLIB라는 바이트코드 조작 라이브러리를 통해 AppConfig클래스를 상속하는 임의의 클래스를 만들고, 이를 스프링 컨테이너에 등록한다!

AppConfig를 상속하는 임의의 클래스는 다음과 같은 로직이 등록되어있을것이다.

```
if 이미 생성된 객체일 경우
 return 이미 생성된 객체
else
 새로운 객체이므로 생성 로직 호출 및 스프링 컨테이너에 등록
 return 새롭게 생성된 객체
```

@Configuration 어노테이션을 제외할 경우 @Bean어노테이션이 붙은 메서드는 빈으로 등록되지만, 싱글톤이 보장되지 않는다.

### 잠깐
우리는 스프링의 빈들이 기본적으로 싱글톤으로 관리된다고 알고 있지 않았나?

왜 위에서는 이런 설명을 했을까?

정답은 AppConfig 코드 내부에 있다. 의존성을 주입한게 아닌, 직접적인 new 키워드를 통해 객체를 반환한다.(개발자의 미스)

스프링 컨테이너가 관리하는 빈이 아닌 것이다.

동일한 동작을 위해서는 AppConfig에 인스턴스를 선언하고, @Autowired 어노테이션을 붙인 뒤

MemberService 레포지토리 메서드를 호출하던 로직을 인스턴스로 갈아끼워야한다.

