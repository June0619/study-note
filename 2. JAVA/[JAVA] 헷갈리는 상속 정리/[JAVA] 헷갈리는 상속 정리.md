# [JAVA] 헷갈리는 상속 정리

## 1. 개요

JAVA 를 공부하면 가장 처음에 듣고, 자주 들으며, JAVA 관련 포지션으로 지원 시 많은 면접에서도 단골로 등장하는 내용들이 있다. 상속과 다형성에 관한 내용인데, 부끄럽지만 기초적인 내용이라고 생각하고 괄시하고 있다가 실무를 하면서도 헷갈리는 순간이 제법 있었다. 마침 학교 수업으로 자바 과목을 듣던 중, 정리하기에 좋은 기회가 생겨 한번 정리해두려고 한다. 

## 2.  접근제어자

### 사용 범위

| 접근 제어자 | 사용 범위 | 클래스에 사용할 시 |
| --- | --- | --- |
| public | 전 범위 | - 클래스명과 소스파일명 일치
- 하나의 소스파일에 하나만 존재 |
| default (생략) | 같은 패키지 내 |  |
| private | 해당 클래스 내부 | Inner Class 를 정의할 때에만 사용 |
| protected | 해당 클래스 및 
상속클래스 내부 | Inner Class 를 정의할 때에만 사용 |

## 3. final 키워드

- 필드
    
    선언 후 초기화가 이루어지면 절대로 값을 변경할 수 없는 상수가 된다.
    
- 메소드
    
    오버라이딩이 불가능해진다.
    
- 클래스
    
    해당 클래스를 상속받는 서브 클래스를 정의할 수 없다.
    

## 4. static 키워드

- 필드
    
    특정 객체에 static 필드가 존재하면 다른 참조값을 가지는 객체가 아무리 많아도 같은 값을 공유한다.
    
- 메소드
    
    객체의 인스턴스와 무관하게 어디서든 호출이 가능하므로 non-static 필드나 메소드를 호출할 수 없다.
    
- 클래스
    
    static 키워드는 원래 클래스에는 붙일 수 없으나, 예외적으로 중첩 클래스(inner class) 를 선언할 때에 사용할 수 있다.  
    

```java
 class Scratch {

    public static void main(String[] args) throws IOException {

        // Static 중첩 클래스는 부모 클래스 인스턴스가 없어도 생성할 수 있음
        OuterClass.NestedStaticClass nsc = new OuterClass.NestedStaticClass();
        nsc.printStaticMsg();

        // 일반 중첩 클래스는 부모 클래스 인스턴스가 선언되어야지만 생성할 수 있음
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.printMsg();

        // 아래와 같은 형태로도 선언 가능
        OuterClass.InnerClass innerObject = new OuterClass().new InnerClass();

        innerObject.printStaticMsg();

    }
}

class OuterClass {

    private static String msg = "정적 메시지";
    private String msg2 = "일반 메시지";

    public static class NestedStaticClass {
        public void printStaticMsg() {
            System.out.println("[Static 중첩 클래스에서 호출 됨] : " + msg);
        }
        // ERROR -> Static 중첩 클래스에서는 Non-Static 필드를 호출할 수 없음
//        public void printMsg() {
//            System.out.println("[Static 중첩 클래스에서 호출 됨] : " + msg2);
//        }
    }

    public class InnerClass {
        public void printStaticMsg() {
            System.out.println("[INNER 클래스에서 호출 됨]" + msg);
        }
        public void printMsg() {
            System.out.println("[INNER 클래스에서 호출 됨]" + msg2);
        }
    }
}
```

## 5. 캡슐화

클래스 내의 필드를 private 접근제어자로 선언하여 외부에서 쉽게 제어하지 못하도록 하고, 명시적인 public 메소드를 통해 값을 제어한다.

## 6. 오버로딩과 오버라이딩

- 오버로딩
    
    메소드 재정의 - 이름은 동일하나 매개변수가 다른 메소드를 정의하는 것
    
- 오버라이딩
    
    메소드 상속 재정의 - 부모의 메소드를 상속받으며 반환형, 매개변수 목록, 메소드 이름은 그대로 두고 몸체의 정의를 새로 하는 것
    
    접근제어자는 범위가 넓어지는 쪽으로 재설정 할 수 있다.
    

> 메소드의 이름과 매개변수를 묶어 메소드의 서명(signature) 이라고 한다.
한 클래스 내에 서명이 동일한 두개 이상의 메소드가 존재할 수 없다.
> 

## 7. 상속과 생성자

- 상속 받은 클래스를 컴파일 할 때 부모 클래스의 기본 생성자를 호출한다.
- 기본 생성자가 정의되어 있지 않을 시 컴파일러가 생성한다.
- 결국 모든 객체는 Object 객체를 상속 받으므로 Object 의 기본 생성자를 호출한다.

```java
 class ParentClass { // extends Object
    public ParentClass() {
        // super(); -> 컴파일 시점에 Object 의 기본 생성자를 호출 한다.;
        System.out.println("[PARENT] 인자없는 부모 클래스 호출입니다.");
    }
    public ParentClass(int a) {
        System.out.println("[PARENT, PARAM] 인자있는 부모 클래스 호출입니다.");
    }
}

class ChildClass extends ParentClass {
    public ChildClass() {
        // super(); -> 컴파일 시점에 부모 클래스의 기본 생성자를 호출한다.
        System.out.println("[CHILD] 인자없는 자식 클래스 호출입니다.");
    }
    public ChildClass(int a) {
        System.out.println("[CHILD, PARAM] 인자있는 자식 클래스 호출입니다.");
    }
}

class InheritanceConstructor {
    public static void main(String[] args) {
        ChildClass childClassA = new ChildClass();
        ChildClass childClassB = new ChildClass(10);
    }
}
```