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

## 주말 과제
### 문제
- 일반적인 ModelAndView 패턴의 컨트롤러를 사용하는 경우 BindingResult 객체에 담긴 에러를 Thymeleaf 템플릿 엔진에서 메시지로 변환해주는 계층이 존재할 것이다. (리소스 번들 -> 애노테이션 메시지 -> 디폴트 메시지 순으로 조회)
    - 예시: `NotBlank.MemberDto.Name` -> `NotBlank.MemberDto` -> `NotBlank` 순으로 각 리소스들을 조회하여 메시지로 변환

- 하지만 BindingResult 객체를 직접 `MethodArgumentNotValidException` 예외 시 사용하는 경우 (API 응답 등) 메시지를 직접 정제해야 하는데 이때 타임리프 등 템플릿 엔진등에서 사용하는 것 처럼 변환하는 로직이 필요하다.

- 이를 구현하여 API 의 응답으로 Validation 예외를 반환하여도 메시지를 깔끔하게 생성하여 반환하는 코드를 구현해보자.

### 구현 
- MemberDto.class
```java
@Data
    @AllArgsConstructor
    static class MemberDto {
        private String memberId;
        @NotBlank //message.properties 에 해당 code 에 대한 메시지 정의 : {0} 필드에는 공백을 입력할 수 없습니다.
        private String name;
    }
```

- RestController
```java
    @PostMapping("/api/member")
    public String insertMember(@RequestBody @Validated MemberDto memberDto) {
        //Insert Member
        return "OK";
    }
```

- RestControllerAdvice
```java
@ExceptionHandler(MethodArgumentNotValidException.class)
    public ErrorResult handleValidationExceptions(MethodArgumentNotValidException ex){
        String message = ex.getAllErrors()
                .stream()
                .map(this::getErrorMessage)
                //Validation에 실패한 필드가 여러개일 경우를 위한 처리
                //list 로 collect 해도 상관없음
                .collect(Collectors.joining("\n"));

        log.error("[exceptionHandler] ex", ex);

        return new ErrorResult("Validation", message);
    }

    private String getErrorMessage(ObjectError error) {
        String[] codes = error.getCodes();
        for (String code : codes) {
            try {
                return messageSource.getMessage(code, error.getArguments(), null);
            } catch (NoSuchMessageException e) {
                continue;
            }
        }
        return error.getDefaultMessage();
    }
```

## Spring MVC2 - TypeConverter
### 스프링 타입 컨버터
- 스프링에서 `@RequestParam`, `@ModelAttribute`, `@PathVariable` 등 파라미터를 직접 받아낼 수 있는 여러 애노테이션이 존재한다.
- 기본은 request 객체에서 꺼낼 때 String 타입으로 넘어오지만, Integer 및 Boolean 등 여러 타입 변환도 지원한다.
- 이 때 이러한 타입 변환을 확장할 수 있는것이 바로 스프링의 Converter 인터페이스이다.
    - 과거에는 `PropertyEditor` 라는 것을 이용했지만 동시성 이슈가 있어서 현재는 잘 사용되지 않는다.

### ConversionService
- 이러한 타입 컨버터들을 하나하나 직접 주입받아 사용하는 것은 너무 불편하기 때문에 등록과 사용을 편리하게 관리해주는 인터페이스이다.
- `DefaultConversionService` 라는 구현체를 Spring Bean 으로 별도 관리하여 사용하면 된다.
- 위의 구현체는 컨버터를 등록하는 `ConverterRegistry` 인터페이스와 사용하는 `ConversionService` 인터페이스를 모두 구현하는데 이처럼 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않도록 하는 원칙을 **ISP(Interface Segregation Principle)** 원칙이라 한다.
- [참고 링크](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4_%EB%B6%84%EB%A6%AC_%EC%9B%90%EC%B9%99)