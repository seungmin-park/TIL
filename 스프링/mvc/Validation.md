# Validation(검증)

[1.스프링 없이 순수한 자바만을 이용한 에러 검증](#스프링-없이-순수한-자바만을-이용한-에러-검증)

[2.스프링에서 제공하는 BindingResult 사용](#스프링에서-제공하는-bindingresult-사용)

- [2-1.FieldError, ObjectError 첫 번째 생성자 사용](#fielderror-objecterror-첫-번째-생성자-사용)

- [2-2.FieldError, ObjectError 두 번째 생성자 사용](#fielderror-objecterror-두-번째-생성자-사용)

[3.에러 코드와 메세지 처리](#에러-코드와-메세지-처리)

- [3-1.rejectValue, reject](#rejectvalue-reject)

- [3-2.ObjectError(reject) 에러 메세지 처리](#objecterrorreject-에러-메세지-처리)

[4.Validator 분리](#validator-분리)

Validation 없이 개발 시 사용자가 특정 정보를 입력하거나 할 경우,
오류가 발생하면 오류 페이지로 이동

ex)

![image](https://user-images.githubusercontent.com/78605779/160267293-0a56ae2c-8bb4-4235-bd60-9c2bc2a326bf.png)

이렇게 되면 사용자는 다시 처음으로 돌아가서 어디서 문제가 발생하는지 모르며, 입력하려던 정보를 처음부터 다시 입력해야함

우리가 실제 사용하는 서비스들은 문제가 발생하면 기존에 데이터들은 그대로 유지한 상태로 문제가 발생한 부분이 어디인지 사용자에게 친절하게 알려줌

**컨트롤러의 중요한 역할은 HTTP요청이 정상적인 요청인지 검증하는 것**

**검증 요구사항**

- ItemName : 필수, 공백X
- price : 필수, 공백X, 숫자만 입력 가능, 1,000원 이상 1,000,000원 이하
- quantity : 필수, 공백X, 숫자만 입력 가능, 최대 9999개까지
- 가격 \* 수량은 10,000원 이상

**프로젝트 설정**

`객체 - Item`

```java
public class Item{

    private Long id; //식별자 아이디
    private String itemName; //이름
    private Integer price; //가격
    private Integer quantity; //수량

    // Getter, Setter ...
}
```

**예시에서는 Mapping 정보, ItemRepository등 부가적인 정보 생략, 에러 검증 로직에 집중**

### 스프링 없이 순수한 자바만을 이용한 에러 검증

```java
public String V1(@ModelAttribute Item, Item, Model model){

    // 에러 보관 <에러 발생 부분, 에러 내용>
    Map<String, String> errors = new HashMap<>();

    // 아이템 명이 비어있을 경우
    if(!StringUtils.hasText(item.getItemName())){
        errors.put("itemName","상품 이름은 필수");
    }
    // 가격이 비어있거나, 범위를 벗어났을경우
    if(item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000){
        errors.put("price","1,000원 이상 1,000,000원 이하");
    }
    //수량이 비어있거나, 범위를 벗어났을경우
    if(item.getQuantity() == null || item.getQuantity() > 9999){
        errors.put("quantity","최대 9999개 까지 입력");
    }

    if(item.getQuantity() != null && item.getPrice() != null){
        int totalPrice = item.getQuantity() * item.getPrice();
        if(totalPrice < 10000){
            errors.put("totalPrice","가격 * 수량은 10,000원 이상");
        }
    }
    //errors가 비어있지 않으면 -> errors 발생
    if(!errors.isEmpty()){
        model.addAttribute("errors",errors);
        return "addForm";
    }

    // 성공시 정상적인 로직 실행
    ...
}
```

타임리프 사용 시 에러 처리

```html
<div th:if="${errors?.containKey('totalPrice')}">
  <p th:text="${errors['totalPrice']}">오류 메세지</p>
</div>

<p th:if="${errors?.containKey('itemName')}" th:text="${errors['itemName']}">
  상품명 오류
</p>

<p th:if="${errors?.containKey('price')}" th:text="${errors['price']}">
  상품명 오류
</p>

<p th:if="${errors?.containKey('quantity')}" th:text="${errors['quantity']}">
  상품명 오류
</p>
```

errors 가 존재하면 div 태그들이 렌더링되어 사용자에게 어느부분에서 에러가 발생했는지 알려줌

`errors?.` ?.는 containKey()를 실행시 errors가 존재하지 않으면 NullPointException이 발생하는 대신 null 반환, th:if에서 null을 받으면 실패로 간주하여 렌더링할 떄 무시

**현재까지의 문제점**

price, quantity 타입 오류를 해결할 수 없음
각각에 숫자가 아닌 문자를 입력시 Controller를 호출되기 전에 에러가 발생
이 문제를 해결하기 위해서는 price와 quantity를 전부 String으로 변환 후 숫자로 변환이 가능한지 판단해야함

### 스프링에서 제공하는 BindingResult 사용

**BindingResult는 검증하려는 객체 다음에 위치**

#### FieldError, ObjectError 첫 번째 생성자 사용

```java
//FieldError
public FieldError(String objectName, String field, String defaultMessage) {
}

//ObjectError
public ObjectError(String objectName, String defaultMessage) {
}
```

`objectName` : 문제가 되는 @ModelAttribute 객체 이름

`field` : 오류가 발생한 필드명

`defaultMessage` : 오류가 발생했을 때 보여주는 메세지

```java
public String V2(@ModelAttribute Item, Item, BindingResult bindingResult, Model model){

    // 아이템 명이 비어있을 경우
    if(!StringUtils.hasText(item.getItemName())){
        bindingResult.addError(new FieldError("item", "itemName","상품 이름은 필수"));
    }
    // 가격이 비어있거나, 범위를 벗어났을경우
    if(item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000){
        bindingResult.addError(new FieldError("item", "price","1,000원 이상 1,000,000원 이하"));
    }
    //수량이 비어있거나, 범위를 벗어났을경우
    if(item.getQuantity() == null || item.getQuantity() > 9999){
        bindingResult.addError(new FieldError("item", "quantity","최대 9999개 까지 입력"));
    }

    if(item.getQuantity() != null && item.getPrice() != null){
        int totalPrice = item.getQuantity() * item.getPrice();
        if(totalPrice < 10000){
            bindingResult.addError(new ObjectError("item", "가격 * 수량은 10,000원 이상"));
        }
    }

    // error가 있을 경우
    if(bindingResult.hasErrors()){
        return "addForm";
    }

    // 성공시 정상적인 로직 실행
    ...
}
```

bindingResult는 자동으로 view에 넘어가므로 model.addAttribute()를 해주지 않는다.

타임리프 사용 시 에러 처리

```html
<div th:if="${#fields.hasGlobalErrors()}">
  <p th:each="err : ${#fields.GlobalErrors()}" th:text="${err}">오류 메세지</p>
</div>

<p th:errors="${item.itemName}">상품명 오류</p>

<p th:errors="${item.price}">상품명 오류</p>

<p th:errors="${item.quantity}">상품명 오류</p>
```

`#fields` : controller에서 넘어온 bindingResult에 접근

`GlobalErrors` : FleldError를 제외한 ObjectError 접근

`th:errors` : 해당 필드에 오류가 존재할 경우 태그 출력, 에러 관련해서 `th:if`를 편하게 사용할 수 있는 속성

#### FieldError, ObjectError 두 번째 생성자 사용

```java
//FieldError
public FieldError(
    String objectName,
    String field,
    @Nullable Object rejectedValue,
    boolean bindingFailure,
    @Nullable String[] codes,
    @Nullable Object[] arguments,
    @Nullable String defaultMessage) {
}

//ObjectError
public ObjectError(
    String objectName,
    @Nullable String[] codes,
    @Nullable Object[] arguments,
    @Nullable String defaultMessage) {
}
```

`objectName` : 문제가 되는 @ModelAttribute 객체 이름

`field` : 오류가 발생한 필드명

`rejectedValue` : 오류가 발생한 값

`bindingFailure` : 사용자가 입력한 데이터가 @ModelAttribute 객체로 변환이 실패했는지 여부

`codes` : 메세지 코드

`arguments` : 메세지 코드에서 사용하는 데이터

`defaultMessage` : 오류가 발생했을 때 보여주는 메세지

```java
// 아이템 명이 비어있을 경우
    if(!StringUtils.hasText(item.getItemName())){
        bindingResult.addError(new FieldError("item", "itemName", item.getName(), false, null, null, "상품 이름은 필수"));
    }
    // 가격이 비어있거나, 범위를 벗어났을경우
    if(item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000){
        bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, null, null,"1,000원 이상 1,000,000원 이하"));
    }
    //수량이 비어있거나, 범위를 벗어났을경우
    if(item.getQuantity() == null || item.getQuantity() > 9999){
        bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, null, null,"최대 9999개 까지 입력"));
    }

    if(item.getQuantity() != null && item.getPrice() != null){
        int totalPrice = item.getQuantity() * item.getPrice();
        if(totalPrice < 10000){
            bindingResult.addError(new ObjectError("item", null, null, "가격 * 수량은 10,000원 이상"));
        }
    }

    // error가 있을 경우
    if(bindingResult.hasErrors()){
        return "addForm";
    }
```

두 번째 파라미터를 사용시 에러가 발생했을 때 사용자가 이전에 입력했던 값을 다시 제공함으로써, 사용자가 내용을 처음부터 다시 입력하지 않아도 된다.

두 번째 파라미터를 통해 사용자가 잘못된 타입의 값을 입력했을 경우에도 사용자가 입력한 값을 그대로 유지할 수 있다.

ex)가격에 문자를 입력했을 경우 예상 코드

```java
bindingResult.addError(new FieldError("item", "price", request.getParameter("price"), true, ...));
```

### 에러 코드와 메세지 처리

FieldError, ObjectError에 존재하는 `codes`, `arguments` 를 이용해 오류 코드를 통해 오류 메세지 사용

**에러 메세지 설정 추가**

```properties
<!-- application.properties -->
spring.messages.basename=errors
```

`resources/errors.properties`

```properties
required.item.itemName=상품 이름은 필수.
range.item.price=가격은 {0} ~ {1} 까지.
max.item.quantity=수량은 최대 {0} 까지.
totalPrice=가격 * 수량의 합은 {0}원 이상, 현재 값 = {1}
```

```java
// 아이템 명이 비어있을 경우
    if(!StringUtils.hasText(item.getItemName())){
        bindingResult.addError(new FieldError("item", "itemName", item.getName(), false, new String[]{"required.item.itemName"}, null, null));
    }
    // 가격이 비어있거나, 범위를 벗어났을경우
    if(item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000){
        bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, new String[]{"range.item.price"}, new Object[]{1000,1000000},null));
    }
    //수량이 비어있거나, 범위를 벗어났을경우
    if(item.getQuantity() == null || item.getQuantity() > 9999){
        bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, new String[]{"max.item.quantity"}, new Object[]{9999},null));
    }

    if(item.getQuantity() != null && item.getPrice() != null){
        int totalPrice = item.getQuantity() * item.getPrice();
        if(totalPrice < 10000){
            bindingResult.addError(new ObjectError("item", new String[]{"totalPrice"}, new Object[]{10000,totalPrice}, null));
        }
    }

    // error가 있을 경우
    if(bindingResult.hasErrors()){
        return "addForm";
    }
```

`codes` : 스트링 배열{에러 메세지 코드명} -> `new String[]{"codeName",...}`

`arguments` : 오브젝트 배열{에러 메세지 코드에서 사용할 인자 값} -> `new Object[]{arguments,...}` 메세지 코드의 {0}, {1}... 에 치환

**에러 메세지 코드가 존재할 겨우 에러 메세지 코드를 출력하고, 없을 경우 defaultMessage를 출력한다.**

코드가 점점 길어진다

#### rejectValue, reject

```java
bindingResult.getObjectName() // -> item
bindingResult.getTarget() // -> Item(id=null, itemName=111, price=1111,quantity=1111)
```

bindingResult는 자신이 검증해야할 객체가 무엇인지 알고있다.

즉 `objectName`을 생략가능

```java
bindingResult.addError(new FieldError(...)); -> bindingResult.rejectValue();
bindingResult.addError(new ObjectError(...)); -> bindingResult.reject();
```

**rejectValue, reject로 변경**

```java
// 아이템 명이 비어있을 경우
    if(!StringUtils.hasText(item.getItemName())){
        //fieldName, errorCode
        bindingResult.rejectValue("itemName","required");
    }
    // 가격이 비어있거나, 범위를 벗어났을경우
    if(item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000){
        //fieldName, errorCode, arguments
        bindingResult.rejectValue("price","range",new Object[]{1000,1000000});
    }
    //수량이 비어있거나, 범위를 벗어났을경우
    if(item.getQuantity() == null || item.getQuantity() > 9999){
        //fieldName, errorCode, arguments, defaultMessage
        bindingResult.rejectValue("quanttiy","max",new Object[]{9999}, null);
    }

    if(item.getQuantity() != null && item.getPrice() != null){
        int totalPrice = item.getQuantity() * item.getPrice();
        if(totalPrice < 10000){
            /*
            errorCode,
            errorCode, arguments
            errorCode, arguments, defaultMessage
            */
            bindingResult.reject("totalPrice",new Object[]{10000, totalPrice},null);
        }
    }

    // error가 있을 경우
    if(bindingResult.hasErrors()){
        return "addForm";
    }
```

FieldError와 ObjectError를 사용시 에러코드를 모두 입력했지만 rejectValue와 reject를 사용할 떄는 required와 같이 입력해도 동작한다.

**스프링의 `MessageCodesResolver` 지원**

rejectValue와 reject는 내부적으로 MessageCodesResolver를 사용한다.

MessageCodesResolver는 다음과 같은 규칙을 통해 에러 코드 메세지를 생성한다.

`ObjectError`

```
1.에러코드.오브젝트 이름
2.에러코드

ex)
1.totalPrice.item
2.totalPrice
```

`FieldError`

```
1.에러코드명.오브젝트명.필드명
2.에러코드명.필드명
3.에러코드명.필드타입
4.에러코드명

ex)
1.required.item.itemName
2.required.itemName
3.required.java.lang.String
4.required
```

bindingResult는 검증해야할 객체를 알고있기 떄문에 에러코드와 필드명만 입력하여도 다음과 같이 에러 코드 메세지들을 생성후 우선순위에 따라 에러 코드 메세지를 출력할 수 있는것이다.

이러한 기능 덕분에 단순한게 에러 메세지를 작성하여 범용적으로 사용하고, 필요한 부분만 세밀하게 메세지를 작성하여 개발자는 요구사항에 따라 손쉽게 에러 메세지를 처리할 수 있게된다.

#### ObjectError(reject) 에러 메세지 처리

여지껏 우리는 검증오류만 해결

검증 오류 같은 경우 개발자가 직접 rejectValue와 같은 메소드를 직접 호출하여 값을 세팅해주었다.

그러나 숫자만 입력해야하는 가격 부분에 문자를 입력할 경우와 같이 바인딩에서 에러가 발생할 경우, 에러 페이지로 이동하는것은 막았지만 에러 메시지를 우리가 지정할 수 없다.

이를 에러 메세지 코드를 이용해 해결해보자.

`price`에 문자를 입력한 후 로그 확인

`Field error in object 'item' on field 'price': rejected value [ㅂ]; codes [typeMismatch.item.price,typeMismatch.price,typeMismatch.java.lang.Integer,typeMismatch];`

에러 코드

```
1.typeMismatch.item.price
2.typeMismatch.price
3.typeMismatch.java.lang.Integer
4.typeMismatch
```

로그를 확인해본 결과 오브젝트명 : item, 필드명 : price, 에러코드명 : typeMismatch으로 필드 에러가 발생했다.

errors.properties에 typeMismatch관련 에러 메세지를 추가하자

```properties
typeMismatch.item.price=가격에는 숫자만 입력가능합니다.
typeMismatch=잘못된 종류의 값 입력
```

다음과 같이 에러 메세지를 추가하여 코드 수정없이 원하는 메세지를 출력할 수 있다.

### Validator 분리

현재까지 데이터를 검증하는 부분이 컨트롤러에 너무 많은 부분을 차지하고 있다.

검증 부분을 클래스로 분리하여 재사용성을 높이고 컨트롤러의 코드를 간단하게 유지하자.

`Validator`

```java
//스프링에서 제공하는 Validator 구현
public class ItemValidator implements Validator{

    @Override
    public boolean supports(Class<?> clazz) {
        return Item.class.isAssignableFrom(clazz);
        // item == clazz
        // item == subItem
    }

     @Override
    public void validate(Object target, Errors errors) {
        Item item = (Item) target;
        // 아이템 명이 비어있을 경우
        if (!StringUtils.hasText(item.getItemName())) {
            errors.rejectValue("itemName", "required");
        }
        // 가격이 비어있거나, 범위를 벗어났을경우
        if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
            errors.rejectValue("price","range",new Object[]{1000,1000000},null);
        }
        //수량이 비어있거나, 범위를 벗어났을경우
        if (item.getQuantity() == null || item.getQuantity() >= 9999) {
            errors.rejectValue("quantity","max",new Object[]{9999},null);
        }

        if (item.getPrice() != null && item.getQuantity() != null) {
            int resultPrice = item.getPrice() * item.getQuantity();
            if (resultPrice < 10000) {
                errors.reject("totalPriceMin",new Object[]{10000, resultPrice},null);
            }
        }
    }
}
```

`Controller`

```java
ItemValidator itemValidator = new ItemValidator();

itemValidator.validate(item, bindingResult);

if (bindingResult.hasErrors()) {
    return "addForm";
}
```

스프링에서 제공하는 Validator를 사용하면 체계적으로 validation이 가능해진다.

**WebDataBinder 사용**

`Controller`

```java

private final ItemValidator itemValidator; // itemValidator @Component 추가 빈 등록

//컨트롤러 내부의 메소드 실행될 때마다 실행, @Validated 객체 검증 실행
// Validator supports메소드를 통해 검증 가능한 객체인지 체크
@InitBinder
public void init(WebDataBinder webDataBinder){
    webDataBinder.addValidator(itemValidator);
}

//검증할 객체 @Validated 추가
public String add(@Validated @ModelAttribute Item item, BindingResult bindingresult, Model model){

    if (bindingResult.hasErrors()) {
        return "addForm";
    }
}
```

<hr/>

## 학습 자료

[인프런)스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2/dashboard)
