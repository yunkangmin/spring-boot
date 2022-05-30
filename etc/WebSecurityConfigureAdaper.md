+WebSecurityConfigurerAdapter
로그인이란 뭘까?
로그인이란 대충 이런 느낌이다.
  1) 로그인하기 전에는 로그인 페이지로 리다이렉트
  2) 사용자에게 아이디와 비밀번호를 받는다.
  3) 입력받은 값과 DB의 값을 비교해서 로그인 처리를 한다.

위의 사항들을 모두 Spring Boot와 Spring Security의 WebSecurityConfigurerAdapter를 통해서 간단히 구현할 수 있다.

0. WebSecurityConfigureAdapter만들기

build.gradle
dependencies {
   compile group: 'org.springframework.boot', name: 'spring-boot-starter-security, version: '2.1.8.RELEASE'
}

SecurityConfig.java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter

1. 로그인하기 전에는 로그인 페이지로 리다이렉트
WebSecurityConfigurerAdapter 안에는 쓸만한 메서드들이 많다.
그 중에서 먼저 void configure(HttepSecurity http)를 적당히 @Override 해보겠다.

SecurityConfig.java
@Override
public void configure(HttpSecurity security) throws Exception {
   security
    .antMatchers("/admin/*").authenticated() -> 로그인이 필요한 경로를 정의
    //formLogin()을 통해 로그인 페이지와 로그인 성공시 보내줄 경로를 정의
    .and().formLogin().loginPage("/login").defaultSuccessUrl("/admin/main", true); 
}

2. 사용자에게 아이디와 비밀번호를 받는다.
@GetMapping("/login")을 통해 적당히 로그인 페이지를 만든다.

login.jsp
<form method="post">
  <input type="text" name="username" />
  <input type="password" name="password" />
</form>

여기서 포인트는 <form />에 action attribute가 없다는 점이다.
Postback으로 구현되어 있기 때문이다.

3. 입력받은 값과 DB의 값을 비교해서 로그인 처리를 한다.
Post로 들어온 아이디와 비밀번호는 누가 처리할까?
UserDetailService와 PasswordEncoder가 처리하도록 정해져 있다.

그런데 이게 공식이 있다.
1. DB에서 아이디로 사용자 정보를 조회(=비밀번호 포함)
2. 입력받은 비밀번호를 인코딩한다.
3. 인코딩한 비밀번호와 사용자 정보의 비밀번호를 비교한다.

OperatorService.java
@Service
public class OperatorService implements UserDetailsService {
   @Override
   public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException
}

여기서 3-1번을 해주게된다.
loadUserByUsername()안에서 적당히 DB에서 조회하면 된다.
리턴해 줄 사용자 정보 객체는 UserDetails의 구현체인 User를 쓰거나 상속받으면 된다.
중요한 건 생성자에 정의되어 있는데 username과 password를 잘 넣어줘야 한다.


SecurityConfig.java
private class MyEncoder implements PasswordEncoder {
  @Override
  public String encode(CharSequence rawPassword)
  
  @Override
  public boolean matches(CharSequence rawPassword, String encodePassword)
  
}

여기서 각각 3-2번과 3-3번을 해준다.
위와 같이 PasswordEncoder를 직접 구현하지 말고 그냥
PasswordEncoderFactories에서 하나 꺼내 쓰거나
StandardPasswordEncoder등의 구현체를 사용하는 것도 좋다.

이제 두 클래스를 객체로 만들어 사용해보자.
다시 WebSecurityConfigureAdapter로 가보자.

SecurityConfig.java
@Autowired
OperatorService operatorService;

@Override 
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   auth
      .userDetailsService(operatorService)
      .passwordEncoder(new MyEncoder());
}

+추가적인 설명
실제로 동작하는 순서는 조금 다르다.
제일 먼저 스프링의 ApplicationContext가 만들어지는 과정에서
DefaultPasswordEncoderAuthenticationManagerBuilder, ProvicerManager,
DaoAuthenticaitonProvider, 그리고 서블릿 필터 UsernamePasswordAuthenticationFilter가 만들어진다.
WebSecurityConfigurerAdapter 덕분이다.

