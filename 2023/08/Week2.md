# Week 2 (2023-08-07 ~ 2023-08-13)

## Spring MVC2 - API 예외 처리

### 스프링이 제공하는 ExceptionResolver 
- 스프링 부트 기본 제공 `ExceptionResolver`
    1. `ExceptionHandlerExceptionResolver`
    2. `ResponseStatusExceptionResolver`
    3. `DefaultHandlerExceptionResolver`

- `ResponseStatusExceptionResolver`
    - `@ResponseStatus(code=404, reason="message")` 가 붙은 Exception (직접 만듬)
    - 예외의 인스턴스가 `ResponseStatusException` 인 경우 (이미 만들어져있는 예외를 감싸서 사용)


- `DefaultHandlerExceptionResolver`
    - 스프링 내부에서 발생하는 예외들을 처리하기 위한 ExceptionResolver
    - `TypeMismatchException`, `HttpRequestMethodNotSupportedException` 등 을 다룬다.