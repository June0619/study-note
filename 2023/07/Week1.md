# Week 1 (2023-07-03 ~ 2023-07-09)

## Sping MVC2 - 스프링 통합과 폼
- `spring-boot-starter-thymeleaf` 의존성 사용 시 Thymeleaf ViewResolver 와 관련된 상세한 설정들을 자동화 해준다.

- `th:field` 를 통해 id, name, value 등 여러 태그를 대체할 수 있다.

### 체크박스
- Form 에서 체크 된 체크박스의 값은 on 으로 넘어가는데 이를 스프링이 Boolean 타입의 True 로 변환해준다. (Spring Type Converter)
- 체크박스의 경우 체크가 되지 않으면 false 가 아니고 null 이 온다.
- Spring MVC 에서 이를 해결하기 위한 약간의 트릭을 사용한다.
    - name 필드에 _name 형태의 hidden input 을 같이 보낸다.
    - 이 경우 체크박스에 값이 오지 않아도 null 이 아닌 false 로 인식한다.
