# Week 1 (2023-07-03 ~ 2023-07-09)

## Sping MVC2 - 스프링 통합과 폼
- `spring-boot-starter-thymeleaf` 의존성 사용 시 Thymeleaf ViewResolver 와 관련된 상세한 설정들을 자동화 해준다.

- `th:field` 를 통해 id, name, value 등 여러 태그를 대체할 수 있다.

### 체크박스
- Form 에서 체크 된 체크박스의 값은 on 으로 넘어가는데 이를 스프링이 Boolean 타입의 True 로 변환해준다. (Spring Type Converter)
- 체크박스의 경우 체크가 되지 않으면 false 가 아니고 null 이 온다.
- Spring MVC 에서 이를 해결하기 위한 약간의 트릭을 사용한다.
    - name 필드에 _name 형태의 hidden input 을 같이 보낸다.
    - 이 경우 체크박스에 값이 오지 않아도 null 이 아닌 false 로 인식한다.

- th:field 사용 시 hidden type 의 input 또한 함께 만들어준다.
- 조회 시에도 th:field 사용 시 개발자가 직접 'checked' 처리 구현을 자동으로 해준다.

### 체크박스 - 멀티
- 다양한 컨트롤러에 동일한 내용의 모델을 제공할 경우 스프링에서는 공통화 하여 분리할 수 있다.
```
@ModelAttribute("regions")
    public Map<String, String> regions() {
        Map<String, String> regions = new LinkedHashMap<>();    
        regions.put("SEOUL", "서울");
        regions.put("BUSAN", "부산");
        regions.put("JEJU", "제주");
        return regions;
    }
```
- th:each 태그를 통해 같은 field 를 다중으로 생성하는 결과 타임리프에서는 id 가 겹치지 않도록 숫자를 붙여준다.
    - label 태그 등으로 앞서 동적으로 생성 된 id 를 찾아야 하는 경우에는 `${#ids.prev(필드명)}`, `${#ids.next(필드명)}`을 이용할 수 있다.

### 체크박스 - 라디오
- 타임리프에서는 ENUM 타입에 직접 접근 가능하다.
```
<!-- radio button -->
        <div>
            <div>상품 종류</div>
            <div th:deach="type : ${T(hello.itemservice.domain.item.ItemType).values()} class="form-check form-check-inline">
            <!-- <div th:each="type : ${itemTypes}" class="form-check form-check-inline"> -->
                <input type="radio" th:field="*{itemType}" th:value="${type.name()}" class="form-check-inline"/>
                <label th:for="${#ids.prev('itemType')}" th:text="${type.description}" class="form-check-inline">
                    BOOK
                </label>
            </div>
        </div>
```
    - 다만 패키지 전체 경로를 명시해야 하므로, 패키지 변경에 취약한 점 등 한계가 있어 잘 사용하지는 않는다.

## Spring MVC2 - 메시지, 국제화
### 개요
- 애플리케이션 내의 단어를 변경하는 경우 (ex: 상품명 -> 상품 이름) 하드 코딩된 텍스트를 변경하는것은 너무 리스크가 크다.
- 따라서 이를 통합으로 관리하고 쉽게 변경할 수 있도록 도와주는 기능들을 알아보자.
- 이를 이용하여 다양한 외국어에 대응할 수 있도록 할 수도 있다.
- - [스프링 설정과 관련 docs link](https://docs.spring.io/spring-boot/docs/current/reference/html/index.html)

### 메시지 소스
- 스프링에서 제공하는 기본적인 메시지 관리 기능을 위해 `MessageSource` 를 빈으로 등록해야 한다.
    - SpringBoot 에서는 기본 빈으로 제공한다.
    - 