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

- `ExceptionHandlerExceptionResolver` 
    - `@ExceptionHanlder` 애노테이션에 담긴 예외를 처리하여 정상응답 처리한다.
        - 이 때, `@ResponseStatus` 애노테이션이나 `ResponseEntity` 객체를 리턴함으로써 Http Status 를 변경할 수 있다.
    - `@ExceptionHanlder` 애노테이션에는 한번에 여러 예외를 처리하거나, 메소드의 파라미터와 동일한 예외를 다룰 시 생략할 수 있다.
        ```java
        @ExceptionHandler({AExcption.class, BException.class})
        public ErrorResult(Exception e) {
            // ...
        }

        @ExceptionHandler
        public ErrorResult(IllegalArgumentException e) {
            // ...
        }
        ```
    - 포괄적인 예외인 `Exception` 객체부터 아주 작은 예외까지 모두 다룰 수 있지만, 우선순위는 자식 예외 객체부터 부모 객체순으로 올라간다.

    - 순서 정리 🚕
    > 예외 발생 (IllegalArgumentException) -> 컨트롤러 밖으로 예외 전파 -> `ExceptionResolver` 작동 -> `ExceptionHandlerExceptionResolver` 작동 -> `@ExceptionHandler` 애노테이션 작동 -> 명시된 응답 객체 반환 -> `@ResponseStatus` 애노테이션에 붙은 코드 반환

- `@ControllerAdvice`
    - 컨트롤러 내에 예외처리 기능들이 있으면 중복 예외 처리 시 다른 컨트롤러에 같은 기능을 만들어야 하고, 보기에도 깔끔하지 않다.
    - 따라서 컨트롤러 들이 하는 예외 처리를 공통으로 분리해낼 수 있도록 스프링이 제공하는 기능이다.
    - `@RestControllerAdvice` 는 `@ControllerAdvice` 애노테이션에 `@ResponseBody` 가 추가된 것이다.

    - 대상 컨트롤러를 지정하는 방법들
    ```java
    @ControllerAdvice(annotations = RestController.class)
    public class ExampleAdvice1 {}

    @ControllerAdvice("org.example.controllers")
    public class ExampleAdvice2 {}

    @ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})
    public class ExampleAdvice3 {}
    ```

