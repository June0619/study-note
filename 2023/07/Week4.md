# Week 4 (2023-07-24 ~ 2023-07-30)

## Spring MVC2 - Login
### 세션
- 로그인 시도 성공 시 서버에 세션 ID와 회원 정보를 저장한다.
- 이때 세션 ID 는 사용자가 유추 불가능한 문자열 (UUID 등) 을 사용한다.
- 사용자는 세션 ID 값만 쿠키로 저장한다. (사용자 정보를 전혀 전달받지 않음)
- 서버 측에서는 해당 세션을 30분 후 제거한다.

### HttpServlet Session
- static 한 String 등을 관리 할 때에는 new 연산자로 객체가 만들어지지 않도록 추상클래스나 인터페이스를 이용한다.
- `getSession(boolean)` 
    - `true` :요청 시 세션이 없으면 생성
    - `false` : 요청 시 세션이 없으면 null 반환

- REST API 통신에서도 위와 마찬가지로 Cookie 헤더를 통해 Session ID 를 전달할 수 있다.

- Spring에서는 `@SessionAttribute` 애노테이션을 통해 손쉽게 세션 체크 및 객체를 꺼낼 수 있다.
    - 참고로 해당 기능은 없는 세션을 생성해주지는 않는다.

- TrackingModes
    - 쿠키를 지원하지 않는 웹 브라우저를 위해 URL 에 세션 정보를 함께 전달하는 방식
    - 잘 사용되지는 않는다.

### 세션 타임아웃
- 세션은 메모리 상 비용을 차지하기 때문에 시간을 체크해서 만료시켜줘야 한다.
- `HttpSession` 객체는 사용자의 마지막 활동으로부터 (기본값) 30분 후 만료시킨다.
- 세션에 저장할 객체는 최소한의 정보만 유지시켜 주어야 한다.

## Spring MVC2 - Filter, Interceptor
- 모든 비지니스 로직에서 공통으로 체크해야 하는(ex: 로그인 여부) 등을 '공통 관심사' 라고 한다.
- 이러한 공통 관심사를 지원하는 기능은 '서블릿 필터', 스프링 인터셉터, 스프링 AOP 등이 있다.
- 이 중 웹과 관련된 정보 (HTTP 관련 정보가 포함되는) 를 처리할때는 필터나 인터셉터를 사용하는 것이 좋다.

### Servlet Filter
- 서블릿이 지원하는 수문장이다.

> ❗ 필터의 흐름 </br>
> HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 컨트롤러 </br></br>
> ❗ 필터 체인 </br>
> HTTP 요청 -> WAS -> 필터1 -> 필터2 -> 필터3 -> 서블릿 -> 컨트롤러

- 필터 인터페이스 구현 후 등록하면, 서블릿 컨테이너가 싱글톤 객체로 관리한다.

- Spring Boot 는 필터 등록 시 WAS 를 내장하고 있기 때문에 Bean 을 통해 등록이 가능하다.
    - `FilterRegistartionBean` 을 사용하면 된다.
        - `setFilter(new LogFilter())` - 등록할 필터 지정
        - `setOrder(1)` - 필터 순서 지정
        - `addUrlPattern("/*")` - URL 패턴 지정 (다수 지정 가능)
    - `@ServletComponentScan` `@WebFilter` 등으로도 등록 가능하지만 순서 조절이 안된다.

- [참고] Logback MDC

- 필터에서는 다음 필터로 `ServletRequest`, `ServletResponse` 객체를 바꿔치기 해서 넘길 수 있다.

### Spring Interceptor

- 스프링 인터셉터는 디스패처 서블릿콰 컨트롤러 사이에서 컨트롤러 직전에 호출된다.
- 스프링 MVC 의 시작점은 디스패처 서블릿이므로, 디스패처 서블릿 이후에 호출된다.

> ❗ 인터셉터의 흐름 </br>
> HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터 -> 컨트롤러 </br></br>

- 체인 형식으로 여러개를 추가할 수도 있다.
- 필터의 경우 doFilter 하나지만, 인터셉터는 컨트롤러 호출 전과 후로 기능이 세분화 되어있다.

- 인터셉터 호출 흐름
    - `preHandle`: 핸들러 어댑터 호출 전에 호출 됨
    - `postHandle`: 핸들러 어댑터 호출 후 호출 됨
    - `afterCompletion`: 뷰 렌더링 이후 호출 됨

- 스프링 인터셉터 예외 상황
    - 예외 발생 시 `DispatcherServlet` 에 예외가 전달되고, postHandle 은 호출되지 않는다.
    - `afterCompletion`은 예외와 무관하게 호출된다. (예외 정보도 가져옴)