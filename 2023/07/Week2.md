# Week 2 (2023-07-10 ~ 2023-07-16)

## Sping MVC2 - 메시지, 국제화

- `messageSource` 에서 직접 메시지를 꺼낼 때에는 다음과 같은 메소드를 이용한다.
```
ms.getMessage(codeName, args, defaultMessage, locale);
```

- 인자를 사용할 때에는 Object 배열 형식으로 전달한다.
```
ms.getMessage("hello", new Object[]{"Spring"}, "defaultMessage", Locale.KOREA);
```

- 타임리프에서는 `#{message.code}` 같은 형식으로 메시지를 불러와 적용할 수 있다.

- 스프링 메시지 국제화는 클라이언트의 `Accept-Language` 헤더를 통해 Locale 을 분기한다.

- 사용자가 클라이언트 설정을 통하지 않고도 Locale 을 변경할 수 있도록 스프링에서는 `LocaleResolver` 라는 옵션을 제공한다.


## Spring MVC2 - Validation1 복습
- Spring 의 도움을 받지 않는 순수 Validation(V1)
    - field 의 값을 비지니스 로직 내에서 직접 체크하여 error 객체를 임의로 만들어 반환한다.
    - 반환받은 error 객체의 유무에 따라 해당 key 의 field 에서 메시지를 출력한다.

    - Safe Navigation Operation
        - Thymeleaf 내에서 NPE 에서 자유롭게 함수를 호출할 수 있도록 해주는 문법
        ```html
        <!-- errors 객체가 Null 이여도 NPE 가 발생하지 않고 결과 값으로 Null 이 반환된다.-->
        <div th:if="${errors?.containsKey('globalError')}">
            <p class="field-error" th:text="${errors['globalError']}">전체 오류 메시지</p>
        </div>
        ```
    - 문제점
        - 타입 체크가 불가능 (타입 불일치 시 Controller 이전에 이미 에러가 발생하므로)
        - 고객이 입력한 값이 Validation 에서 통과 안될 시 저장이 불가능

- 스프링을 사용한 검증 (V2)
    - `BindingResult`
        - 스프링이 Validation 에 실패하면 오류를 보관하는 객체이다.
        - 해당 객체가 파라미터에 존재하면 ModelAttribute 데이터 바인딩 중 오류가 발생해도 컨트롤러가 호출된다.
        - 검증할 대상 바로 뒤에 와야한다.
    - `FieldError` 객체는 validation 실패 시 사용자의 입력값을 보관한다.

## Spring MVC2 - Validation
- 검증 오류코드는 개발자가 직접 줄 수도 있고, 타입 불일치 등은 스프링이 직접 생성해준다. (ex: `typeMismatch.java.lang.Integer`)
- 스프링이 생성한 오류 코드도 앞서 `MessageCodesResolver` 를 활용한 전략이 사용 가능하다. (`typeMismatch` -> `typeMismatch.java.lang.Integer`)
- 복잡한 검증로직을 별도 모듈로 분리하기 위해서는 스프링이 제공하는 `Validator` 인터페이스를 상속받아 사용한다.
    - 해당 인터페이스는 검증이 가능한지를 판단하는 `supports` 메소드와 검증을 진행하는 `validate` 메소드를 사용한다.
        - 참고
        ```
        Item.class.isAssignableFrom(clazz) 
        //item == clazz
        //item == subItem
        ```

- `WebDataBinder` 를 사용하면 파라미터의 바인딩과 검증기를 편리하게 함께 사용할 수 있다.
- `@Validated` 애노테이션을 붙이면 검증기가 실행된다.
    - 검증기 적합성 여부는 검증기 내부의 `supports` 메소드로 먼저 확인한다.
- Spring Main 실행 지점에서 글로벌한 Validator 를 제공할 수 있다.