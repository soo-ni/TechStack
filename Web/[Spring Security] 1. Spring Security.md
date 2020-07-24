# Spring Security

> 스프링 시큐리티는 스프링 기반의 애플리케이션의 보안(인증과 권한,인가 등)을 담당하는 스프링 하위 프레임워크이다. 주로 서블릿 필터와 이들로 구성된 필터체인으로의 위임모델을 사용한다. 그리고 보안과 관련해서 체계적으로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안관련 로직을 작성하지 않아도 된다.



## Authentication & Authorization

### Authentication (인증)

보호된 리소스에 접근한 대상에 대해 이 유저가 누구인지, 애플리케이션의 작업을 수행해도 되는 주체인지 확인하는 과정

> **크리덴셜(Credential:자격) 기반 인증**
>
> 우리가 웹에서 사용하는 대부분의인증 방식은 크리덴션 기반의 인증 방식입니다. 즉 권한을 부여받는데 1차례의 인증과정이 필요하며 대개 사용자명과 비밀번호를 입력받아 입력한 비밀번호가 저장된 비밀번호와 일치하는지 확인합니다. 일반적으로 스프링 시큐리티에서는 **아이디를 프린시플(principle)**, **비밀번호를 크리덴셜(credential)**이라고 부르기도 합니다.
>
> **TIP**
>
> 접근 주체(Principal) : 보호된 리소스에 접근하는 대상



### Authorization (권한)

해당 리소스에 대해 접근 가능한 권한을 가지고 있는지 확인하는 과정(After Authentication, 인증 이후)

어떠한 리소스에 대한 접근 제한, 모든 리소스는 접근 제어 권한이 걸려있다. 즉, 인가 과정에서 해당 리소스에 대한 제한된 최소한의 권한을 가졌는지 확인한다.

> **부여된 권한(Granted Autority) ** 
>
> 적절한 절차로 사용자가 인증되었다면 권한을 부여(Granted Authority)해야 할 것입니다. 회원가입 등을 통해 반영구적인 권한이 부여됬다면 우리는 이 회원에게 부여된 권한을 어딘가에 저장해야 하구요. 만약 해당 사용자가 로그인을 했는데 메인 페이지로 넘어갈 수 없다면 권한부여에 문제가 있다는 것이겠죠.

> **리소스의 권한(Intercept)** 
>
> 사용자의 권한만 있다고 보안이 제대로 동작할리는 없습니다. 보안이란 본래 권한이 없는 자들이 원천적으로 리소스에 접근할 수 없도록 막아내는 것이기 때문입니다. 그런 의미에서 적절한 권한을 가진자만 해당 자원에 접근할 수 있도록 자원의 외부요청을 원천적으로 가로채는 것(Intercept)이 웹보안, 그 중 권한부여(Authorization)의 핵심 원칙이라 할 수 있겠습니다.



## 1. Authentication Architecture

<img src="https://blog.kakaocdn.net/dn/kSN2h/btquDLF3geH/umf8frkg8cc9pRDq9TgUz1/img.png">



1. 사용자가 Form을 통해 로그인 정보를 입력 후 인증 요청을 보냄
2. **AuthenticationFilter**가 HttpServletRequest에서 사용자가 보낸 아이디와 패스워드를 인터셉트함HttpServletRequest에서 꺼내온 사용자 아이디와 패스워드를 진짜 인증을 담당할 **AuthenticationManager** 인터페이스에게 인증용 객체(UsernamePasswordAuthenticationToken)를 만들어준 후 위임
3. AuthenticationFilter에게 인증용 객체(UsernamePasswordAuthenticationToken)을 전달받음
4. 실제 인증을 할 **AuthenticationProvider**에게 Authentication 객체(UsernamePasswordAuthenticationToken)을 다시 전달
5. DB에서 사용자 인증 정보를 가져올 **UserDetailsService** 객체에게 사용자 아이디를 넘겨줌
   DB에서 인증에 사용할 사용자 정보(사용자 아이디, 암호화된 패스워드, 권한 등)를 UserDetails(인증용 객체와 도메인 객체를 분리하지 않기 위해서 실제 사용되는 도메인 객체에 UserDetails를 상속하기도 한다.)라는 객체로 전달 받음
6. AuthenticationProvider는 UserDetails 객체를 전달 받은 이후 실제 사용자의 입력정보와 UserDetails 객체를 가지고 인증을 시도
7. 8. 9. 10. 인증이 완료되면 사용자 정보를 가진 **Authentication** 객체를 **SecurityContextHolder**에 담은 이후 **AuthenticationSuccessHandle**를 실행(실패시 AuthenticationFailureHandler를 실행한다.)



## 2. Security Filters

<img src='https://blog.kakaocdn.net/dn/q9x6Q/btquDWtOWaA/SKHXuyh5eneH5Jn29HWUk0/img.png'>



* SecurityContextPersistenceFilter
  : SecurityContextRepository에서 SecurityContext를 가져오거나 저장하는 역할
