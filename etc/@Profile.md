## 스프링 @Profile 어노테이션을 통한 환경 설정(Spring Environment Configuration, @Profile)

### 스프링 환경설정(Spring Environment Configuration)
스프링에서는 프로필(Profile)을 통해 런타임 환경을 설정할 수 있는 기능을 제공한다.  
이 기능을 이용하여 테스트 환경에서 여러 테스트를 돌리고 난 다음 프로덕션 환경으로 전환  
하는 것을 어렵지 않게 할 수 있다.  
  
스프링이 동작할 때 해당 환경(프로필)이 어떤 것인지를 알기 위해서는 ApplicationContext   
인터페이스의 구현 객체에서 getEnvironment 메서드를 통해 Environment 객체를 뽑아낸 뒤  
확인이 가능하다.  

```
@Component
public class AppRunner implements ApplicationRunner {
  
  @Autowired
  ApplicationContext ctx;
  
  @Autowired
  BookRepository bookRepository;
  
  @Override
  public void run(ApplicationArguments args) throws Exception {
    Environment environment = ctx.getEnvironment();
    
   System.out.println(Arrays.toString(environment.getActiveProfiles()));
   System.out.println(Arrays.toString(environment.getDefaultProfiles()));
 }
}
```
```
[]
[default]
```
현재 활성화된 프로필이 없으므로 빈 배열을 반환한다.  
또한 Default 프로필은 default 값이라는 것을 확인할 수 있다.  

### @Profile 어노테이션을 통한 프로필 설정
@Profile 어노테이션을 통해 스프링 환경설정을 할 수 있다.  
자바 설정 파일을 통해 설정하는 방법은 다음과 같다.  

```
@Configuration
@Profile("test")
public class TestConfiguration {
  
  @Bean
  public BookRepository bookRepository() {
    return new TestBookRepository();
  }
}
```

위 코드는 스프링 프로필이 test일 시, @Bean 어노테이션이 붙은 bookRepository 빈이  
컨테이너에 등록이 된다는 것을 뜻한다.  
  
그 다음 JVM의 옵션에 해당 프로파일을 지정하는 옵션을 설정한다.  
```
-Dspring.profiles.active="test"
```

참고로 설정파일에 직접 명시하지 않아도 POJO 클래스에 @Profile 어노테이션을 붙이면  
위와 같이 test 프로필 환경에서 등록되는 스프링 빈을 구현할 수 있다.  

```
@Repository
@Profile("test")
public class TestBookRepository implements BookRepository {
}
```

@Profile 어노테이션에서 '!'와 같은 Not 표현식이나 &, |과 같은 논리연산자도 쓸 수 있다.  
```
@Repository
@Profile("!prod | saelobi")
public class TestBookRepository implements BookRepository {
}
```
다시 AppRunner을 가동하면 다음과 같이 출력된다. 

```
[test]
[default]
```





































