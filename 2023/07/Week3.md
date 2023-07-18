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