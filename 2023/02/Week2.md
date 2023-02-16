# Week 1 (2023-02-01 ~ 2023-02-12)

## GoF 디자인 패턴

### 중재자 (Mediator) 패턴
- 객체 간의 소통을 캡슐화 하는 패턴
- 객체 사이의 결합도를 낮출 수 있다. (중재자 객체의 결합도는 올라간다)

### 메멘토 (Memento) 패턴
- 객체 내부 상태를 캡슐화 하여 외부에 저장하는 패턴
- 너무 자주 저장하면 리소스를 낭비할 수 있다.

### 옵저버 (Observer) 패턴
- 여러 객체들이 특정 객체의 상태 변화를 감지하고 알림을 받는 패턴
- Subject(변화 감지 대상) 와 Observer 로 나뉜다.
- 너무 많은 객체가 옵저버로 등록되어 있으면 메모리 누수가 발생할 수 있다.(`WeekRefference` 이용 가능)

### 상태 (State) 패턴
- 객체 내부 상태에 따라 행동이 달라지는 패턴

### 전략 (Stretagy) 패턴
- 알고리즘을 캡슐화 하여 상호 교환 가능하게 만드는 패턴
- 새로운 전략 추가가 쉬움 (OCP)

---

## JAVA

### Try-with-resources
- 기존 try-catch-finally 구문 형태의 문제점 (자원을 종료해주어야 함) 을 보안하기 위한 문법 (Java 7 이후로 지원)

- try-with-resource 문법에서 자원을 사용 후 자동으로 종료하기 위해서는 `AutoCloseable` 인터페이스 상속 및 `close()` 메서드 구현이 필수

```java

    // try-catch-finally

    FileInputStream is = null;
    BufferedInputStream bis = null;

    try {
        is = new FileInputStream("file.txt");
        bis = new BufferedInputStream(is);
        int data = -1;

        while((data = bis.read()) != -1){
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printstackTrace();
    } finally {
        if (is != null) is.close();
        if (bis != null) bis.close();
    }

```

```java

    // try-with-resrouce

    try (FileInputStream is = new FileInputStream("file.txt");
    BufferedInputStream bis = new BufferedInputStream(is)) {
        
        int data = -1;

        while ((data = bis.read()) != -1) {
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printstackTrace();
    }

```

### List.of()
- <b>불변한 객체</b>가 리턴되기 때문에 사용에 주의해야 함
- 객체를 수정해야 할 필요가 있을경우 새로운 리스트 생성 후 add 로 추가 

### ApplicationRunner
- 스프링 부트가 기동되면서 작동할 코드를 제공하는 인터페이스