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

## GoF 디자인 패턴 - 생성 패턴
### 싱글톤 패턴
- 싱글톤 패턴 기본 코드 예시
```java
public class Settings {

    private static Settings instance;

    //기본 생성자 호출 접근 제어 (new 연산자 사용을 막기 위해)
    private Settings() {}

    public static Settings getInstance() {
        if (instance == null) {
            instance = new Settings();
        }

        return instance;
    }
}
```
- 싱글톤 패턴의 문제점
    - Thread Safe 하지 않음
    - 고려할 수 있는 해결 방법
        - `synchronized` 키워드를 통해 방지 가능하지만 Lock 을 사용하는 매커니즘이므로 성능 저하가 발생할 수 있음
        - 클래스 생성 시점에 미리 생성해 둠 (사용하지 않을 시 불필요한 메모리 낭비 발생)
        - double checked locking 사용 (불필요하게 복잡함)
        - static inner 클래스 사용 
    
     

