## 정리
1. @EnableConfigurationProperties가 하는 일은  
@ConfigurationProperties를 적용한 클래스를 빈으로 등록하고 프로퍼티 값을 할당한다.  
(@ConstructorBinding을 사용하지 않았을때)  
2. @ConstructorBinding은 생성자를 통해 프로퍼티 값을 클래스에 할당하는 것이고    
@ConfigurationProperties는 setter를 통해 값을 할당한다.   
@ConstructorBinding을 사용하는 이유는 생성자를 통해 객체 생성시 값을 할당함으로써  
생성 이후 값을 변경하지 못하게 하기 위한 것 같다.
3. @ConfigurationProperties를 사용할 때 세터를 쓰거나 @ConstructorBinding을 쓸 수 있게  
되있다. 즉, 바이딩하는 방법은 Setter 말고도 생성자로 할 수 있다는 이야이이다.   
4. @ConstructorBinding은 @ConfigurationProperties와 같이 사용할 수 있다.  
같이 사용하면 생성자를 통해 프로퍼티 값을 할당한다.  
단, main 메소드가 있는 클래스에 @ConfigurationPropertiesScan 어노테이션을 선언해  
주어야 한다(@ConstructorBinding과 @ConfigurationProperties를 같이 사용했을 때,
@ConstructorBinding을 사용하지 않고 @ConfigurationProperties를 단독으로 사용했을 때도  
생성이 되는 듯하다?).     
@ConfigurationPropertiesScan을 선언해주면 @ConfigurationProperties가   
있는 위치를 스캔한다. 즉, main 메소드가 있는 클래스의 위치인 베이스 패키지를 기준으로    
하위 패키지에 있는 클래스들을 탐색하여 빈으로 생성한다(빈 생성 대상인 경우).  
  

## @EnableConfigurationProperties
스프링 부트는 application.properties 파일을 이용해서 설정을 제공한다.  
이 파일에는 부트가 제공하는 프로퍼티뿐만 아니라 커스텀 프로퍼티를 추가할 수 있다.  
커스텀 프로퍼티를 사용하는 몇 가지 방법이 있는데 그 중에서 설정 프로퍼티 클래스를 사용하면  
관련 설정을 한 클래스에 관리할 수 있어 편리하다.  
  
설정 프로퍼티 클래스를 사용하는 방법은 간단하다.  
먼저, 설정 프로퍼티 클래스로 사용할 클래스를 작성한다.  
이 클래스는 다음과 같이 작성한다.  
- @ConfiguraionProperties 어노테이션을 클래스에 적용한다. application.properties 파일에서  
  사용할 접두어를 지정한다.  
- application.properties 파일에 설정한 프로퍼티 값을 전달받을 setter를 추가한다.  
- 프로퍼티 값을 참조할 때 사용할 get 메서드를 추가한다.  

다음은 설정 프로퍼티 클래스의 작성 예이다.
```
@ConfigurationProperties(prefix = "eval.security")
public class SecuritySetting {
  private String authcookie;
  private String authcookieSalt;
  
  public String getAuthcookie() {
    return authcookie;
  }
  
  public void setAuthcookie(String authcookie) {
    this.authcookie = authcookie;
  }
  
  public String getAuthcookieSalt() {
    return authcookieSalt;
  }
  
  public void setAuthcookieSalt(String authcookieSalt) {
    this.authcookieSalt = authcookieSalt;
  }
}
```
application.properties 파일의 프로퍼티와 설정 프로퍼티 클래스의 프로퍼티는 다음과 같이 매칭된다.  

> application.properties 프로퍼티 이름 = prefix + "." + setter 프로퍼티 이름

예를 들어, 위 코드에서 prefix는 "eval.security"이므로 setAuthcookie()에 해당하는 프로퍼티는  
"eval.security.authcookie"가 된다.   
만약 setter의 프로퍼티 이름 중간에 대문자가 포함되어 있다면 다양한 매핑을 지원한다.  
예를 들어, 위 코드가 authcookieSalt가 중간에 대문자를 포함하고 있는데 이 경우 다음과 같은  
프로퍼티로부터 값을 가져올 수 있다.  
- eval.security.authcookieSalt
- eval.security.authcookie-salt
- eval.security.authcookie_salt
- EVAL_SECURITY_AUTHCOOKIE_SALT

@ConfigurationProperties를 적용한 클래스를 만들었다면, 다음 할 일은 빈으로 등록하는 것이다.  
스프링 빈으로 등록하는 방법은 간단하다. 다음의 두 가지 방식 중 하나를 사용하면 된다.  

- EnableConfigurationProperties을 이용해서 지정하기
- 설정 프로퍼티 클래스에 @Configuration 적용하기(또는 설정 프로퍼티 클래스를 @Bean으로 등록하기)

먼저 @EnableConfigurationProperties을 사용하는 방법은 다음과 같다.  
@EnableConfigurationProperties을 설정 클래스에 추가하고 @EnableConfigurationProperties의 값으로  
@ConfigurationProperties를 적용한 클래스를 지정하면 된다.  
이 경우 @EnableConfigurationProperties는 해당 클래스를 빈으로 등록하고 프로퍼티 값을 할당한다.  

```
@SpringBootApplication
@EnableConfigurationProperties(SecuritySetting.class)
public class Application {...}
```
두 번째 방법은 설정 프로퍼티 클래스를 등록하는 것이다.  
스프링 부트는 컴포넌트 스캔을 하므로 설정 프로퍼티 클래스에 @Configuration을 붙이면  
자동으로 빈으로 등록된다. 스프링 부트는 해당 빈이 @ConfigurationProperties를 적용한 경우  
프로퍼티 값을 할당한다.  
  
@ConfigurationProperties 적용 클래스를 빈으로 등록했다면 이제 설정 정보가 필요한 곳에  
해당 빈을 주입받아 사용하면 된다.  
예를 들면 다음과 같이 자동 주입받아 필요한 정보를 사용하면 된다. 
```
@Configuration
public class SecurityConfig {
  @Autowired
  private SecuritySetting securitySetting;
  
  @Bean
  public Encryptor encryptor() {
    Encryptor encryptor = new Encryptor();
    encryptor.setSalt(securitySetting.getAuthcookieSalt());
    ...
  }
}
```

@ConfigurationProperties를 사용할 때의 장점은 다음과 같다.

- 필요한 외부 설정을 접두어(prefix)로 묶을 수 잇고 중첩 설정을 지원한다.  
  (예, YAML을 사용하면 계층 구조로 묶을 수 있다.)
- 기본값을 쉽게 지정할 수 있다. 설정 프로퍼티 클래스의 필드에 기본값을 주면 된다. 
- int, double 등 String 이 외의 타입을 설정 프로퍼티 클래스의 프로퍼티에 사용할 수 있다.  
  설정 파일의 문자열을 설정 프로퍼티 클래스의 프로퍼티 타입으로 스프링이 알아서 변환해준다. 
  
  


































