# message
메시지, 국제화 실습

## 메시지, 국제화

### 메시지

기획자가 화면에 보이는 문구가 마음에 들지 않는다고
상품명이라는 단어를 상품이름으로 변경해달라고 한다면 어떻게 해야할까?
> as is: 상품명
> 
> to be: 상품이름

만약 HTML 파일에 메시지가 하드코딩 되어있다면...
수십개의 파일을 모두 고쳐야 한다...😭

이러한 문제를 해결하기 위해서
메시지를 한곳에서 관리하도록 하는 기능을 **메시지 기능**이라고 한다.

예를 들어 아래와 같은 `messages.properties`파일을 만든다.
```properties
item=상품
item.id=상품 ID
item.itemName=상품명
item.price=가격
item.quantity=수량
```

그리고 HTML에 다음과 같이 해당 데이터를 key값으로 불러서 사용한다.
```html
<label for="itemName" th:text"#{item.itemName}"></label>
```

### 국제화
메시지에서 한 발 더 나아가서
`한국어`, `영어` 둘 다 지원해야 한다면 `messages.properties`파일을 만들어서 관리한다.

`messages_ko.properties`파일
```properties
item=상품
item.id=상품 ID
item.itemName=상품명
item.price=가격
item.quantity=수량
```

`messages_en.properties`파일
```properties
item=Item
item.id=Item ID
item.itemName=Item Name
item.price=Price
item.quantity=Quantity
```

**한국에서 접근한 것인지 영어에서 접근한 것인지 인식하는 방법**
1. HTTP `accept-language`헤더 값을 사용
2. 사용자가 직접 언어를 선택하도록 하고, `쿠키 🍪` 등을 사용해서 처리

## 스프링 국제화 메시지 선택
스프링도 `Locale` 정보를 알아야 언어를 선택 할 수 있다.
스프링은 언어 선택시 기본으로 `Accept-Language`헤더의 값을 사용한다.

### LocaleResolver
스프링은 `Locale`선택 방식을 변경할 수 있도록 `LocaleResolver`라는 인터페이스를 제공하는데,
스프링 부트는 기본적으로 Accept-Language를 활용하는 `AcceptHeaderLocaleResolver`를 사용한다.

**LocaleResolver 인터페이스**
```java
public interface LocaleResolver {
    Locale resolveLocale(HttpServletRequest request);
    void setLocale(HttpServletRequest request, @Nullable HttpServletResponse response, @Nullable Locale locale);
}
```

**LocaleResolver 변경**

만약 `Locale` 선택 방식을 변경하려면 `LocaleResolver`의 구현체를 변경해서 쿠키나 세션 기반의 `Locale`선택 기능을 사용할 수 있다.
**고객이 직접 `Locale`을 선택**하도록 하는 것!
