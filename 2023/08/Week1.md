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