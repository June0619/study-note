# Week 4 (2023-04-03 ~ 2023-04-09)

## Spring MVC2 (김영한)
### 오류 코드와 메시지 처리4
- `DefaultMessageResolver`의 기본 메시지 생성 규칙
- 객체 오류
    1. code + "." + object name
    2. code

- 필드 오류
    1. code + "." + object name + "." + field
    2. code + "." + field
    3. code + "." + field type
    4. code