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

