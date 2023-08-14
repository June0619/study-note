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