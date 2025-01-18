# Spring Security

#### 어플리케이션 보안 구성

1.  인증(Authentication) : 해당 사용자가 본인이 맞는지 확인하는 과정
2.  인가(Authorization) : 해당 사용자가 요청하는 지원을 실행할 수 있는 권한이 있는가 확인하는 과정

        Spring Security는 기본적으로 인증 절차를 거친 후 인가 절차를 진행
        인가 과정에서 해당 리소스에 접근 권한이 있는지 확인

Spring Security는 이러한 인증과 인가를 위해 **Principal**을 아이디로, **Credential**을 비밀번호로 사용하는 **Credential 기반의 인증 방식**을 사용 <br>

- Principle(접근 주체) : 보호 받는 Resource에 접근하는 대상
- Credential(비밀번호) : Resource에 접근하는 대상의 비밀번호

<br>

---

<br>

## Spring Security

**Spirng 기반의 어플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크**

- 보안과 관련하여 체계적으로 많은 옵션을 제공하주기 때문에 개발자 입장에서 보안 관련 로직을 작성하지 않아도 됨
- 인증과 권한 부분을 **Filter 흐름에 따라 처리**

<br>

### Spring Security 구조의 처리 과정

![](/Spring/images/SpringSecurity.png)

1. 사용자가 로그인 정보와 함께 인증 요청 (Http Request)
2. AuthenticationFilter가 요청을 가로채고, 가로챈 정보를 통해 UsernamePasswordAuthenticationToken의 인증용 객체 생성
3. AuthenticationManager의 구현체인 ProviderManager에게 생성한 UsernamePasswordToken 객체를 전달
4. AuthenticationManager는 등록된 AuthenticationProvider(들)을 조회하여 인증 요구
5. 실제 DB에 사용자 인증정보를 가져오는 UserDetailsService에 사용자 정보를 넘겨줌
6. 넘겨받은 사용자 정보를 통해 DB에서 찾은 사용자 정보인 UserDetails 객체를 만듦
7. AuthenticationProvider(들)은 UserDetails를 넘겨받고 사용자 정보를 비교
8. 인증이 완료되면 권한 등의 사용자 정보를 담은 Authentication 객체 반환
9. 다시 최초의 AuthenticationFilter에 Authentication 객체 반환
10. Authentication 객체를 SecurityContext에 저장

<br>

### Spring Security 모듈

#### Authentication

- 현재 접근하는 주체의 정보와 권한을 담은 인터페이스
- SecurityContext에 저장되며 SecurityContextHolder를 통해 SecurityContext에 접근하고, SecurityContext를 통해 Authentication에 접근

```java
public interface Authentication extends Principal, Serializable {
  // 현재 사용자의 권한 목록을 가져옴
  Collection<? extends GrantedAuthority> getAuthorities();

  // credentials(주로 비밀번호)을 가져옴
  Object getCredentials();    

  Object getDetails();

  // Principal 객체를 가져옴
  Object getPrincipal();

  // 인증 여부를 가져옴
  boolean isAuthenticated();    

  // 인증 여부를 설정함
  void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}
```

<br>

#### UsernamePasswordAuthenticationToken

- Authentication을 implements한 AbstractAuthenticationToken의 하위 클래스
- 유저의 ID가 Pricipal, Password가 Credential의 역할을 함
- 첫 번째 생성자는 인증 전 객체를 생성, 두 번째 생성자는 인증이 완료된 객체를 생성

```java
public abstract class AbstractAuthenticationToken implements Authentication, CredentialsContainer {
}

public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
  private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;

  // 주로 사용자의 ID에 해당
  private final Object principal;

  // 주로 사용자의 PW에 해당
  private Object credentials;

  // 인증 완료 전의 객체 생성
  public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
    super(null);
    this.principal = principal;
    this.credentials = credentials;
    setAuthenticated(false);
  }

  // 인증 완료 후의 객체 생성
  public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
            Collection<? extends GrantedAuthority> authorities) {
    super(authorities);
    this.principal = principal;
    this.credentials = credentials;
    super.setAuthenticated(true); // must use super, as we override
  }
}
```

