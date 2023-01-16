# Week 2 (2023-01-16 ~ 2023-01-22)

## Spring MVC
### ArgumentResolver 와 ReturnValueHandler
- `@RequestMapping` 애노테이션을 처리하는 `RequestMappingHandlerAdapter` 에서 파라미터, 애노테이션 정보를 기반으로 컨트롤러에 전달할 데이터를 가공한다.
- 파라미터를 가공하는 과정에서 ArgumentResolver를 호출하며 파라미터 뿐만 아니라 엔티티까지 처리 가능하다.
- 응답에 대한 처리도 `ArgumentResolver` 와 비슷하게 `ReturnValueHandler` 에서 처리한다.
- 두 객체 모두 HTTP 메시지에 대한 직접적인 처리가 필요한 경우 `HttpMessageConverter` 를 사용한다.
- 요청 및 응답에 대해 스프링에서 정의 된 형태 외의 추가적인 처리 과정을 등록하고 싶다면 `WebMvcConfigurer` 를 통해 직접 등록할 수도 있다.