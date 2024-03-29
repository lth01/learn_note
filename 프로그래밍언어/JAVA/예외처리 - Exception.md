---
Date: 2024-02-06
Subject: 예외처리 - Exception
reference: 헤드퍼스트 자바 - 3판
---
# 예외란 언제 발생하는가
우리가 실무에서 프로그램을 개발한다 가정하자. 우리가 1 ~ 100까지 코드를 만드는 경우는 없을 것이다.

선임, 후임 혹은 유료 프로그램, 오픈소스에서 개발된 소스를 사용할 것이다.

그렇다면, 우리가 API 형태로 개발된 소스코드를 모두 분석하지 않았다고 가정했을 때, 각 소스코드에서 발생할 위험을 미리 알 수 있을까?
- 우리가 소스코드를 분석하기 전까지 그건 불가능할 것이다.

이제 우리가 소스코드를 배포하는 사람이라고 가정해보자. 프로그램을 일종의 상품이라고 가정하고, 사용설명서를 제공한다 생각해보겠다.

사용 설명서에는 이런 설명들이 있을것이다.
1. 상품의 장단점
2. 상품의 사용방법
3. 주의사항
4. 문제 사항 발생시 대처 방법

여기서 3, 4번을 담당하는게 예외이다. 소스코드에서 어떤 상황에 문제가 발생할 수 있고, 이때 어떤 이유로 오류가 발생한 것인지를 예외처리를 통해 넘겨주게된다.

## 예외처리의 예제
예외 처리 예제를 보여주기 위해 헤드퍼스트 자바에선 getSequencer()메소드를 호출한다.