<br>

#### AuthenticationManager

- 인증에 대한 부분은 AuthenticationManeger를 통해 처리하는데 실질적으로는 AuthenticationManager에 등록된 AuthenticationProvider에 의해 처리됨
- 인증에 성공하면 두 번째 생성자를 이용해 객체를 생성하여 SecurityContext에 저장

```java
public interface AuthenticationManager {
  Authentication authenticate(Authentication authentication) throws AuthenticationException;
}
```

<br>

#### AuthenticationProvider

- 실제 인증에 대한 부분 처리
- 인증 전의 Authentication 객체를 받아서 인증이 완료된 객체를 반환하는 역할
- 커스텀한 AuthenticationProvider 인터페이스를 구현하고 AuthenticationManeger에 등록하면 됨

```java
public interface AuthenticationProvider {
  Authentication authenticate(Authentication authentication) throws AuthenticationException;
  boolean supports(Class<?> authentication);
}
```

<br>

#### ProviderManager

- AuthenticationManager를 implements함
- AuthenticationProvider를 구성하는 목록을 갖음

```java
public class ProviderManager implements AuthenticationManager, MessageSourceAware, InitializingBean {	    
  public List<AuthenticationProvider> getProviders() {
    return this.providers;
  }        

  public Authentication authenticate(Authentication authentication) throws AuthenticationException {
    Class<? extends Authentication> toTest = authentication.getClass();
    AuthenticationException lastException = null;
    AuthenticationException parentException = null;
    Authentication result = null;
    Authentication parentResult = null;
    int currentPosition = 0;
    int size = this.providers.size();                

    // for문으로 모든 provider를 순회하여 처리하고 result가 나올때까지 반복한다.
    for (AuthenticationProvider provider : getProviders()) { ... }
  }
}
```

<br>

#### UserDetailsService

- UserDetails 객체를 반환하는 하나의 메소드만 가지고 있음
- 일반적으로 이를 implements한 클래스에 UserRepository를 주입받아 DB와 연결하여 처리

```java
public interface UserDetailsService {
  UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}
```

<br>

#### UserDetails

- 인증에 성공하면 생성됨
- Authentication 객체를 구현한 UserPasswordAuthenticationToken을 생성하기 위해 사용됨
- UserDetails를 implements하여 처리할 수 있음

```java
public interface UserDetails extends Serializable {
  // 권한 목록
  Collection<? extends GrantedAuthority> getAuthorities();

  String getPassword();
  String getUsername();

  // 계정 만료 여부
  boolean isAccountNonExpired();

  // 계정 잠김 여부
  boolean isAccountNonLocked();

  // 비밀번호 만료 여부
  boolean isCredentialsNonExpired();

  // 사용자 활성화 여부
  boolean isEnabled();
}
```

<br>

#### SecurityContextHolder

- 보안 주체의 세부 정보를 포함하여 응용프로그램의 현재 보안 컨텍스트에 대한 세부 정보가 저장 됨

<br>

#### SecurityContext

- Authentication을 보관하는 역할
- SecurityContext를 통해 Authentication을 저장하거나 꺼내올 수 있음

```java
SecurityContextHolder.getContext().setAuthentication(authentication);
SecurityContextHolder.getContext().getAuthentication(authentication);
```

<br>

#### GrantedAuthority

- 현재 사용자(Principal)가 가지고 있는 권한을 의미
- `ROLE_ADMIN`이나 `ROLE_USER`와 같이 `ROLD_*` 형태로 사용
- UserDetailsService에 의해 불러올 수 있고, 특정 자원에 대한 권한이 있는지 검사하여 접근 허용 여부를 결정

<br>
<br>
<br>
<br>
https://dev-coco.tistory.com/174
