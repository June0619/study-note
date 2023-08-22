# Week 4 (2023-08-21 ~ 2023-08-27)
## Spring MVC2 - 파일 업로드
### Spring - 파일 업로드 다운로드

- 파일 다운로드 시 헤더에 `attachment; filename="test.txt"` 와 같이 추가해주어야 한다.
- 컨트롤러에서 `Resource` 타입을 반환함으로써 이미지 및 첨부파일 다운로드를 지원할 수 있다.