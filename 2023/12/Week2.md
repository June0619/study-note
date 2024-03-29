# Week 1 (2023-12-11 ~ 2023-12-17)

## 스프링 핵심 원리 - 고급편

### 프록시 패턴 소개
클라이언트가 요청을 하는 측, 서버가 요청을 받는 측이라고 한다면 프록시는 중간에서 클라이언트의 **요청을 대신 서버측으로 전달**하는 역할을 한다.

프록시 개념을 이용하는 예시는 다음과 같다.

- 엄마에게 라면을 사다달라고 부탁했는데 라면이 이미 집에 있다고 한다. 따라서 더 기대보다 빨리 라면을 받을 수 있다. (접근제어, 캐싱)
- 아버지께 주유를 부탁했는데 세차까지 해주셨다. (부가기능)
- 엄마에게 라면을 사다달라고 부탁했는데 엄마도 동생에게 부탁을 전달했다. (프록시 체인)

- 프록시의 주요 기능
    - 접근 제어
        - 권한에 따른 접근 차단
        - 캐싱
        - 지연 로딩
'    - 부가 기능 추가

- GOF 디자인 패턴
    - 프록시 패턴 : 접근 제어 목적
    - 데코레이터 패턴 : 새로운 기능 추가 목적
    - `프록시 패턴` 과 `프록시` 객체는 정확하게 의미하는바가 다르므로 구분해서 알고있을 것



