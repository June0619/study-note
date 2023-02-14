# Week 1 (2023-02-01 ~ 2023-02-12)

## GoF 디자인 패턴

### 중재자 (Mediator) 패턴
- 객체 간의 소통을 캡슐화 하는 패턴
- 객체 사이의 결합도를 낮출 수 있다. (중재자 객체의 결합도는 올라간다)

### 메멘토 (Memento) 패턴
- 객체 내부 상태를 캡슐화 하여 외부에 저장하는 패턴
- 너무 자주 저장하면 리소스를 낭비할 수 있다.

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