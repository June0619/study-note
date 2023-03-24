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