[JAVA API 17 getSequencer](https://softeer.ai/practice/6249) 를 들어가면 Throws 문맥을 볼 수 있는데, 여기에는 언제 이런 예외가 발생할 수 있고, 어떤 예외 객체를 던지는지 확인할 수 있다.

이렇게 예외 명시가 되어있는 메서드를 사용하기 위해선 반드시 메서드에서 던질 수 있는 예외에 대한 처리를 수행해야한다.

### 예외처리 기본 문법
예외 처리를 위한 기본적인 문섭은 try - catch이다.
```java
public static void main(String[] args){
	try{
		%% do something dangerous %%
		if(something failed){
			 throw new Exception();
			 %% 예외 객체를 만들고, throw 예약어로 던진다. %%
		}
	}catch(Exception e){
		%% Exception handling %%
	}
}
```

### 예외도 객체다
예외 역시 객체이다!

Throwable 인터페이스를 구현한 익셉션 클래스를 최종 부모로 삼는다.

예외가 굳이 객체일 이유가 있을까?
- 뒤에 후술할것이다.
## 예외의 종류
예외에는 두가지 종류가 존재한다.

1. 컴파일타임에 처리되는 예외
2. 런타임에 처리되는 예외

컴파일러는 2번 예외를 제외한 모든 예외를 확인한다.

### 왜 런타임 예외는 컴파일러가 잡지 않을까?
예를 들어보자 배열의 요소에 인덱스를 사용하여 접근하려고 한다.
인덱스의 값은 알지 못하지만 컴파일러가 배열에 접근하려는 것은 런타임에러가 발생할 수 있다 생각하여 예외처리를 강제로 요구할 수 있다.

그런데, 실제로는 이렇게 처리하지 않는다. 런타임 예외는 별도로 검사하지않는다. 왜 그런걸까?

#### 상품의 자체적인 결함이 있는 경우
런타임 예외는 상품에 우리가 예상치 못한 문제가 있던 것으로 생각할 수 있다.

사용자가 잘못 사용하거나, 환경의 문제로 발생하는 예외는 미리 주의시킬 수 있다.

하지만, 상품 개발자가 파악하지 못한 오류에 대해서는 미리 파악할 수 없다. 그렇다고 이 모든 가능성을 모두 고려해서 사용 설명서에 적어놓는 것은 소비자에게 오히려 혼란만 가중시킬 뿐이다.

따라서, 컴파일러는 런타임 예외가 예상치 못한 상황에서 논리를 통한 예측이 불가능한 경우에 발생한다 생각하여, 별도로 예외처리를 강제하지 않는다.

## try/catch 블록에서 실행 흐름
try문 내부에서 소스를 실행하다 오류가 발생할 경우, 오류가 발생한 코드 라인 이후로는 실행이 되지 않은 상태로 catch문으로 넘어가게된다.

이는 파일 입출력에서 문제가 발생할 수 있다.

```java
public static void FileRead(String filePath){
	try{
		File f = new File(filePath);
		f.readToEnd();  %% 여기서 예외가 발생한다.  %%
		f.close();
	}catch(Exception e){
		return void;
	}
}
```


파일은 열려진 상태이지만, close가 호출되지 않는다. 물론 GC에 의해 언젠가 사라지겠지만, 동일한 파일 경로에서 파일을 변경하려할때 이미 열려진 파일이라 파일 변경이 불가능하다 에러가 발생할 것이다.

그래서 언제나 실행해야할 부분은 finally문에 추가하여야한다.

```java
try{
	실행
}catch{
	실행하다 예외가 발생하면 여기를 거침
}finally{
	예외가 발생하든, 정상적으로 호출이 완료되었던 여기를 거쳐감
}
```


## 예외의 다형성
예외 역시 객체다!

이는 Exception1, Exception2 가 동일한 부모를 상속하고 있다면, 부모 예외만 핸들링할 수 있다.

다만 컴파일러는 부모 클래스의 예외를 처리하면, 더이상 상속 계층에 있는 예외는 처리하지 않아도 된다 생각하므로, 반드시 하위 예외 클래스 -> 상위 예외 클래스로 예외문을 처리하자


## try-with-resource
우리가 BufferedReader로 파일을 읽어온다 했을때 아래와 같이 코드를 짤 수 있다.

```java
public static void main(String[] args){
	try{
		BufferedReader br = new BufferedReader(new FileReader(filePath));
		br.readLine();
		br.close();
	}catch(IOException e){
		%% do something %%
	}finally{
		
	}
}
```

하지만, BufferedReader객체가 생성될 때, readLine중 예외가 발생하면  br.close()는 호출되지 않을것이다.

BufferedReader 객체는 생성되었을 때, 반드시 close()메서드가 호출되어야한다.

그래서 아래처럼 코드를 바꿔보자
```java
public static void main(String[] args){
	try{
		BufferedReader br = new BufferedReader(new FileReader(filePath));
		br.readLine();
	}catch(IOException e){
		%% do something %%
	}finally{
		br.close();
		%%이 코드는 문제가 없을까? %%
	}
}
```

얼핏 보면 문제가 없어보이지만.. br.close()역시 예외처리가 필요하다.

예외처리에 문제가 없으려면 아래와 같이 코드가 구성되어야한다.

```java
public static void main(String[] args){
	try{
		BufferedReader br = new BufferedReader(new FileReader(filePath));
		br.readLine();
	}catch(IOException e){
	}finally{
		if(br != null){
			try{
				br.close()
			}catch(IOException e){
			}
		}
	}
}
```

BufferedReader 하나만 사용해도 이렇게 복잡한 코드가 나온다.
- 이런 코드가 반복된다면 개발자가 파악해야할 핵심 로직보다, 예외처리로 인한 코드가 더 많아질 것이다. 이는 가독성의 문제를 야기한다.

자바에서는 이런 문제를 해결하기 위해 try-with-resource라는 문법적 설탕을 제공한다

try-with-resource를 사용하려면 이런식으로 작성해야한다.

```java
try(BufferedReader br = new BufferedReader(new FileReader(filePath))){

}catch(Exception e){

}
```

이렇게 호출하면 예외, 정상 실행 여부와 상관없이 try-catch문을 빠져나올때 br.close()를 자동으로 호출한다.

#### 그러면 어떤 클래스를 try 조건문 안에 넣을 수 있을까?
정렬의 Comparable 인터페이스와 비슷하게 close 추상 메서드를 가진 AutoClosable 인터페이스를 구현한 클래스를 넣을 수 있다.

### try-with-resource의 실행 순서

try-with-resource의 실행 순서는 아래와 같다.

> 1. try 구문을 실행하다 예외를 만난다.
> 2. catch로 넘기기 전 try() 조건문 내부에 생성된 객체의 close()를 호출한다.
> 3.예외 처리 구문으로 넘어간다.