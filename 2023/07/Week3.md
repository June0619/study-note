# Week 3 (2023-07-17 ~ 2023-07-23)

## Sping MVC2 - Bean Validation
### Introduce
    - 일반적으로 자주 사용하는 검증을 애노테이션을 이용하도록 공통화, 표준화 한 것
    - `Bean Validation 2.0 (JSR-380) 자바 표준 기술 - 구현체는 Hibernate Validator`
    - `javax.validation` 구현체에 관계없는 표준 인터페이스
    - `org.hibernate.validator` 하이버네이트 validator 에서만 동작 (대부분 hibernate 사용하므로 크게 신경 안써도 된다.) 

### Bean Validation
    - ModelAttribute 에 들어온 값이 우선 바인딩이 되어야 (타입 일치) Bean Validation 적용
        - 타입 일치하지 않을 시 `typeMismatch` 로 `FieldError` 추가
    - Bean Validation 에서 에러 메시지를 찾는 순서
        1. 레벨 순서대로 `messageSource`
        2. 애노테이션의 `message` 필드 값
        3. 라이브러리의 기본 값 -> '공백일 수 없습니다.'
    
    - ObjectError 의 경우 `@ScriptAssert` 라는 애노테이션을 이용할 수 있는데 너무 복잡하므로 그냥 객체단위로 관리하는 것을 권장한다.

    - 한계
        - 하나의 모델에 대해 등록/수정 과 같이 별도의 검증을 적용해야 하는 경우에 제한적이다.
        
    - 위 문제를 극복하기 위해 Groups 기능을 사용할 수 있다. 
        - Interface 를 파라미터로 Validation 을 적용받을 그룹을 분리
        - 코드가 너무 복잡해져서 권장되지는 않음

    - Groups 기능 대신 등록/수정 등 상황에 맞추어 Model 을 분리하는 것이 Best 이다.

    - API 의 Vadliation 세 가지 케이스
        - 성공하는 경우 (Validation 통과)
        - 바인딩에 실패하는 경우 (JSON 파싱 자체가 실패하는 경우 - 타입 불일치) : 컨트롤러 호출 자체가 X
        - Vadliation 실패 : 컨트롤러는 호출 됨

    - `@ModelAttribute` 는 필드 단위로 정교하게 바인딩이 된다. 
        - 한 가지 필드에서 타입 불일치해도 나머지 필드는 생성되어 바인딩 된다.
    - `@RequestBody` 는 `HttpMessageConverter` 단계에서 JSON 데이터를 객체로 변경하지 못하면 예외 발생.
        - 컨트롤러도 호출되지 않음

### Login
    - 좋은 설계
        - **Domain** 이 가장 중요하며 Web이나 App, API 등 다양한 계층이 와도 대응할 수 있도록 설계해야 한다.
        
