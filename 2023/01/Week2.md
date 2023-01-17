# Week 2 (2023-01-16 ~ 2023-01-22)

## Spring MVC
### ArgumentResolver 와 ReturnValueHandler
- `@RequestMapping` 애노테이션을 처리하는 `RequestMappingHandlerAdapter` 에서 파라미터, 애노테이션 정보를 기반으로 컨트롤러에 전달할 데이터를 가공한다.
- 파라미터를 가공하는 과정에서 ArgumentResolver를 호출하며 파라미터 뿐만 아니라 엔티티까지 처리 가능하다.
- 응답에 대한 처리도 `ArgumentResolver` 와 비슷하게 `ReturnValueHandler` 에서 처리한다.
- 두 객체 모두 HTTP 메시지에 대한 직접적인 처리가 필요한 경우 `HttpMessageConverter` 를 사용한다.
- 요청 및 응답에 대해 스프링에서 정의 된 형태 외의 추가적인 처리 과정을 등록하고 싶다면 `WebMvcConfigurer` 를 통해 직접 등록할 수도 있다.

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
