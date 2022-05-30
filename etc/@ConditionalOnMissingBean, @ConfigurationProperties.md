### 스프링 부트 자동 설정 만들기, @ConditionalOnMissingBean, @ConfiguraionProperties
[@Configuration 어노테이션을 등록하여 Autoconfigure 스프링 부트 자동 설정](https://github.com/yunkangmin/spring-boot/blob/main/etc/Starter%2C%20AutoConfigure.md)  
만들었지만 여기에 문제점이 있다.
```
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(Application.class);
        application.setWebApplicationType(WebApplicationType.NONE);
        application.run(args);
    }
    
    //재정의
    @Bean
    public Saelobi saelobi(){
        Saelobi saelobi = new Saelobi();
        saelobi.setName("KBS2");
        saelobi.setHowLong(50);
        return saelobi;
    }
}
```
```
@Component
public class AppRunner implements ApplicationRunner {

    @Autowired
    Saelobi saelobi;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(Saelobi.class);
        System.out.println(saelobi);
    }
}
```
만일 @EnableAutoConfiguration 어노테이션을 통한 스프링 빈이 주입되었을 경우 어떤 특별한  
이유로 이 빈을 다시 재정의하여 쓸 시에는 다음과 같은 에러 문구가 뜨면서 재정의 시도가  
차단된다.  
```
***************************
APPLICATION FAILED TO START
***************************

Description:
//클래스 경로 리소스 [com/tutorial/AutoConfig.class]에 정의된 'saelobi' 빈을 등록할 수 없습니다. 
//해당 이름을 가진 빈은 이미 com.tutorial.springboot.Application에 정의되어 있으며 재정의가
//비활성화되어 있습니다.
The bean 'saelobi', defined in class path resource [com/tutorial/AutoConfig.class],
could not be registered. A bean with that name has already been 
defined in com.tutorial.springboot.Application and overriding is disabled.

Action:


//bean 중 하나의 이름을 바꾸거나 spring.main.allow-bean-definition-overriding=true를
//설정하여 재정의를 활성화하는 것을 고려하십시오.
Consider renaming one of the beans or enabling overriding by setting
spring.main.allow-bean-definition-overriding=true
```

### @ConditonalOnMissingBean을 통한 충돌 요소 해결
위 오버라이딩 충돌을 해소하려면 자동 설정 부분을 Maven 프로젝트를 통해 만들 시  
@ConditionalOnMissingBean 어노테이션을 추가해야한다. 이 어노테이션은 스프링 부트  
프로젝트 상에서 동명의 스프링 빈이 정의되었을 시에는 쓰지 않고 그 스프링 빈을 쓰며  
만약 없을 시에는 자동 등록한 빈을 쓰게 끔 유도하는 용도로 쓰인다.  
```
@Configuration
public class AutoConfig {
  
  @Bean
  @ConditionalOnMissingBean
  public Saelobi saelobi() {
    Saelobi saelobi = new Saelobi();
    saelobi.setHowLong(5);
    saelobi.setName("KBS");
    return saelobi;
  }
}
```
다시 mvn install을 실행하여 local repository에 등록할 경우 이번에는 충돌했던 부분이  
해소되고 스프링 부트에 등록됬던 saelobi 빈이 주입되는 것을 알 수 있다.



























