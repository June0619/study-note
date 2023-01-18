# Week 2 (2023-01-16 ~ 2023-01-22)

## Spring MVC
### ArgumentResolver 와 ReturnValueHandler
- `@RequestMapping` 애노테이션을 처리하는 `RequestMappingHandlerAdapter` 에서 파라미터, 애노테이션 정보를 기반으로 컨트롤러에 전달할 데이터를 가공한다.
- 파라미터를 가공하는 과정에서 ArgumentResolver를 호출하며 파라미터 뿐만 아니라 엔티티까지 처리 가능하다.
- 응답에 대한 처리도 `ArgumentResolver` 와 비슷하게 `ReturnValueHandler` 에서 처리한다.
- 두 객체 모두 HTTP 메시지에 대한 직접적인 처리가 필요한 경우 `HttpMessageConverter` 를 사용한다.
- 요청 및 응답에 대해 스프링에서 정의 된 형태 외의 추가적인 처리 과정을 등록하고 싶다면 `WebMvcConfigurer` 를 통해 직접 등록할 수도 있다.

### ModelAttribute 애노테이션
- 보통 화면에서 받은 객체가 뷰에서 바로 사용되므로 `@ModelAttribute` 애노테이션의 밸류 값으로 저장된다.
    - 뷰로 나가기 위한 별도 저장 코드가 불필요 하다.

### Thymeleaf
- Thymeleaf 사용 선언
```html
<html xmlns:th="http://www.thymeleaf.org">
```

- 순수한 HTML을 사용하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 내츄럴 템플릿이라 한다.

#### th 태그
- 서버 렌더링 시에만 적용되는 태그
- 단독 html 실행시 th가 붙지 않은 기존 마크업 태그들이 적용 된다

#### URL 링크 표현식
```
<!-- CSS 링크 예제 -->
<th:href="@{/css/bootstrap.min.css}">
```

#### 리터럴 대체 문법
- 타임리프에서 문자와 표현식을 `+` 연산자로 연결해야 하지만, 리터럴 대체 문법을 통해 다음과 같이 간단하게 표현 가능하다

```html
<!-- 기존 표현 방식 -->
<span th:text="'Hello ' + ${user.name} + '!'"> 

<!-- 리터럴 대체 문법 적용-->
<span th:text="|Hello ${user.name}!|">
```

- 경로 변수 및 파라미터 간편하게 생성도 가능하다
```html
<span th:onclick="|location.href='@{/member/{memberId}(memberId=${member.id}, query='test'}|">
```
    - 생성 링크 : http://localhost:8080/member/1?query=test

### PRG POST/REDIRECT/GET
#### HTML 폼을 이용해 저장 후 새로고침을 하는 시나리오
1. GET /item
2. POST /item
3. POST /item
4. POST 요청 반복

- 브라우저 입장에서는 마지막 요청을 반복하기 때문에 POST 요청이 반복 된다.

#### POST, Redirect GET
- 위 상황을 해결하기 위해서는 간단하게 Redirect 를 통해 마지막 요청을 GET 으로 변경해주면 된다.

#### RedirectAttribute
- Redirect 되었을 경우 필요한 값들을 컨트롤러에게 전달해주기 위해 `RedirectAttribute` 모델을 사용할 수 있다. 해당 모델은 `Model` 객체를 상속받으며 리다이렉트 시에만 전달받은 값이 존재한다.

- `RedirectAttribute` 에 저장된 데이터 중 PathVariable 에 존재하는 변수명은 대체되고, 나머지는 쿼리 파라미터로 사용된다.

## GoF 디자인 패턴

### 싱글톤 패턴
- 싱글톤 패턴을 깨는 방법
    - 리플렉션을 사용
    - 직렬화/역직렬화 사용 (대응이 가능)
    - enum 을 사용하는 방법

#### enum 클래스로 생성하는 방법
- 장점
    - enum 클래스는 리플렉션이 불가능하다.
    - 직렬화 후 역직렬화 해도 싱글톤이 보장된다.
- 단점
    - 처음에 초기화 된다 (지연 생성이 불가)
    - 상속을 쓸수가 없다

#### 실제 싱글톤을 사용하고 있는 코드

- `Runtime`
- Spring 의 싱글톤 Scope (엄밀히 말하면 싱글톤 패턴은 아님)
