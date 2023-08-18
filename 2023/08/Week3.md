# Week 3 (2023-08-04 ~ 2023-08-20)

## Spring MVC2 - 스프링 타입 컨버터

### 스프링에 Conveter 적용
- 스프링 내부의 `ConversionService` 에 내가 생성한 Converter 객체를 등록해주면 된다.
- 여타 WebConfigure 등록과 비슷하게 `WebMvcConfigurer` 객체를 상속받아서 `addFormatter` 메소드를 통해 등록 가능하다.
- 기본 타입 변환 컨버터가 있어도 사용자가 추가한 컨버터가 우선순위가 높아진다.
- 이렇게 등록한 Converter들은 `@ModelAttribute`, `@RequestParam`, `@PathVariable` 등 파라미터를 받을 때 자동으로 작동한다.

### View 템플릿에 적용
- 타임리프 문법으로는 값을 `${{}}` 와 같이 사용하면 컨버전 서비스를 사용한다.
- `th:field` 필드에는 자동으로 컨버전 서비스가 적용된다.

### Formatter
- `Converter` 는 범용적인 상황에서 쓰이지만 일반적으로는 문자->숫자 나 문자->문자 등 의 제한적인 상황이 대부분이다.
- 이를 위해 현지화 정보(`Locale`)를 추가해서 문자에 맞춘 출력을 적극적으로 지원하는 것이 바로 `Formatter` 이다.
- `Formatter` 인터페이스를 상속받아서 컨버터와 마찬가지로 `ConversionService`에 등록하여 사용하면 된다.
- `DefaultFormattingConversionService` 는 `FormattingConversionService` 에 기본적인 통화나 숫자등 기본 포멧터를 추가해서 제공한다.
- 스프링 부트는 `DefaultFormattingConversionService` 를 상속받는 `WebConversionService`를 사용한다.
- 컨버터와 포멧터를 겹치는 타입으로 두가지 등록하면 컨버터가 우선으로 동작한다.
- 애노테이션 기반으로도 간단하게 사용할 수 있다.

```java
@Data
static class Form {
    @NumberFormat(patter = "###,###")
    private Integer number;

    @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private LocalDateTime localDateTime;
}
```

### 컨버터 주의사항
- 메시지 컨버터(`HttpMessageConverter`) 에는 컨버전 서비스가 적용되지 않는다.
- JSON 객체 변환에 포멧팅을 사용하고 싶은 경우 Jackson 내부 포멧터를 사용해야 한다.

## Spring MVC2 - 파일 업로드
### 개요
- HTML 폼 전송 방식
    - `application/x-www-form-unlencoded` : 일반적인 폼 데이터 전송 방식
    - `multipart/form-data` : 파일등을 포함한 전송을 위한 방식

- `multipart/form-data` 전송 예제
```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: multipart/form-data; boundary=-------XXX (랜덤 값)
Content-length: 10123

-------XXX
Content-Disposition: form-data; name="name"

kim
-------XXX
Content-Disposition: form-data; name="file1"; filename="img.png"
Content-Type: image/png

10aefe01230ejfewiooiaefeewf....
-------XXX--
```

### 서블릿과 파일 업로드
- 스프링 파일 사이즈 제한 설정
    - `spring.servlet.multipart.max-file-size`
    - `spring.servlet.multipart.max-request-size`
- 스프링 멀티파티 관련 동작 온오프 설정
    - `spring.servlet.multipart.enabled`

- `DispatcherServlet` 에서 멀티파트 관련 요청이면 `MultipartResolver` 에 의해 `HttpServletRequest` 객체가 `MultipartHttpServletRequest` 객체로 변환된다.

## 이펙티브 자바 - 아이템1. 생성자 대신 정적 팩토리 메소드를 고려하라

### 장점
1. 메소드를 사용함으로써 생성의 구분이 명시적으로 나타난다.
