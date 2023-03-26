# Week 3 (2023-03-20 ~ 2023-03-26)
## 정보통신망 3강 - 데이터 통신 요소
### 통신선로
- 점대점 선로
- 멀티드롭 선로
- 집선 선로

### 전송 매체
- 하드 와이어 (유선)
    - 꼬임선
    - 동축 케이블
    - 광섬유 케이블
- 소프트 와이어 (무선)
    - 마이크로파, 라디오파

### 네트워크 형태
- 링형
- 스타형
- 버스형
- 그물형
- 계층형 (tree)

### 네트워크 장치 (아래로 갈 수록 응용계층)
- 리피터 (신호증폭)
- 허브
- 브리지 (물리주소 사용)
- 라우터 (IP 주소 사용)
- 게이트웨이 (프로토콜 전환)

---

## 컴퓨터보안 3강- 인증
### 개념
- 어떤 실체가 그 실체가 정말 맞는지 확인하는 과정
- 예시
    - A가 B에게 메시지를 보냄 (A가 맞는지?)
    - 회원이 장비에 접속하려는 경우 (회원이 맞는지?)
### 메시지 인증
- 메시지가 변조되지 않고 원본이 잘 왔는지 확인
- Message Authentication Code (MAC) 
    메시지 인증을 위한 부가적인 정보
- 과정
    - 송신자가 메시지 원문과 메시지 원문을 알고리즘으로 MAC 를 만들어서 함께 전 송
    - 수신자는 송신자가 보낸 MAC 과 자신이 받은 메시지 원문을 MAC 으로 만든 것이 일치하는지 확인
- MAC 특징
    - 비밀키를 이용
    - 기밀성은 제공안함 (변조는 안되도 원문 내용을 훔쳐볼수는 있음)
    - 작은 크기 (메시지 크기와 무관함)

- 종류
    - HMAC : 비밀키를 메시지에 붙이고 전체를 해시함수에 태움
    - CMAC

### 사용자 인증
- 시스템에 접근하려는 사용자에 대한 인증
- 방식
    - 비밀번호 방식
    - 토큰 방식
    - 2단계 인증(2FA Two-Factor Authentication)

---

## Spring MVC2 (김영한)
### 리터럴
- 문자: `'hello'` (공백이 없으면 작은 따옴표 생략 가능)
- 숫자: `10`
- 불린: `true`, `false`
- null: `null`

### 연산
- Elvis 연산자
    ```html
    ${data}?: '데이터가 없습니다.' = Spring!
    ${nullData}?: '데이터가 없습니다.' = 데이터가 없습니다.
    ```
- No-Operation
    ```html
    ${data}?: _ = Spring!
    ${nullData}?: _ = 데이터가 없습니다.
    ```

### Safe Navigation Operator
- thymeleaf 객체 사용 시 NPE 가 발생할만한 지점에 `.` 연산자를 붙여 NPE 대신 null 을 발생시킨다.

```html
th:class="${errors?.containsKey('price')} ? 'form-control field-error' : 'form-control'"
```

### Validation
- `BindingResult` 객체 파라미터는 `@ModelAttribute` 객체 바로 다음에 와야한다. 
- Thymeleaf 의 스프링 검증 오류 통합 기능
    - `#field` : `BindingResult` 객체의 검증 오류에 바로 접근
    - `th:errors`: 해당 필드에 오류가 있는경우 태그 출력

- `@ModelAttribute` 에 바인딩 시 타입 오류가 발생한다면
    - `BindingResult` 가 있으면 400 에러 발생 
        - 컨트롤러는 호출하지 안고 에러페이지로 이동
    - `BindingResult` 가 있는 경우 에러 정보를 `BindingResult` 에 담음
        - 컨트롤러도 정상 호출

- `BindingResult` 와 `Errors` 모두 인터페이스 이다.
    - 실제 구현체는 `BeanPropertyBindingResult` 이다.
    

---

## Jenkins를 이용한 CI/CD Pipeline 구축 (Dowon)
### Waterfall VS Agile
- 폭포수 : 지나치게 계획에 의존적
- 애자일 : 계획과 변화의 타협을 가져감
- 클라우드 네이티브 아키텍처의 구성요소로는 데브옵스, 마이크로서비스, 컨테이너 가상화, 클라우드 환경 등이 있음


