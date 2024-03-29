# Week 3 (2023-12-18 ~ 2023-12-24)

## 스프링 핵심 원리 - 고급편

### 구체 기반 프록시
상속 받은 클래스의 생성자는 부모의 생성자를 호출하는데 부모 클래스에 기본생성자는 없는 경우 null 을 파라미터로 받는 생성자를 인위적으로 만들어주어야 한다.

```java
public OrderServiceConcreteProxy(OrderServiceV2 target, LogTrace logTrace) {
        super(null);
        this.target = target;
        this.logTrace = logTrace;
}
```

### 인터페이스 기반 프록시 vs 클래스 기반 프록시
- 클래스 기반 프록시는 해당 클래스에만 적용할 수 있다. 인터페이스 기반 프록시는 해당 타입을 상속받는 모든 곳에 적용할 수 있다.
- 클래스 기반 프록시는 상속을 사용하기 때문에 제약이 있다.
  - 부모 클래스의 생성자를 호출해야 한다.
  - final 키워드가 붙으면 상속이 불가능하다.
  - final 키워드가 붙으면 메서드를 오버라이딩 할 수 없다.
- 인터페이스 기반 프록시는 캐스팅 관련해서 단점이 존재한다.

### 프록시 패턴과 데코레이터 패턴

- 둘다 프록시 컴포넌트를 쓴다는 점은 똑같지만 **의도**에 명확한 차이가 있다.
- 보통 접근 제어를 위한 의도가 프록시 패턴이고, 부가 기능을 붙이는 의도가 데코레이터 패턴으로 부른다.