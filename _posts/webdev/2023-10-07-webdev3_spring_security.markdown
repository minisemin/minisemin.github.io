---
layout: post
title:  "풀스택 웹개발(3) - Spring Security"
date:   2023-10-07 12:40:48 +0900
categories: webdev
---


## 소개
[풀스택 웹개발] 시리즈는 Spring을 활용하여 풀스택 웹사이트를 만드는 것을 목표로 합니다. 빠르고 직관적인 이해를 돕기 위해 전문적인 용어보다는 비유적인 표현을 많이 사용했습니다. 웹사이트를 처음 만들어보는 분도 쉽게 배우고 따라하는 웹개발 설명서가 되었으면 합니다.

본 시리즈가 다루는 주제는 크게 다음과 같습니다:
> - [API, REST API](#https://minisemin.github.io/webdev/2023/09/09/webdev1.html)
> - [Spring Framework](#https://minisemin.github.io/webdev/2023/09/14/webdev2.html)
> - [Spring Sequrity](#https://minisemin.github.io/webdev/2023/09/14/webdev3_spring_security.html)
> - RDBMS 중급 활용
> - AWS 클라우드 컴퓨팅
> - 웹서비스 배포
> - 마이크로서비스 아키텍처

다음과 같은 내용을 학습하시면 본 시리즈를 이해하는 데 도움이 됩니다:
> - 객체지향(Opject-oriented-programming)
> - Java
> - SQL

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### Table of Contents
---
1. [Introduction](#소개)
2. [Spring Sequrity](#spring-security)
3. [Authentication & AUthorization](#authentication--authorization)
4. [Architecture](#architecture)
5. [DTO](#dto)

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## Spring Security

Spring Sequirty는 Spring이 자체적으로 제공하는 보안 프레임워크입니다.

주요 기능은 다음과 같습니다:

- 인증(Authentication),
- 권한 부여(Authoprization),
- 세션 관리(Session Management),
- CSRF(Cross-Site Request Forgery)로부터의 보호,
- CORS(Cross-Origin Resource Sharing) 지원,
- LDAP(Lightweight Directory Access Protocol) 통합,
- OAuth 및 OpenID Connect 지원

Spring Security는 확장성이 뛰어나며, 맞춤 설정이 가능하다는 장점이 있습니다. 스프링 애플리케이션을 만든다면 스프링 보안 설정을 사용하시는 것을 강력 추천드립니다.

&nbsp;

## Authentication & Authorization

본격적으로 시작하기 전에 웹 애프리케이션은 어떤 보안을 신경써야 할까요? 가장 먼저 인증(Authentication)과 권한부여(Authorization)가 있습니다. "현재 접근하려는 사용자가 내가 알고 있는 사용자인가?" "해당 사용자가 이 데이터를 요청할 권한이 있는가?" 확인하는 과정이 필요합니다.

즉, Spring은 사용자가 인증(Authenticated)되면 요청한 자원에 접근이 가능한 지 확인(Authorize)하는 절차를 거칩니다.

> Authentication (인증) --> 성공 --> Authorization (인가)

가장 익숙한 Authentication 과정은 아이디와 비밀번호를 입력해 로그인하는 과정입니다. 이때 리소스에 접근하는 유저는 접근 주체(Principal)이라고 부르며, 비밀번호(Credential)를 기반으로 인증이 일어납니다.


&nbsp;

### Architecture

다음으로 Spring security의 작동 구조를 살펴보겠습니다. Spring Security는 `SecurityFilterChain`이라는 연속적인 필터를 정보가 통과하는 양상을 보입니다. 공기청정기의 필터 흐름과 비슷하게 생각할 수 있습니다.

![공기청정기](https://s3.amazonaws.com/airfilterbuy-content-service/748beeFilterPurposePhoto1.jpg)

> ID와 PW가 담긴 인증 객체 생성 --> 인증 요구 --> 데이터베이스 조회 --> 비밀번호 확인 --> 인증 성공 --> 인증 객체 세션에 저장

&nbsp;

![Spring Security Architecture](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSvk8p%2FbtqEIKlEbTZ%2FvXKzokudAYZT9kRGXNHJe1%2Fimg.png)

1. 사용자가 HTTP Request, 즉 인증 요청을 보냅니다
2. `Authentication Filter`가 인증 요청을 가로챈 뒤, `UsernamePasswordAuthenticationToken`을 생성하여 `AuthenticationManager`에게 전달합니다.
    - `UsernamePasswordAuthenticationToken`은 `AbstractAuthenticationToken`을 extend하며, Principal(UserID)과 Credential(비밀번호)를 이용해 인증 전의 개체와 인증이 완료된 개체를 생성합니다.
        <details>
        <summary>UsernamePasswordAuthenticationToken</summary>

        ```Java
        public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {

            private final Object principal; // UserID
            private Object credentials; // Password

            // Pre-authentication object
            public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
                super(null);
                this.principal = principal;
                this.credentials = credentials;
                setAuthenticated(false);
            }

            // Post-authentication object
            public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
                    Collection<? extends GrantedAuthority> authorities) {
                super(authorities);
                this.principal = principal;
                this.credentials = credentials;
                super.setAuthenticated(true);
            }
        }
        public abstract class AbstractAuthenticationToken implements Authentication, CredentialsContainer {
        }

        ```

        </details>
3. `AuthenticationManager`는 `AuthenticationProvider`를 조회하며 인증을 요구합니다.
4. `AuthenticationProvidor`는 `UserDetailService`를 통해 데이터베이스에 접근하여 아이디와 사용자 정보를 조회합니다.
5. 비밀번호를 암호화하여 데이터베이스에 있는 암호와 매칭되는 경우 인증에 성공하고, `UsernameAuthenticationToken`을 생성하여 `AuthenticationManager`에게 돌려줍니다.
6. `AuthenticationManager`는 `UsernameAuthenticationToken`을 `AuthenticationFilter`로 전달합니다.
7. `AuthenticationFilter`는 `UsernameAuthenticationToken`을 `LoginSuccessHandler`로 전송하고, `SecurityContextHolder`에 저장합니다.

사용자의 인증정보는 Authentication 객체로 `SecurityContext`에 보관되며, 세션-쿠키 기반 인증 방식에 사용됩니다.



### Registration

## *DTO*

DTO(Data Trasfer Object)는 Spring의 계층 간 데이터를 교환하기 위해 사용하는 객체입니다. 유저 객체를 통채로 백엔드에 주고받으면 너무 많은 정보가 한꺼번에 옮겨다니기 때문에 보안상 문제가 발생할 수 있습니다. 이를 방지하기 위해 제한된 정보만 DTO 객체로 새롭게 정의하면 보안을 강화하며 네트워크 사용을 최적화할 수 있습니다.


### DTO 사용 예시

예를 들어, User에게 ID, Password, 나이, 성별, 이름, 이메일, 주소, 등등 많은 정보가 있는 상황에서, 아이디와 이름, 이메일 정보만 필요한 상황이 있을 수도 있습니다. 그럼 User라는 겍체에서 UserDto를 따로 정의하여 User객체 대신 사용할 수 있게 됩니다.

```java

/***
 * User --- controller
 *      |-- domain
 *      |-- dto          <-- 이 폴더입니다
 *      |-- repository
 *      ㄴ-- service
***/

public record UserDto (
    private UUID userId,
    private String email,
    private String name
){
    // standard getters and setters
}

```

유저가 회원가입을 할 때 이메일과 이름만 사용한다고 해봅시다. 그러면 간단한 회원가입 로직은 다음과 같습니다:


```java

/***
 * User --- controller    <-- 이 폴더입니다
 *      |-- domain
 *      |-- dto
 *      |-- repository
 *      ㄴ-- service
***/

@PostMapping("/register")
public ResponseEntity<Optional<User>> registerUser(@RequestBody UserDto userDto) {

    UUID userId = userDto.userId(); // 이런 식으로 DTO에서 정보를 추출할 수 있습니다.
    String email = userDto.email();
    String name = userDto.name();

    // AddUser DTO를 따로 만들어서(1차 가공) 새로운 DTO를 전달할 수도 있습니다.
    AddUserDto addUserDto = new AddUserDto(
        userId,
        email,
        name,
        // 가입일
        // 회원정보 만료 날짜 등
    )
    return ResponseEntity.ok(Optional.of(addUserDto));
}


```

## *Validiation*

@Valid를 사용하면 이 객체가 규칙을 준수하고 있는 지 확인할 수 있습니다.


## 이메일 주소가 올바른 지 확인

```java

public class EmailValidator
  implements ConstraintValidator<ValidEmail, String> {

    private Pattern pattern;
    private Matcher matcher;
    private static final String EMAIL_PATTERN = "^[_A-Za-z0-9-+]+
        (.[_A-Za-z0-9-]+)*@" + "[A-Za-z0-9-]+(.[A-Za-z0-9]+)*
        (.[A-Za-z]{2,})$";
    @Override
    public void initialize(ValidEmail constraintAnnotation) {
    }
    @Override
    public boolean isValid(String email, ConstraintValidatorContext context){
        return (validateEmail(email));
    }
    private boolean validateEmail(String email) {
        pattern = Pattern.compile(EMAIL_PATTERN);
        matcher = pattern.matcher(email);
        return matcher.matches();
    }
}


```

## 유저가 이미 존재하는 지 확인

```java

@PostMapping("/user/registration")
public ModelAndView registerUserAccount(
  @ModelAttribute("user") @Valid UserDto userDto,
  HttpServletRequest request,
  Errors errors) {

    try {
        User registered = userService.registerNewUserAccount(userDto);
    } catch (UserAlreadyExistException uaeEx) {
        mav.addObject("message", "User already exists.");
        return mav;
    }

    return new ModelAndView("successRegister", "user", userDto);
}

```

## 비밀번호가 올바른 지 확인

```java

public class PasswordMatchesValidator
  implements ConstraintValidator<PasswordMatches, Object> {

    @Override
    public void initialize(PasswordMatches constraintAnnotation) {
    }
    @Override
    public boolean isValid(Object obj, ConstraintValidatorContext context){
        UserDto user = (UserDto) obj;
        return user.getPassword().equals(user.getMatchingPassword());
    }
}

```


&nbsp;
&nbsp;


---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### 자료 출처
- YC Tech Academy 4주차
- https://mangkyu.tistory.com/76 [MangKyu's Diary:티스토리]
- https://toosalty00.hashnode.dev/pre-w4-spring-security#heading-registration-with-spring-security
- https://webfirewood.tistory.com/115
- https://midnight-icon-758.notion.site/Week5-Spring-Security-Registration-fdbd3af65e174e1f8945c0346a20120d