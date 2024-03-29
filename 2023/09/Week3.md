# Week 3 (2023-09-18 ~ 2023-09-24)

## Spring DB 1편 - 스프링 트랜잭션

### 스프링 트랜잭션 AOP
- 앞서 살펴보았던 트랜잭션 템플릿 또한 트랜잭션 관련 소스가 서비스 레이어로 올라온다.
- 이러한 문제를 해결하기 위해 `@Transactional` 애노테이션을 이용하여 트랜잭션을 적용할 수 있다.
- `@Transactional` 애노테이션 하나만을 통해 트랜잭션을 관리 하는것이 선언적 트랜잭션 관리라고 한다.
- 트랜잭션 매니저나 트랜잭션 템플릿을 사용하여 트랜잭션 코드를 직접 작성하는 것을 프로그래밍 방식의 트랜잭션 관리라고 한다.
- 대부분 선언적 트랜잭션 관리를 사용하고 가끔 테스트를 위해서만 프로그래밍 방식 트랜잭션 관리를 사용한다.