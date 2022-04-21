# Login

테스트 데이터

`loginId -> test, username -> 테스터, password -> test!`

```java
public class Member{

    Long id;
    String loginId;
    String username;
    String password;

    ...Getter, Setter, Constructor
}
```

Repository, template등 생략

`로그인 검증 구현`

```java
public class MemebrService{

    ...

    // 로그인 검증 로직
    public Member login(String loginId, String password){
        return memberRepository.findByLoginId(loginId)
                .filter(m -> m.getPassword().equals(password))
                .orElse(null);
    }

    ...
}
```

### 쿠키를 이용한 로그인 구현

회원이 로그인 시도한 아이디와 비밀번호로 repository에서 member를 조회한 다음 비빌번호가 일치한지 확인한다. 비밀번호가 일치할 경우 member를 반환하고, 그렇지 않을 경우 null을 반환한다.

```java
public class MemberController{

    ...
    //로그인 메소드 내부
    Member loginMember = memberService.login(loginForm.getLoginId(), loginForm.getPassword());

    if(loginMember == null){
        bindingResult.reject("loginFail","아이디 또는 비밀번호가 잘못 되었습니다.");
        return "loginForm";
    }

    Cookie cookie = new Cookie("memberId", String.valueOf(loginMember.getId());
    response.addCookie(cookie);

    return "redirect:/";
}
```

로그인 검증 로직을 통해서 null이 반환되었을 경우, 회원의 아이디 혹은 비밀번호가 잘못되었기때문에 ObjectError를 생성하여 다시 로그인 페이지로 돌아가 사용자에게 로그인에 실패했다는 오류메세지를 보여준다.

로그인에 성공했을경우 회원의 식별자 값을 쿠키로 생성하여 브라우저 전달해준다.
로그인 상태를 유지하기위해 쿼리 파라미터를 유지하고 모든 요청에 사용자에 정보를 포함하는 것은 비 효율적이고 번거롭다. 그래서 쿠키를 통해서 로그인 상태를 유지할 수 있다.

![Cookie](https://user-images.githubusercontent.com/78605779/164481906-492035c9-7ce8-43cc-a6ed-37fc765b2006.png)

```
해당 예시에서는 만료 날짜를 지정하지 않았기 때문에 브라우저가 종료될 때 삭제되는 세션 쿠키
```

```java
//로그아웃 메소드
public String logout(HttpServletResponse response){
    Cookie cookie = new Cookie("memberId", null);
    cookie.setMaxAge(0);
    reponse.addCookie(cookie);

    return "redirect:/";
}
```

사용자가 로그아웃을 하면 동일한 이름을 가진 쿠키를 생성한 후 유효 기간(Max-Age)를 0으로 설정한다. 클라이언트는 동일한 이름의 다른 값을 가진 쿠키가 들어오면 해당 쿠키로 교체를한다. 이를 통해 기존 쿠키를 유효 기간이 0인 쿠키로 교체하여 해당 쿠키를 삭제한다.

<hr/>

## 학습 자료

[인프런)스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2/dashboard)
