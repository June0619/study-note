# Week 1 (23-01-06 ~ 23-01-15)

## Spring MVC
### RequestMapping
- RequestMappingHandlerMapping 이 요청 URL 을 모으는 원리
  - `@RequestMapping` 혹은 `@Controller` 애노테이션 기반으로 매핑 정보를 수집한다.

### HandlerMapping
- 자주 쓰는 핸들러 매핑
  1. `RequestMappingHandlerMapping` (애노테이션 기반 `@RequestMapping`)
  2. `BeanNameUrlHandlerMapping` (스프링 빈 이름 기반)

### Handler Adapter
- 자주 쓰는 핸들러 어댑터
  1. `RequestMappingHandlerAdapter` 
  2. `HttpRequestHandlerAdapter` 
  3. `SimpleControllerHandlerAdapter`

