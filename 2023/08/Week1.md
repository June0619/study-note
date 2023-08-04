# Week 1 (2023-07-31 ~ 2023-08-06)

## Sping MVC2 - 예외 처리와 오류 페이지

- 서블릿의 예외처리 방식 두가지
    1. `Exception` - 예외
    2. `response.sendError` (HTTP Status, 오류 메시지)

### Exception (예외)
- Java
    - main 쓰레드를 넘어 전파되면 예외 정보를 남기고 쓰레드가 종료된다.

- Web Application
    - 사용자 요청별로 쓰레드가 할당되는데, 각각 서블릿 컨테이너 안에서 실행된다.
    - 서블릿 밖으로 예외가 전파 되면 톰캣 같은 WAS 까지 전파 된다.
    - 이 경우 톰캣이 자체적으로 `500` 이나 `404` 같은 StatusCode 와 에러 페이지를 출력한다.

### Servlet Exeception
- Error 발생 시 WAS 까지 가서 등록된 Error 페이지를 컨트롤러까지 전부 호출한다. (White Label Page도 동일할까?)

> 🚗 오류 페이지 요청 흐름 </br>
> 컨트롤러 (예외 발생) -> 인터셉터 -> 서블릿 -> 필터 -> WAS -> (에러페이지 요청) -> <br/> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(error-page) -> View

- 정리
    1. 에외가 발생해서 WAS 까지 전파
    2. WAS 에서 오류 페이지 경로를 찾아 오류페이지를 호출 (**필터, 서블릿, 인터셉터, 컨트롤러 모두 다시 호출**)
    3. 클라이언트는 서버 내부에서 호출이 다시 일어나는지 **전혀 알지 못함**

- 오류가 발생했을 때 필터 및 인터셉터를 재 호출하는 것은 비효율적이고 위험할 수 있다.
    - 필터에서는 DispatcherType 을 기준으로 필터 호출 여부를 결정할 수 있다. (Default: REQUEST)
    - 인터셉터에서는 에러 페이지의 Path 를 제외시켜줌으로써 에러페이지의 인터셉터 호출을 회피할 수 있다.

### Spring Boot Exception
- 예외 처리를 위해 앞선 과정에서는 `WebServletCustomizer`를 만들고 예외에 따라 컨트롤러 및 페이지를 추가했다.
- 스프링 부트에서는 `ErrorPage` 객체 및 `/error` 경로의 기본 오류 페이지를 제공한다.

- `BasicErrorController` 에서는 오류 정보를 모델에 담지 않는다.
    - `server.error.include-` 옵션들을 통해 에러 메시지를 모델에 전달할 수 있지만, 보안상의 이유로는 사용하지 않는다.

- 몇 가지 설정들
    - `server.error.whitelabel.enabled=true` : 오류 처리하는 화면이 없을 때 스프링의 기본 whitelabel 오류 페이지 제공 여부
    - `server.error.path=/error` : `BasicErrorController` 에서 기본으로 사용하는 오류 페이지 경로 

### API 에외처리
- 기존의 예외처리에서는 에러 발생 시 HTML 문서를 전달해준다. 하지만 API 통신 중인 클라이언트는 JSON 응답을 기대하고 있다.
- ExceptionController 측에서 `Content-Type: Application/JSON` 인 경우에 JSON 응답을 주도록 설정 할 수 있다.