이후 사용자가 POST로 로그인을 시도하면 위의 서블릿 필터를 통해
ProviderManager.authenticate()가 호출되고
이어 DaoAuthenticationProvider의 retrieveUser()와 additionalAuthenticationChecks()를 통해
UserDetailsService.loadUserByUsername()과 PasswordEncoder.matched()가 호출되어 로그인 정보를 확인한다.

확인하여 이상이 없다면, 필터에서는 UsernamePasswordAuthenticationToken을 리턴받아 세션에 저장하고 
다시 successfulAuthentication()을 통해 SecurityContext에 저장하고
사용자를 로그인 완료 페이지로 리다이렉트 시켜준다.

---

# 정리
로그인이란 뭘까?   
로그인이란 대충 이런 느낌이다.
1. 로그인하기 전에는 로그인 페이지로 리다이렉트  
2. 사용자에게 아이디와 비밀번호를 받는다.  
3. 입력받은 값과 DB의 값을 비교해서 로그인 처리를 한다.   

위에 사항을 모두 Spring Boot와 Spring Security의 WebSecurityConfiguerer  
Adapter를 통해서 간단히 구현해볼 수 있다.  
하나씩 한 번 해보자.   

## 1. WebSecurityConfigureAdapter 만들기  
build.gradle
```
dependencies {
   compile group: 'org.springframework.boot', name: 'spring-boot-starter-  
   security', version: '2.1.8.RELEASE' 
}
```
SecurityConfig.java  
```
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter
```
### 1. 로그인하기 전에는 로그인 페이지로 리다이렉트    
WebSecurityConfigurerAdapter 안에는 쓸만한 메서드들이 많다. 그 중에서  
먼저 void configure(HttpSecurity http)를 적당히 @Override 해보겠다.   

SecurityConfig.java
```java
public void configure(HttpSecurity security) throws Exception {
   security.antMatchers("/admin/*").authenticated()
         .and().formLogin().loginPage("/login").defaultSuccessUrl("/admin/main", true);
}
```
`HttpSecurity`를 쓰려고 애를 `@Override` 했다.  
`.antMatchers()`를 통해 로그인이 필요한 url을 정의해주고,   
`formLogin()`을 통해 로그인 페이지와 로그인 성공 시 보내줄 url을 정의해준다.   

### 2. 사용자에게 아이디와 비밀번호를 받는다.   
`@GetMapping("/login")`을 통해 적당히 로그인 페이지를 만든다.   
login.jsp   
```java
<form method="post">
   <input type="text" name="username">
   <input type="password" name="password">
</form>
```
여기서 포인트는 `<form/>`에 action attribute가 없다는 점이다. 그러니까   
Postback으로 구현되어 있다. 

### 3. 입력받은 값과 DB의 값을 비교해서 로그인 처리를 한다.  
`POST`로 들어온 아이디와 비밀번호는 누가 처리할까?  
`UserDetailsService`와 `PasswordEncoder`가 처리하도록 정해져 있다.   
  
그런데 이게 애들만의 공식이 있어서 먼저 간단히 알아보자.  
1. DB에서 아이디로 사용자 정보를 조회한다(비밀번호 포함).  
2. 입력받은 비밀번호를 인코딩한다.   
3. 인코딩한 비밀번호와 사용자 정보의 비밀번호를 비교한다.   
OperatorService.java   
```
@Service
public class OperatorService implements UserDetailsService {
   @Override
   public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException
}
```
여기서 3-1번을 해주게된다. `loadUserByUsername()`안에 적당히 DB에서 조회  
하면 된다. 리턴해줄 사용자 정보 객체는 `UserDetails`의 구현체인 `User`를  
쓰거나 상속받으면 된다.  
보면 생성자에 정의되어 있다. 여기서 username과 password를 잘 넣어줘야 한다.  
  
