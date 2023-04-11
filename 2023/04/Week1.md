# Week 4 (2023-04-03 ~ 2023-04-16)

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

## 정보통신망 5강 - 데이터 전송기술2
### 오류 검사

## CI/CD 
- docker exec -it {container name} bash
- Maven plugin install
    - Manage Jenkins -> Jenkins Plugins -> Available -> Maven

### Exercise2
- Goals and Options
    - clean : Remove previous files
    - compile
    - package : JAR, WAR Packaging

- Trouble Shooting
    - 빌드 후 톰캣 배포 시 배포되지 않는 경우
        - 톰캣 배포 환경(외부 OS) 와 젠킨스 구축 환경 (VM) 이 분리되어 있는지 확인한다.
        - VM 바깥에 배포 시 localhost ip가 아니라 private ip 주소로 배포해야 함