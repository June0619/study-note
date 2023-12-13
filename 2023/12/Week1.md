# Week 1 (2023-12-04 ~ 2023-12-10)

## 스프링 핵심 원리 - 고급편
### 템플릿 메소드 패턴

- `@RequestMapping`: 스프링 MVC는 `@Controller` 혹은 `@RequestMapping` 애노테이션이 있어야 컨트롤러로 인식한다. 이 애노테이션은 애노테이션에 사용해도 된다.

- `@ResponseBody`: HTTP 메시지 컨버터를 사용해서 응답한다. 이 애노테이션은 인터페이스에 사용해도 된다.

- `@Import(AppV1Config.class)`: 클래스를 스프링 빈으로 등록한다. 일반적으로 설정파일을 등록할 떄 사용하지만, 스프링 빈을 등록할때도 사용할 수 있다.