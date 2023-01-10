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


### Logging
- Logback, Log4J, Log4J2 등 수많은 로그 라이브러리가 있고, 그것들을 통합하여 인터페이스로 제공하는 것이 SLF4J 라이브러리이다.
- 부트에서는 기본으로 Logback 을 제공한다.
- 로그 레벨은 Trace > Debug > Info > Warn > Error (기본 Info)
    - 보통 개발서버에서는 Debug 레벨, 운영 서버에서는 Info 레벨을 채용한다.

- log 사용 시 문자열 + 연산을 사용하면 안되는 이유
    - 실제 출력하지 않는 레벨의 로그도 문자열 `+` 연산을 사용하면 리소스를 사용한다.

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
    
     

## 알고리즘
### 정수론
- 정수 N이 합성수 일 경우 N^(1/2) 까지의 정수 중 반드시 약수가 존재한다.
    - N이 소수일 경우 N^(1/2) 까지 약수가 존재하지 않는다.

- 1부터 루트N 까지 소수를 찾는 코드 (Math.sqrt 사용 안하는게 나음)
```java
for (int i = 1; i*i < n; i++) {
    isPrime(i);
}
```
- 에라토스테네스의 체를 적용하면 시간복잡도를 더욱 줄일 수 있다.

### 정렬
- 시간복잡도 O(N^2) 을 가지는 기본적인 정렬
    - 선택 정렬 
    - 삽입 정렬 
    - 버블 정렬
- 머지 정렬 - 시간복잡도 O(nlogN)
    - 쪼개고 합치기 합치기

