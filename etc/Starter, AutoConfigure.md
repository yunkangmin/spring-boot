## 스프링 부트 자동 설정 만들기: Starter, AutoConfigure

### 스프링 자동 설정 이해
스프링 부트는 스프링 프레임워크에서 어플리케이션을 만들 때 주로 사용하는  
설정들을 자동으로 설정한다. 이 기능은 자바의 main 진입점에 @SpringBootApplication을  
붙임으로서 사용할 수 있다.  
```
@SpringBootApplication
public class Application {
  
  public static void main(String[] args) { 
    SpringApplication.application = new SpringApplication(Application.class);
    application.run(args);
  }
}
```
위 @SpringBootApplication는 다음의 어노테이션들을 붙인 효과와 동일한 기능을 한다.  
```
@Configuration
@ComponentScan
@EnableAutoConfiguration
public class Application {
  
  public static void main(String[] args) {
    SpringApplication.application = new SpringApplication(Application.class);
    application.run(args);
  }
}
```
스프링 부트는 @SpringBootApplication에 있는 @ComponentScan과 @EnableAutoConfiguration  
어노테이션을 통해 두 단계로 나뉘어서 스프링 부트 프로젝트의 스프링 빈을 찾아내어 등록한다.  
  
@ComponentScan은 스프링 프레임워크에서 @Repository, @Configuration, @Service등 스프링 빈을  
나타내는 어노테이션을 @ComponentScan이 붙은 클래스가 위치하고 있는 현재 패키지를 기준으로  
그 아래 패키지까지 찾아내서 스프링 빈으로 등록하는 기능을 가진 어노테이션이다.  
  
@EnableAutoConfiguration은 스프링 부트에서 스프링 프레임워크에서 많이 쓰이는 스프링 빈들을  
자동적으로 컨테이너에 등록하는 역할을 하는 어노테이션이다.  
@EnableAutoConfiguration이 등록하는 빈들의 목록은 spring-boot-autoconfigure-2.X.X.RELEASE.jar  
파일에 포함되어 있다.  

```
// spring-boot-autoconfigure-2.X.X.RELEASE.jar org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudServiceConnectorsAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
```
위 목록 중에는 @EnableAutoConfiguration을 붙였을 시 스프링 부트 프로젝트를 기본적으로  
웹 프로젝트로 만들 수 있는 기본값이 설정되어 있다.(ServletWebServerFactory빈이 설정되어 있음)  
  
따라서 스프링 부트를 웹 어플리케이션으로 만들지 않고 일반 프로젝트 용도로 쓰고자 하면 다음과  
같은 코드로 실행해야 한다.  
```
@Configuraion
@ComponentScan
public class Application {
  
  public static void main(String[] args) {
    SpringApplication application = new SpringApplication(Application.class);
    application.setWebApplicationType(WebApplicationType.NONE);
    application.run(args);
  }
}
```

### Maven 프로젝트로 AutoConfigure 자동설정 클래스 만들기
Maven 프로젝트로 @EnableAutoConfiguration 어노테이션의 자동 빈 등록 리스트에  
포함되는 스프링 빈 설정 클래스를 만들 수 있다.  
  
먼저 IDE를 통해서 Maven 프로젝트를 만들고, pom.xml에 다음과 같이 설정정보를  
입력해야 한다.  
```
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-autoconfigure</artifactId>
  </dependency>
  
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-autoconfigure-processor</artifactId>
    <optional>true</optional>
  </dependency>
</dependencies>

<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>2.1.1.RELEASE</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```
참고로 현재 Maven 프로젝트의 그룹과 아티팩트 정보는 다음과 같다.
```
<groupId>com.tutorial</groupId>
<artifactId>autoconfigure</artifactId>
<version>1.0-SNAPSHOT</version>
```
![image](https://user-images.githubusercontent.com/33191974/140319614-1085d761-40ae-4701-9f65-b9918f47a22a.png)  
프로젝트의 각 파일들의 내용은 다음과 같다.
```
#spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration= \com.tutorial.AutoConfig
```
```
public class Saelobi {
  
  String name;
  
  int howLong;
  
  public String getName(){
    return name;
  }
  
  public void setName(String name) { 
    this.name = name;
  }
  
  public int getHowLong() {
    return howLong;
  }
  
  public void setHowLong(int howLong) {
    this.howLong = howLong;
  }
  
  @Override
  public String toString() {
    return "Saelobi{" + 
            "name" + name + \ +
              ", howLong" + howLong +
              "}";
  }
}
```

```
@Configuration
public class AutoConfig {
  
  @Bean
  public Saelobi saelobi() {
    Saelobi saelobi = new Saelobi();
    saelobi.setHowLong(5);
    saelobi.setName("KBS");
    return saelobi;
  }
}
```
@EnableAutoConfiguration 어노테이션은 프로젝트 내의 spring.factories의 설정정보를 읽어와 등록되어  
있던 @Configuration 클래스의 정보를 자동적으로 컨테이너에 등록한다.  
  
프로젝트에서 mvn install 명령어를 실행하여 패키지를 빌드하고 local repository에 등록한다. 

### AutoConfigure 프로젝트 사용하기
위에서 만든 AutoConfig 설정 클래스를 사용하기 위해서는 스프링 부트 프로젝트에서 다음과 같은  
의존성을 추가한다.  
```
<dependency>
  <groupId>com.tutorial</groupId>
  <artifactId>autoconfigure</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>
```
![image](https://user-images.githubusercontent.com/33191974/140322680-e2299651-f6b4-4aff-bb59-c91747472dd4.png)  
이 의존성을 토대로 스프링 부트가 컨테이너에 스프링 빈을 등록했는지 확인하기 위해 다음과  
같은 코드를 작성한다.  
  
이 의존성을 토대로 스프링 부트가 컨테이너에 스프링 빈을 등록했는지 확인하기 위해  
다음과 같은 코드를 작성한다.  
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
다음과 같이 @SpringBootApplication 어노테이션이 붙은 메인 진입점에서 프로젝트를 실행하면  
saelobi에 자동적으로 스프링 빈이 주입된 것을 알 수 있다. 새로 스프링 부트에서 Saelobi 클래스를  
작성하여 스프링 빈으로 등록하지 않고도 위에서 작성한 AutoConfig 설정 파일에 등록되어 있는  
Saelobi 클래스가 자동적으로 등록되어 사용할 수 있다는 것을 알 수 있다.  
```
@SpringBootApplication
public class Application {
  
  public static void main(String[] args) {
    SpringApplication application = new SpringApplication(Application.class);
    application.setWebApplicationType(WebApplicationType.NONE);
    application.run(args);
  }
}
```

  
















































