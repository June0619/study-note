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
```java
//선언
//클래스 레벨에 @Slf4j 애노테이션으로 대체 가능
private final Logger log = LoggerFactory.getLogger(getClass());
```
- Logback, Log4J, Log4J2 등 수많은 로그 라이브러리가 있고, 그것들을 통합하여 인터페이스로 제공하는 것이 SLF4J 라이브러리이다.
- 부트에서는 기본으로 Logback 을 제공한다.
- 로그 레벨은 Trace > Debug > Info > Warn > Error (기본 Info)
    - 보통 개발서버에서는 Debug 레벨, 운영 서버에서는 Info 레벨을 채용한다.

- log 사용 시 문자열 + 연산을 사용하면 안되는 이유
    - 실제 출력하지 않는 레벨의 로그도 문자열 `+` 연산을 사용하면 리소스를 사용한다.

### 기본 헤더 조회
```java
@RequestMapping("/headers")
public String headers(
    //모든 헤더 정보를 한번에 조회하고 싶을 때
    @RequestHeader MultiValueMap<String, String> allHeaders,
    //특정 헤더를 지정해서 받고 싶을 때
    @RequestHeader("host") String host) {
        // Controller Source ...
    }
```
- 애노테이션 기반 컨트롤러에선 위와 같이 헤더를 조회할 수 있다.
- `@RequestHeader` 애노테이션에 필수값 여부(`required`) 및 기본값 속성(`defaultValue`) 필드도 있다.


### 파라미터 조회
#### 애노테이션 기반 컨트롤러에서 파라미터를 받는 네가지 방법

1. request.getAttribute("username")
2. @RequestParam("username") String name 
3. @RequestParam String username //파라미터 이름과 변수 이름이 일치해야 한다.
4. String username //3번의 조건에서 `@RequestParam` 애노테이션까지 생략이 가능하다.
5. @RequestParam Map<String, Object> paramMap // map 으로 받아서 하나씩 꺼내쓰기

#### 필수 여부와 기본값
- `@RequestParam` 애노테이션에 밸류로 설정할 수 있는 필드들이다.
- 필수 여부 (`reqired`)
    - 원시 타입 파라미터 변수에는 `reqired` 여부와 관계없이 null 이 들어갈 수 없다. (500 erer)
    - 공백문자는 null 과 달리 required 를 만족한다. (username=)
- 기본 값 (`defaultValue`)
    - 해당 파라미터에 값이 없으면 기본 값으로 채워진다.
    - 필수 여부 필드를 무효화 시키는 특징이 있다. (어차피 기본값을 주므로)
    - 공백문자의 경우에도 설정한 기본값을 채워버린다. 

#### Param Model 로 받기
- `@ModelAttribute` 애노테이션을 이용하여 모델을 직접 파라미터로 받을 수 있다.
```java
@RequestMapping("/url")
public String modelAttribute(@ModelAttribute HelloModel model) {
    //Controller Source...
}
``` 
- model 내부의 setter 메서드를 호출하여 필드 값을 채우므로 setter 없으면 값을 채울 수 없다. (모델은 생성 됨)
- `@ModelAttribute` 애노테이션은 생략이 가능하다.

#### 애노테이션 생략 시 ArgumentResolver 동작
- `@RequestParam` 과 `@ModelAttribute` 모두 애노테이션이 생략 가능하다.
- 스프링의 ArgumentResolver 는 컨트롤러의 파라미터 애노테이션 생략 시 다음과 같은 기본 규칙을 따른다.
  1. 기본 타입(String, int, Integer 등) = `@RequestParam`
  2. 나머지 = `@ModelAttribute` = ArgumentResolver 로 지정해둔 타입 외

#### HTTP Request Message Body 직접 받는 네가지 방법
1. HttpServletRequest 객체에서 ServletInputStream 을 꺼낸 후 바디 내용을 추출한다.
2. 컨트롤러 파라미터로 InputStream 객체를 받아 위 과정을 생략한다.
3. HttpEntity 객체를 상속받은 RequestEntity 객체와 ResponseEntity 객체를 이용한다. (HttpEntity 써도 됨)
4. `@RequestBody` 애노테이션과 `@ResponseBody` 애노테이션을 통해 더 간단하게 할 수 있다.

- 주의사항: `@RequestBody` 를 통해 바디내용을 직접 받는것과 `@RequestParam` 을 통해 파라미터 내용을 직접 받는것은 다르다
- 3번이나 4번에서 바디 메시지를 문자열이나 객체로 변환하는 과정은 HttpMessageConverter 가 수행한다.




<br>

---

<br>

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
    
<br>

---

<br>

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
- 머지 정렬  O(nlogN)
    - 쪼개고 합치기 합치기
- 카운팅 정렬 - O(N)
    - 정렬해야 하는 자료 범위가 넓으면 메모리 낭비가 심해짐
