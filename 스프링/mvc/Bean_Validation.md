# Bean Validation

### Bean Validation이란?

특정 기술이 아닌 검증 어노태이션과 인터페이스의 모음(기술 표준), 구현체는 Hibernate Validator(ORM과 연관 없음)

ex) JPA(표준) - Hibernate(구현체)

사용 예시

```java
public class Car {

   @NotNull//null이 들어올 수 없음
   private String manufacturer;

   @NotNull//null이 들어올 수 없음
   @Size(min = 2, max = 14) //최소 2, 최대 14
   private String licensePlate;

   @Min(2) //최소값 2
   private int seatCount;

   // ...
}
```

이 처럼 간단하게 해당 필드 검증을 프로젝트 전체에 적용할 수 있도록 공통화 및 표준화 한 것이 Bean Validation

Bean Validation은 자바 표준 기술이며 스프링 부트 2.3.0 이상을 사용하면 별도로 dependency를 추가해야 사용할 수 있다.

```groovy
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

[공식 사이트](http://hibernate.org/validator/)

### 검증 어노태이션

`@NotNull` : null값 허용 X

`@NotEmpty` : null값, "" 허용 X

`@NotBlank` : null값, "", " " 허용 X

`@max(10)` : 최대 10까지 허용

`@min(10)` : 최소 10부터 허용

[더 많은 검증 어노태이션 모음](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/?v=7.0#validator-defineconstraints-spec)

...

`객체 - Item`

```java
public class Item{

    @NotNull
    private Long id; //식별자 아이디

    @NotBlank
    private String itemName; //이름

    @NotNull
    @Range(min=1000, max=10000)
    private Integer price; //가격

    @NotNull
    @Max(9999)
    private Integer quantity; //수량

    // Getter, Setter ...
}
```

**예시에서는 Mapping 정보, ItemRepository등 부가적인 정보 생략, 에러 검증 로직에 집중**

### Bean Validation 사용

```java
public String add(@Valid @ModelAttribute Item item, BindingResult bindingresult, Model model){

    if (bindingResult.hasErrors()) {
        return "addForm";
    }
}
```

객체에 검증 어노태이션을 추가하고 검증할 객체에 `@Valid`를 추가만 해주면 된다.

Spring boot는 Bean Validation 라이브러리를 추가하면 LocalValidatorBean을 글로벌 Validator로 등록한다. 그래서 `@Valid` 또는 `@Validated`가 붙은 객체에 검증 로직을 수행한다.

Bean Validation은 @ModelAttribute를 통해 값이 제대로 바인딩 객체에서만 검증 로직을 수행한다. 만약 값을 바인딩 하는데 실패하면 FieldError에 typeMismatch를 추가한다.

예를 들어 가격을 입력하는 부분에 문자를 입력했을 경우 가격이 범위안에 들어오는지 검증하는 의미가 없다.

### 에러 코드

Bean Validation은 에러 코드를 필드에 추가한 어노태이션 이름으로 등록한다.

```properties
<!-- @NotBlnak -->
NotBlank.item.itemName     어노태이션.객체명.필드명
NotBlank.itemName          어노태이션.필드명
NotBlank.java.lang.String  어노태이션.타입
NotBlank                   어노태이션

<!-- @Range -->
Range={0}, {2} ~ {1} 까지
```

에러 메세지를 작성할 때 {0}은 필드명에 해당하고, {1},{2}는 대체적으로 파라미터에 해당하지만 정확한 스펙은 사용하는 어노태이션을 찾아보자.

#### Bean Validation 메세지 찾는 순서

1.에러 메세지에 작성한 메세지 확인

2.필드에 추가한 검증 어노태이션에 message속성 확인 ex)`@Notnull(message="null X")`

3.라이브러리에서 제공하는 default값

### ObjectError

`@ScriptAssert()`사용

```java
@ScriptAssert(lang = "javascript", script="_this.price * _this.quantity >= 10000")
public class Itme{
    ...
}
```

`ScriptAssert.item`, `ScriptAssert`로 ObjectError에서 에러 코드 만드는 방식과 같은 방식으로 에러 메세지 코드가 생성된다.

**비 추천**

여러 객체를 참조해서 검증해야하는 경우 사용하기 힘들다.

**ObjectError는 직접 자바 코드로 작성하는 것을 권장**

### 한계

하나의 객체를 분리된 화면에서 각각 검증하는 내용이 다를 경우

ex) 아이템 등록 시 수량 제한 최대 9999, 수정 시 수량 제한 없음

```java
public class Item{
    ...
    @Max(9999)
    private Integer quantity;
    ...
}
```

아이템을 등록하는 경우에 수량을 제한하기 위해 `@Max`를 사용하였다. 하지만 수정하는 경우에 제한을 없애기 위해서 `@Max`를 제거하는 순간 등록 검증 로직이 사라진다.

#### groups 사용

등록할 경우에 검증 로직과 수정할 경우에 검증 로직을 그룹으로 분리

등록 그룹 생성

```java
public interface SaveG{}
```

수정 그룹 생성

```java
public interface UpdateG{}
```

그룹 추가

```java
public class Item{
    ...
    @NotNull(groups={SaveG.class, UpdateG.class})
    @Max(value = 9999, groups=SaveG.class)
    private Integer quantity;
    ...
}
```

```java
public class add(@Validated(SaveG.class) ...){
    ...
}

