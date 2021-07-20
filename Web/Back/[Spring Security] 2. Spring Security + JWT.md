# Spring Security + JWT

> 스프링의 보안 프레임워크의 표준 보안 필터의 사용자 인증, 인가 방식을 Session이 아닌 **JWT(Json Web Token)**을 이용



## Gradle 의존성 추가

```
dependencies{
	// ... 생략
	// for spring security + jwt
	compile 'org.springframework.boot:spring-boot-starter-thymeleaf'
	compile 'org.springframework.boot:spring-boot-starter-security'
    compile group: 'io.jsonwebtoken', name: 'jjwt', version: '0.7.0'
}
```



## UserDto

> SpringSecurity는 UserDetails 객체를 통해 권한 정보를 관리하기 때문에 User 클래스에 UserDetails 를 구현하고 추가 정보를 재정의 해야 함
>
> Entity와 UserDetails는 구분할 수도, 같은 클래스에서 관리할 수도 있음 (현재는 같은 클래스에서 관리하는 방법을 사용)

```java
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
public class User implements UserDetails {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 100, nullable = false, unique = true)
    private String email;

    @Column(length = 30, nullable = false)
    private String password;

    @ElementCollection(fetch = FetchType.EAGER)
    @Builder.Default
    private List<String> roles = new ArrayList<>();

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return this.roles.stream()
                .map(SimpleGrantedAuthority::new)
                .collect(Collectors.toList());
    }

    @Override
    public String getUsername() {
        // getUsername을 통해 spring security에서 사용하는 username을 가져감
        // 현재 사용할 username은 email -> 추후 uid로 변경
        return email;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```



## SecurityConfig

Spring Security Filter Chain을 사용한다는 것을 명시

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    //
    @Override
    protected void configure(HttpSecurity http) throws Exception {
    	http
                .httpBasic().disable() // rest api 만을 고려하여 기본 설정은 해제
                .csrf().disable() // csrf 보안 토큰 disable처리.
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) // 토큰 기반 인증이므로 세션 사용하지 않음
                .and()
                .authorizeRequests() // 요청에 대한 사용권한 체크
                .antMatchers("/admin/**").hasRole("ADMIN")
                .antMatchers("/user/**").hasRole("USER")
                .anyRequest().permitAll() // 그외 나머지 요청은 누구나 접근 가능
                .and()
                .addFilterBefore(new JwtAuthenticationFilter(jwtTokenProvider),
                        UsernamePasswordAuthenticationFilter.class);
                // JwtAuthenticationFilter를 UsernamePasswordAuthenticationFilter 전에 넣는다
    }
}
```



## JWT 생성 및 검증

```

```



## JWTAUthenticationfilter

실제 인증작업을 진행하는 필터로, 검증이 끝난 JWT로부터 유저정보를 받아와서 UsernamePasswordAuthenticationFilter 로 전달

> 아무래도 이게 JWTInterceptor인 듯?!

```java
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends GenericFilterBean {
	 private final JwtTokenProvider jwtTokenProvider;

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        // 헤더에서 JWT 를 받아옵니다.
        String token = jwtTokenProvider.resolveToken((HttpServletRequest) request);
        // 유효한 토큰인지 확인합니다.
        if (token != null && jwtTokenProvider.validateToken(token)) {
            // 토큰이 유효하면 토큰으로부터 유저 정보를 받아옵니다.
            Authentication authentication = jwtTokenProvider.getAuthentication(token);
            // SecurityContext 에 Authentication 객체를 저장합니다.
            SecurityContextHolder.getContext().setAuthentication(authentication);
        }
        chain.doFilter(request, response);
    }
}
```



## CustomUserDetailService

토큰에 저장된 유저 정보를 활용: UserDetailsService를 상속받아 재정의
추후에 해당 내용으로 UsernamePasswordAuthenticationToken 확인

```java
@RequiredArgsConstructor
@Service
public class CustomUserDetailService implements UserDetailsService {
	
    private final UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        return userRepository.findById(Integer.parseInt(username));
    }
}
```

```java
@RequiredArgsConstructor
@Component
public class JwtTokenProvider {
    // ...생략...
    
	// JWT 토큰에서 인증 정보 조회
    public Authentication getAuthentication(String token) {
        UserDetails userDetails = userDetailsService.loadUserByUsername(this.getUserPk(token));
        return new UsernamePasswordAuthenticationToken(userDetails, "", userDetails.getAuthorities());
    }
    
    // ...생략...
}
```





## UserController

```java
@RequiredArgsConstructor
@RestController
public class UserController {
    // 회원가입
    
    
    // 로그인
    @PostMapping("/login")
    public String login(@RequestBody Map<String, String> user) {
        User member = userRepository.findByEmail(user.get("email"))
                .orElseThrow(() -> new IllegalArgumentException("가입되지 않은 E-MAIL 입니다."));
        if (!passwordEncoder.matches(user.get("password"), member.getPassword())) {
            throw new IllegalArgumentException("잘못된 비밀번호입니다.");
        }
        return jwtTokenProvider.createToken(member.getUsername(), member.getRoles());
    }
}
```



## 실행

1. /login으로 POST 요청

2. Token 생성 후 반환

3. 돌려 받은 Token을 header - 'X-AUTH-TOKEN'에 담아 제한된 리소스에 대한 요청

4. Token을 통해 권한을 확인한 후 리소스를 반환

   ```java
   	// JwtTokenProvider.class
   	// Request의 Header에서 token 값을 가져옵니다. "X-AUTH-TOKEN" : "TOKEN값'
   	public String resolveToken(HttpServletRequest request) {
           return request.getHeader("X-AUTH-TOKEN");
       }
   ```

   



<참고>

* http://www.incodom.kr/Spring_security_with_JWT