* LogoutFilter
  : 설정된 로그아웃 URL로 오는 요청을 감시하며, 해당 유저를 로그아웃 처리
* (UsernamePassword)**AuthenticationFilter**
  : 아이디와 비밀번호를 사용하는 form기반 인증/설정된 로그인 URL로 오는 요청을 감시하며, 유저 인증 처리
  * **AuthenticationManager를 통한 인증 실행**
  * 인증 성공 시, 얻은 Authentication 객체를 SecurityContext에 저장 후 AuthenticationSuccessHandler 실행
  * 인증 실패 시, AuthenticationFailureHandler 실행
* DefaultLoginPageGeneratingFilter
  : 인증을 위한 로그인폼 URL을 감시
* BasicAuthenticationFilter
  : HTTP 기본 인증 헤더를 감시하여 처리
* RequestCacheAwareFilter
  : 로그인 성공 후, 원래 요청 정보를 재구성하기 위해 사용
* SecurityContextHolderAwareRequestFilter
  : HttpServletRequestWrapper를 상속한 SecurityContextHolderAwareRequestWapper 클래스로 HttpServletRequest 정보를 Wrapping
  : SecurityContextHolderAwareRequestWrapper 클래스는 필터 체인상의 다음 필터들에게 부가정보를 제공
* AnonymousAuthenticationFilter
  : 이 필터가 호출되는 시점까지 사용자 정보가 인증되지 않았다면 인증토큰에 사용자가 익명 사용자로 표시
* SessionManagementFilter
  : 인증된 사용자와 관련된 모든 세션을 추적
* ExceptionTranslationFilter
  : 보호된 요청을 처리하는 중에 발생할 수 있는 예외를 위임하거나 전달하는 역할
* FilterSecurityInterceptor
  : AccessDecisionManager 로 권한부여 처리를 위임함으로써 접근 제어 결정을 쉽게해줌 (앞에 지나온 모든 필터들의 정보를 토대로 최종 결정을 내린다.)



<img src="https://jungeunlee95.github.io/assets/post-img/java/1563347991695.png">





## 3. Authentication

접근 주체는 Authentication 객체 생성, 해당 객체는 SecurityContext(내부 메모리)에 보관되고 사용됨

```java
public interface Authentication extends Principal, Serializable { 
    Collection<? extends GrantedAuthority> getAuthorities(); // Authentication 저장소에 의해 인증된 사용자의 권한 목록 
    Object getCredentials(); // 주로 비밀번호 
    Object getDetails(); // 사용자 상세정보 
    Object getPrincipal(); // 주로 ID 
    boolean isAuthenticated(); //인증 여부 
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException; 
}

// 출처: https://coding-start.tistory.com/153 [코딩스타트]
```



## 4. AuthenticationManager

<img src="https://blog.kakaocdn.net/dn/nUewF/btquCKBmNlf/vcbEydktTLvDBTaUoK2Sr1/img.png">



> 요청에 담긴 Authentication을
> -> AuthenticationManager에 넘겨주면
> -> AuthenticationManager를 구현한 ProviderManager가 처리
> -> AuthenticationManager가 AuthenticationProvider(多)를 통해
> -> UserDetailsService를 거쳐 인증을 받아
> -> UserDetails에 SercurityUser를 등록
>
> 유저의 요청을 AuthenticationFilter에서 Authentication 객체로 변환해 AuthenticationManager(ProviderManager)에게 넘겨주고, AuthenticationProvider(DaoAuthenticationProvider)가 실제 인증을 한 이후에 인증이 완료되면 Authentication객체를 반환
>
> Spring Security 는 ProviderManager 라는 AuthenticationManager 인터페이스의 유일한 구현체를 제공
>
> ProviderManager 는 하나 또는 여러 개의 AuthenticationProvider 구현체를 사용 가능, AuthenticationProvider는 많이 사용되고 ProviderManager(AuthenticationManager 의 구현체) 와도 잘 통합되기 때문에 기본적으로 어떻게 동작하는 지 이해하는 것이 중요

* AbstractAuthenticationProcessingFilter 
  : 웹 기반 인증요청에서 사용되는 컴포넌트로, POST 폼 데이터를 포함하는 요청을 처리
  : 사용자 비밀번호를 다른 필터로 전달하기 위해서 Authentication 객체를 생성하고 일부 프로퍼티를 설정
* AuthenticationManager 
  : 인증요청을 받고 Authentication을 채워줌
* AuthenticationProvider 
  : 실제 인증이 일어나고 만약 인증 성공시 Authentication 객체의 authenticated = true로 설정



## 5. Password Authentication

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F3TmlA%2FbtquDMdTUT2%2FzkiHWi9JUfPNtKXJuutTlK%2Fimg.png">



>DaoAuthenticationProvider 는 UserDetailsService 타입 오브젝트로 위임한다. 
>
>UserDetailsService 는 UserDetails 구현체를 리턴하는 역할을 한다. UserDetails 인터페이스는 이전에 설명한 Authentication 인터페이스와 상당히 유사하지만 서로 다른 목적을 가진 인터페이스이므로 헷갈리면 안된다. 