public class edit(@Validated(UpdateG.class) ...){
    ...
}
```

`@Validated`에서 사용할 그룹을 지정해주므로써 상황에 맞게 검증 로직을 실행할 수 있다.

스프링에세 제공하는 `@Validated`의 기능으로 `@Valid`에서는 사용할 수 없다.

#### Form 전송 객체 분리

현실은 사용자로부터 전달받는 데이터와 정확하게 일치하는 도메인 객체를 만들기는 어렵다.

예를 들어 특정 사이트에 회원가입을 할 경우 개인 정보만 입력하는 것이 아닌 약관 동의 정보등도 함께 넘어오게 된다.

그렇기 때문에 복잡한 데이터를 받아줄 별도의 객체를 만든 후 필요한 데이터를 통해 도메인 객체를 생성한다.

**Item 객체 검증 어노태이션 전부 제거**

이제 검증은 form 객체에서 할 것이기 때문에 도메인 객체에 검증 어노태이션이 필요없다.

`itemSaveForm`

```java
public class ItemSaveForm{

    @NotBlank
    private String itemname;

    @NotNull
    @Range(min = 1000, max = 1000000)
    private Integer price;

    @NotNull
    @Max(9999)
    private Integer quantity;

    //Getter, Setter
}
```

```java
public String add(@Valid @ModelAttribute ItemSaveFrom itemSaveForm ...){
    ...

    //성공 로직
    Item item = new Item();
    item.setItemName(itemSaveForm.getItemname());
    item.setPrice(itemSaveForm.getPrice());
    item.setQuantity(itemSaveForm.getQuantity());

    itemRepository.save(item);

    return "addForm";
}
```

상황에 맞는 기능을 구상하고, 검증도 분리가 가능하다.

### HTTP 메세지 컨버터

HTTP 요청 파라미터가 아닌 HTTP Body에 JSON 요청에서 Validation

**고려사항**

1.성공적인 요청

2.JSON을 객체로 변환하는 것 자체가 실패하는 요청

3.객체로 변환은 성공했으나, 검증 로직에서 실패하는 요청

객체로 변환하는 과정에서 실패할 경우 컨트롤러 자체가 실행되지 않고 에러가 발생한다.

#### 객체로 변환하는 것 자체가 실패하는 요청

```json
{
  "timestamp": "2022-04-01T12:44:17.091+00:00",
  "status": 400,
  "error": "Bad Request",
  "message": "",
  "path": "/validation/api/items/add"
}
```

#### 검증 로직에서 실패하는 요청

`Controller`

```java
if (bindingResult.hasErrors()) {
    log.info("검증 오류 발생 errors={}",bindingResult);
    return bindingResult.getAllErrors();
}
```

`결과`

```json
[
  {
    "codes": [
      "Range.itemSaveForm.price",
      "Range.price",
      "Range.java.lang.Integer",
      "Range"
    ],
    "arguments": [
      {
        "codes": ["itemSaveForm.price", "price"],
        "arguments": null,
        "defaultMessage": "price",
        "code": "price"
      },
      1000000,
      1000
    ],
    "defaultMessage": "1000에서 1000000 사이여야 합니다",
    "objectName": "itemSaveForm",
    "field": "price",
    "rejectedValue": 1111111111,
    "bindingFailure": false,
    "code": "Range"
  }
]
```

이전과는 다르게 컨트롤러가 실행된다.

`bindingResult.getAllErrors()`는 발생한 FieldError와 ObjectError를 모두 반환한다.

@ModelAttribute는 각각의 필드 단위로 적용되기 때문에 특정 필드 타입 에러(`typeMismatch`)가 발생해도 나머지는 전부 정상적인 처리가 가능하다.

HttpMessageConverter는 객체 단위로 적용된다. 객체로 변환하는데 완전히 성공해야지만 검증 로직을 수행할 수 있다.

<hr/>

## 학습 자료

[인프런)스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2/dashboard)