SecurityConfig.java
```java
private class MyEncoder implements PasswordEncoder {
   @Override
   public String encode(CharSequence rawPassword) 

   @Override
   public boolean matches(CharSequence rawPassword, String encodePassword)
}
```
그리고 여기서 각각 3-2번과 3-3번을 해주게 된다.   
아니면 이렇게 `PasswordEncoder`를 직접 구현하지 말고 그냥  
`PasswordEncoderFactories`에서 하나 꺼내 쓰거나   
`StandardPasswordEncoder`등의 구현체를 사용하는 것도 좋다.   
  
이제 두 클래스를 객체로 만들어 잘 사용해보자.   
`WebSecurityConfigurerAdapter`로 다시 가보자.   

SecurityConfig.java  
```java
@Autowired
OperatorService operatorService;

@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   auth.userDetailService(operatorService)
       .passwordEncoder(new MyEncoder());
}
```

# 추가적인 설명  
사실 실제로 동작하는 순서는 조금 다르다.   
  
제일 먼저, 스프링의 `ApplicationContext`가 만들어지는 과정에서  
`DefaultPasswordEncoderAuthenticationManagerBuilder`, `ProviderMana  
ger`, `DaoAuthenticationProvider`, 그리고 서블릿 필터 `UsernamePass   
wordAuthenticationFilter`가 만들어진다. `WebSecurityConfigurerAdapter`  
덕분이다.  
  
이후 사용자가 `POST`로 로그인을 시도하면 위의 서블릿 필터를 통해 `Provider  
Manager.authenticate()`가 호출되고, 이어 `DaoAuthenticationProvider`의  
`retrieveUser()`와 `additionalAuthenticationChecks()`를 통해  
`UserDetailsService.loadUserByUsername()`과 `PasswordEncoder.matches()`  
가 호출되어 로그인 정보를 확인한다.  
  
확인하여 이상이 없다면, 필터에서는 `UsernamePasswordAuthenticationToken`  
을 리턴받아 세션에 저장하고, 다시 `successfulAuthentication()`을 통해   
`SecurityContext`에 저장하고, 사용자를 로그인 완료 페이지로 리다이렉트  
시켜준다. 

---

# 스프링 시큐리티 시작하기(기본세팅)  
## 1. dependency 추가  
- Gradle 이용  
```java
dependencies {
   compile 'org.springframework.security:spring-security-web:4.2.2.RELEASE'
   compile 'org.springframework.security:spring-security-config:4.2.2:RELEASE'
}
```
버전은 사용하는 프로젝트에 맞게 바꿔주면 된다.  

## 2. Java Configuration   
WebSecurityConfigurerAdapter를 상속받은 config 클래스에 @EnableWebSe  
curity 어노테이션을 달면 SpringSecurityFilterChain이 자동으로 포함된다.  
```java
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {}
```
작성한 뒤 configure를 오버라이딩하여 접근 권한을 작성한다.   
```java
@EnableWebSecurity //스프링 시큐리티 사용을 위한 어노테이션선언  
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

}

```

# Spring Security 커스텀 필터를 이용한 인증 구현 - 스프링시큐리티 설정  
## 버전정보  
- Spring Boot v2.1.2
- Spring Security v5.1.3

## Spring Security 설정   
스프링부트를 사용한다면 application.yml에서 설정도 가능하지만 몇 가지   
설정만 제공하고 모든 설정을 할 수 없으므로 Configuration Class에서 하는  
설정이 기본이라고 생각하면 된다.  

## Configuration Class 작성   
스프링 시큐리티 Configuraiton Class를 작성하기 위해서는 WebSecurity  
ConfigurerAdapter를 상속하여 클래스를 생성하고 @EnableWebSecurity   
어노테이션을 추가한다(@Configuration 어노테이션 대신).    
```java
@EnableWebSecurity
public class BrmsWebS
```