* Authentication : 사용자 ID, 패스워드와 인증 요청 컨텍스트에 대한 정보를 가지고 있다. 인증 이후의 사용자 상세정보와 같은 UserDetails 타입 오브젝트를 포함할 수도 있다. 
* UserDetails : 이름, 이메일, 전화번호와 같은 사용자 프로파일 정보를 저장하기 위한 용도로 사용한다.



## 6. Authentication Exception

인증과 관련된 모든 예외는 AuthenticationException을 상속

* Member feild

  * authentication
    : 인증 요청관련 Authentication 객체를 저장
  * extraInformation
    : 인증 예외 관련 부가 정보를 저장
    : 예를 들어 UsernameNotFoundException 예외는 인증에 실패한 유저의 id 정보를 저장

* 대표적 예외들

  * BadCredentialsException 
    : 사용자 아이디가 전달되지 않았거나 인증 저장소의 사용자 id 에 해당하는 패스워드가 일치하지 않을 경우 발생

  * LockedException
    : 사용자 계정이 잠긴경우 발생

  * UsernameNotFoundException
    : 인증 저장소에서 사용자 ID를 찾을 수 없거나 사용자 ID에 부여된 권한이 없을 경우 발생

    

## 7. Authorization

**FilterSecurityInterceptor**
: Spring Security 필터 체인의 마지막 서블릿 필터로, 해당 요청의 수락 여부를 결정
: 이 필터가 실행되는 시점에는 이미 사용자가 인증되어 있어 유효한 사용자인지 알 수 있음
: Authentication 인터페이스에는 List<GrantedAuthority> getAuthorities() 라는 메소드가 있다는 것을 상기 - 이 메소드는 사용자 아이디에 대한 권한 목록을 리턴한다. 권한처리시에 이 메소드가 제공하는 권한정보를 참조해서 해당 요청의 승인 여부를 결정

**Access Decision Manager** 
: 인증 확인을 처리
: AccessDecisionManager 인터페이스는 인증 확인을 위해 두 가지 메소드를 제공

- supports 
  : AccessDecisionManager 구현체는 현재 요청을 지원하는지의 여부를 판단하는 두개의 메소드를 제공
  : 하나는 java.lang.Class 타입을 파라미터로 받고 다른 하나는 ConfigAttribute 타입을 파라미터로 받음
- decide 
  : 요청 컨텍스트와 보안 설정을 참조하여 접근 승인여부를 결정
  : 리턴값이 없지만 접근 거부를 의미하는 예외를 던져 요청이 거부되었음을 알려줌

인증과정에서 발생할 수 있는 예상 가능한 에러를 처리하는 AuthenticationException 과 하위 클래스를 사용했던 것처럼 특정 타입의 예외 클래스들을 사용하면 권한처리를 하는 애플리케이션의 동작을 좀더 세밀하게 제어 가능

**AccessDecisionManager**
: 표준 스프링 빈 바인딩과 레퍼런스로 완벽히 설정
: 디폴트 AccessDecisionManager 구현체는 AccessDecisionVoter 와 Vote 취합기반 접근 승인 방식을 제공

Voter 는 권한처리 과정에서 다음 중 하나 또는 전체를 평가

- 보호된 리소스에 대한 요청 컨텍스트 (URL 을 접근하는 IP 주소)
- 사용자가 입력한 비밀번호
- 접근하려는 리소스
- 시스템에 설정된 파라미터와 리소스

**AccessDecisionManager** 
: 요청된 리소스에 대한 access 어트리뷰트 설정을 보터에게 전달하는 역할도 하므로 보터는 웹 URL 관련 access 어트리뷰트 설정 정보를 가짐

**Voter**
: 사용할 수 있는 정보를 사용해서 사용자의 리소스에 대한 접근 허가 여부를 판단
: 보터는 접근 허가 여부에 대해서 세 가지 중 한 가지로 결정하는데, 각 결정은 AccessDecisionVoter 인터페이스에 다음과 같이 상수로 정의

- **Grant(ACCESS_GRANTED)** : Voter 가 리소스에 대한 접근 권한을 허가하도록 권장
- **Deny(ACCESS_DENIED)** : Voter 가 리소스에 대한 접근 권한을 거부하도록 권장
- **Abstain(ACCESS_ABSTAIN)** : Voter 는 리소스에 대한 접근권한 결정을 보류한다. 이 결정 보류는 다음과 같은 경우에 발생
  - Voter 가 접근권한 판단에 필요한 결정적인 정보를 가지고 있지 않은 경우
  - Voter 가 해당 타입의 요청에 대해 결정을 내릴 수 없는 경우





















<참고>

* http://springmvc.egloos.com/504862
* https://coding-start.tistory.com/153
* https://jungeunlee95.github.io/java/2019/07/17/2-Spring-Security/