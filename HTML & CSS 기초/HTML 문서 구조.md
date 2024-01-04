---
Date: 2024-01-04
reference: https://www.notion.so/oreumi/HTML-284e18749cae453b953fcd9106f4d165
Title: HTML 템플릿 내부 태그들 해부!
---
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>

```

> 💡  기본적인 HTML 구조는 위와 같은데, 각각의 태그들이 어떤 의미를 갖는지, 어떤 역할을 수행하는지 알아보자.

## 1.  \<!DOCTYPE html\>
- DTD (Document Type Definition)이라 부르며 문서의 타입에 대한 정보를 제공함.
- 태그가 적힌 html 파일이 [HTML Living Standard](https://html.spec.whatwg.org/)를 따르는 문서 (html5는 이제 죽었어!)
 - 누락되더라도 브라우저가 기본적으로 처리함!

## 2.  \<html lang=en\>
- \<html\> 문서의 루트, 최상단의 요소입니다.
- lang 속성에 붙은 en, ko-KR등은 스크린리더가 음성 표현에 사용할 언어 선택에 도움을 줌
- 검색엔진에서 홈페이지의 주 lang 속성으로 판단함.


## 3.  HEAD
- 문서의 메타데이터를 담기는 곳
- 사용자에게 보여줄 title, favicon, viewport, css 파일을 불러오는 등의 작업을 수행함.

### meta
- 메타데이터: "어떤 목적을 위해 만들어진 데이터"
- 브라우저와 검색엔진은 메타 정보를 통해 필요한 정보를 취합하고 사용합니다.

대표적인 meta 속성

**charset**
```html
<meta charset=""utf-8">
```
html에서 사용하는 문자 코드의 종류를 설정합니다.

**http-equiv="X-UA-Compatible" content="IE=edge"**
```html
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
```
- 사실 최신 브라우저를 사용할 땐 필요하지 않으나, MS에 여러 IE버전(6,7,8,9,10,..edge)중 어느 브라우저의 호환성을 따를것인지 명시하는 속성

**viewport**
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```
-  뷰포트 즉 문서가 보여질 포트의 속성을 명시합니다. width는 뷰포트의 크기, initial-scale은 확대 정도를 지정합니다.

## 4. title
- 페이지 탭에 보이는 문서 제목을 정의
- ==60자를 넘으면 안됨!==

## 5. link
- 현재 문서와 외부 리소스의 관계를 명시
- 외부 스타일 시트(css), 폰트, 파비콘, javascript 파일을 연결할때 사용

## 6. body
사용자에게 직접적으로 보여지는 영역




div와 섹션의 차이

구획을 나누나, 섹션은 의미있는 구획을 나눌때 사용한다.