## 커스텀 필터 등록
스프링 시큐리티는 각각 역할에 맞는 필터들이 체인형태로 구성되서 순서에  
맞게 실행되는 구조로 동작한다. 사용자는 특정 기능의 필터를 생성하여 등록할  
수 있다. 인증을 처리하는 기본 필터 UsernamePasswordAuthenticationFilter  
대신 별도의 인증 로직을 가진 필터를 생성하고 사용하고 싶을 때 아래와 같이   
필터를 등록하고 사용한다.   
```java
@EnableWebSecurity
public class BrmsWebSecurityConfiguration extends WebSecurityConfigurerAdapter {
   @Override
   public void configure(HttpSecurity http) throws Exception {
      http.addFilterBefore(new CustomAuthenticationProcessingFilter("/login-process"), UsernamePasswordAuthenticationFilter.class);
   }
}
```
### addFilterBefore  
지정된 필터 앞에 커스텀 필터를 추가    
(UsernamePasswordAuthenticationFilter보다 먼저 실행된다)    

### addFilterAfter   
지정된 필터 뒤에 커스텀 필터를 추가   
(UsernamePasswordAuthenticationFilter 다음에 실행된다.)

### addFilterAt  
지정된 필터의 순서에 커스텀 필터가 추가된다.   
  
마치 지정된 필터 대신에 커스텀 필터를 추가하는 것처럼 메소드가 동작할 것  
같지만 실제로는 오버라이드되지는 않는다. 설명에는 지정된 필터의 순서와  
같은 자리에 커스텀 필터를 삽입한다고 되어 있고 오버라이드 되지 않는다고   
설명되어 있다. 하지만 직접 테스트 결과 지정된 필터보다 커스텀 필터가 먼저  
실행되었다. 결론적으로 어떤 메소드로 커스텀 필터를 추가하더라도 기존 필터  
가 오버라이드되는 메소드는 없다. 다만 커스텀 필터가 실행되고 인증이 완료  
되었기 때문에 UsernamePasswordAuthenticationFilter가 수행되면서 인증   
완료된 상태이면 인증 로직이 수행되지 않고 자연스럽게 통과하기 때문에   
마치 오버라이드된 것처럼 동작하는 것으로 착각할 수 있다.  

## 커스텀 필터에 설정을 추가   
커스텀 필터에 인증 매니저 및 성공 실패 핸들러를 추가적으로 등록 및 설정  
을 추가할 수 있다.   
```java
@EnableWebSecurity
public class BrmsWebSecurityConfiguration extends WebSecutiryConfigurerAdapter {

   ...

   @Bean
   public CustomAuthenticaitonProcessingFilter customAuthenticationProcessingFilter() {
      CustomAuthenticationProcessingFilter filter = new CustomAuthenticationProcessingFilter("/login-process");
      filter.setAuthenticationManager(new CustomAuthenticationManager());
      filter.setAuthenticationFailureHandler(new CustomAuthenticationFailureHandler("/login"));
      filter.setAuthenticationSuccessHandler(new SimpleUrlAuthenticationSuccessHandler("/"));
   }

   @Override
   public void configure(HttpSecurity http) throws Exception {
      http.addFilterBefore(customAuthenticaitonProcessingFilter(),
               UsernamePasswordAuthenticationFilter.class);
   }
}
```
### CustomAuthenticationProcessingFilter  
AbstractAuthenticationProcessingFilter 상속 및 구현하여 인증처리를   
담당 

```java
http.addFilterBefore(customAuthenticationProcessingFilter(),
         UsernamePasswordAuthenticationFilter.class);
```
스프링 시큐리티의 기본 인증 처리 담당 필터인 UsernamePasswordAuthenti  
cationFilter 앞에 커스텀 필터를 추가한다.   
UsernamePasswordAuthenticationFilter 필터는 Override되지 않고   
CustomAuthenticaitonProcessingFilter 인증 처리가 되면 자연스럽게   
로직을 통과하는 구조이다. 인증로직이 포함된 인증매니저 인증성공/실패  
처리 핸들러 등등 기타 추가 설정이나 기능을 대체하는 의존성을 주입할   
수 있다.  










---

# Postback  
- 동일한 웹 페이지를 다시 호출하는 것   
- 보통 웹이동을 하는 경우 데이터가 갱신되어 기존에 입력한 데이터가 사라짐   
- Post 방식으로 요청  




























