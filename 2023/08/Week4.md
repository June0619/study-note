# Week 4 (2023-08-21 ~ 2023-08-27)
## Spring MVC2 - 파일 업로드
### Spring - 파일 업로드 다운로드

- 파일 다운로드 시 헤더에 `attachment; filename="test.txt"` 와 같이 추가해주어야 한다.
- 컨트롤러에서 `Resource` 타입을 반환함으로써 이미지 및 첨부파일 다운로드를 지원할 수 있다.

## Spring DB 1편 - JDBC 이해

### JDBC(Java Database Connectivity) 등장 배경
- DB 벤더사 별로 사용 방법이 너무 달라서 개발자들이 어려움을 많이 겪었음
- 이것들을 표준 인터페이스로 추상화 하여 DB가 바뀌더라도 동일한 사용법을 가지는 인터페이스 개발

### JDBC 주요 표준 기능 세가지
1. `java.sql.Connection` - 연결
2. `java.sql.Statement` - SQL을 담은 내용
3. `java.sql.ResultSet` - SQL 요청 응답

### 참고
- 이렇게 표준화 된 기능을 상속받은 JDBC 구현체들을 JDBC 드라이버라 하며 각 DB 별로 존재한다.
- ANSI SQL 이라는 표준 SQL이 있긴 하지만 일반적인 부분만 공통화 되었으며 한계가 존재한다. 페이징 등은 아직 DB별로 문법이 많이 다름

### JDBC 와 데이터 접근 기술
- 종류
    - JDBC
    - SQL MAPPER
    - ORM
- 기본적으로 다른 응용기술들 또한 결국 low 레벨에서는 JDBC 를 사용하기 때문에 JDBC 를 잘 이해하지 못하면 트러블 슈팅이 힘들다.

## 이펙티브 자바 - 아이템 1. 생성자 대신 정적 팩토리 메소드를 고려하라
### 장점
1. 장점 2 - 기본 생성자를 private 으로 두고 (싱글톤) 객체 생성 메모리를 관리 할 수 있다. (플라이 웨이트 패턴과 상통하는 부분 존재)
