
## @Bean @Configutation @Component
기존의 Spring MVC에서는 xml을 활용하여 Bean을 등록하고 있었다.  
하지만 프로젝트의 규모가 커짐에 따라 사용하는 요소들을 xml에 등록하는 것이 상당히 번거로워 져서 어노테이션을 활용한  
Bean 등록 방법이 탄생하게 되었다.  
이번에는 Spring에서 Bean을 등록하기 위해서 활용가능한 @Bean, @Component, @Configuration 어노테이션에 대해서 알아보도록 한다.  

## Spring Bean이란?
Spring에서는 Spring의 DI Container에 의해 관리되는 POJO(Plain Old Java Object)를 Bean이라고 부르며  
이러한 Bean들은 Spring을 구성하는 핵심 요소이다. Spring의 Bean을 정리하면 아래와 같다.  

 - POJO로서 Spring 애플리케이션을 구성하는 핵심 객체이다.
 - Spring IOC 컨테이너(또는 DI 컨테이너)에 의해 생성 및 관리된다.
 - class, id, scope, constructor-arg등을 주요 속성으로 지닌다.
 
만약 xml을 사용하여 Bean을 등록해주어야 하는 클래스가 1개라면 간단하지만 실제 개발을 하다 보면 상당히 많은 양의 클래스를 Bean으로  
등록하게 되고, 해당 클래스를 일일이 xml을 통해 Bean으로 등록하려면 상당히 번거로운 작업이 된다.  
이러한 번거로움을 덜기 위해 Spring에서는 Annotation을 활용해 Bean을 등록하고 사용하는 방법을 제공하고 있다.  

## @Bean, @Configuraion, @Component를 이용한 Spring Bean 등록 방법
AuthenicateInterceptor를 Bean으로 등록하고자 한다면 @Bean 어노테이션을 활용하면 된다.  
아래의 예제에서 authenticateInterceptor()라는 메서드를 통해서 AuthenticateInterceptor를 Bean으로 등록하고 있다.  

```java
import com.example.HeaderFilter; 
import com.example.SessionInterceptor; 
import org.springframework.boot.web.servlet.FilterRegistrationBean; 
import org.springframework.context.annotation.Bean; 
import org.springframework.context.annotation.Configuration; 
import org.springframework.core.Ordered; 
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder; 
import org.springframework.web.servlet.config.annotation.InterceptorRegistry; 
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry; 
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry; 
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer; 
import org.springframework.web.servlet.resource.ResourceUrlEncodingFilter; 

import java.util.Properties; 

// 1개 이상 Bean을 등록하고 있음을 명시하는 어노테이션 
@Configuration 
//WebMvcConfigurer를 사용하면 @EnableWebMvc(스프링 MVC사용을 위한 HandlerMapping, HandlerAdapter 설정)가 
//자동적으로 세팅해주는 설정에 개발자가 원하는 설정을 추가할 수 있게 된다.
public class WebMvcConfig implements WebMvcConfigurer { 
   private static final String[] CLASSPATH_RESOURCE_LOCATIONS = {"classpath:/static/", "classpath:/public/", "classpath:/",
                           "classpath:/resources/", "classpath:/META-INF/resources/", "classpath:/META-INF/resources/webjars/"}; 
   
   @Bean 
   public AuthenticateInterceptor authenticateInterceptor(){ 
     return new AuthenticateInterceptor(); 
   } 
   
   @Override 
   public void addInterceptors(InterceptorRegistry registry) {
     // 로그인, 로그아웃, 회원가입은 세션 검사를 할 필요가 없으므로 exclude
     registry.addInterceptor(authenticateInterceptor()) 
             .addPathPatterns("/user/signIn") 
             .addPathPatterns("/user/signOut")
             .addPathPatterns("/user/signUp") 
             .excludePathPatterns("/*"); 
   } 
   
   @Override 
   public void addViewControllers(ViewControllerRegistry registry) { 
     // /에 해당하는 url mapping을 /common/test로 forward한다. 
     registry.addViewController("/").setViewName("forward:/index"); 
     // 우선순위를 가장 높게 잡는다. 
     registry.setOrder(Ordered.HIGHEST_PRECEDENCE); 
   } 
   
   @Override
   public void addResourceHandlers(ResourceHandlerRegistry registry) { 
     registry.addResourceHandler("/**").addResourceLocations(CLASSPATH_RESOURCE_LOCATIONS); 
   } 
   
   @Bean 
   public ResourceUrlEncodingFilter resourceUrlEncodingFilter() { 
     return new ResourceUrlEncodingFilter(); 
   } 
   
   @Bean
   public HeaderFilter createHeaderFilter() { 
     return new HeaderFilter(); 
   } 
   
   @Bean 
   public FilterRegistrationBean<HeaderFilter> getFilterRegistrationBean() { 
     FilterRegistrationBean<HeaderFilter> registrationBean = new FilterRegistrationBean<>(createHeaderFilter()); 
     registrationBean.setOrder(Integer.MIN_VALUE);
     registrationBean.addUrlPatterns("/*");
     return registrationBean; 
   } 
   
   @Bean 
   public BCryptPasswordEncoder createBCryptPasswordEncoder(){ 
     return new BCryptPasswordEncoder(); 
   } 

}
```

위의 예제에서는 AuthenticateInterceptor 외에도 HeaderFilter, BCrpytPasswordEncoder 등을 Bean으로 등록하고 있는데,  
이러한 클래스들을 xml에 일일이 적어서 등록한다고 하면 상단히 많은 시간을 차지할 것이고 생산력 저하를 야기할 것이다.  
그렇기 때문에 반드시 어노테이션을 활용한 Bean의 등록을 지향해야 한다.  
  
하지만 어떤 임의의 클래스를 만들어서 @Bean 어노테이션을 붙인다고 되는 것이 아니고 @Bean을 사용하는 클래스에는 반드시  
@Configuration 어노테이션을 활용하여 해당 클래스에서 Bean을 등록하고자 함을 명시해주어야 한다.  
  
위의 예제에서도 클래스 이름 위에 @Configuration 어노테이션을 명시하여 해당 클래스에서 1개 이상의 Bean을 생성하고   
있음을 명시하고 있다.   
그렇기 때문에 @Bean 어노테이션을 사용하는 클래스의 경우 반드시 @Configuration과 함께 사용해주어야 한다.  
  
이러한 @Bean 어노테이션의 경우 아래와 같은 상황에서 주로 사용한다.  
 1. 개바자가 직접 제어가 불가능한 라이브러리를 활용할 때
 2. 초기에 설정을 하기 위해 활용할 때
 
 위의 예제에서는 Spring에서 제공하여 개발자가 직접 제어할 수 없는 BCryptPasswordEncoder, ResourceUrlEncodingFilter,  
 FilterRegistration을 위해 @Bean을 사용하고 있고,   
 설정을 위해 개발자가 개발한 AuthenticateInterceptor, HeaderFilter를 위해 @Bean을 사용하고 있다.  
 AuthenticateInterceptor, HeaderFilter은 개발자가 개발한 클래스이지만 초기 설정을 하기 위해  
 @Bean으로 설정했다.
   
 @Configuraion없이 @Bean만 사용해도 스프링 빈으로 등록이 된다.  
 대신 메서드 호출을 통해 객체를 생성할 싱글톤을 보장하지 못한다.  
 그렇기 때문에 Spring 설정 정보를 위해서는 반드시 @Configuration을 사용해주어야 한다.  
   
 ## @Component 어노테이션
 개발자가 직접 개발한 클래스를 Bean으로 등록하고자 하는 경웨는 @Component 어노테이션을 활용하면 된다.  
 
 ```java
 @Component
 public class FileUtils {  -> 이 상태로 @Bean 설정을 안해도 객체가 생성됨.
 ``` 
   
 물론 다음과 같은 방법으로도 빈 등록을 할 수 있다.  
   
 ```java
 @Bean
 public FileUtils fileUtils() {
    return new FileUtils();
 }
 ```
   
 하지만 이렇게 직접 개발한 클래스 단위의 빈을 등록할 때에는 @Component를 통해 해당 클래스를 빈으로 등록함을 노출하는 것이 좋다.  
 왜냐하면 해당 클래스에 있는 @Component만 보면 해당 빈이 등록되도록 잘 설정되었는지를 찾지 않아도 확인할 수 있기 때문이다.  
 반면에 @Bean을 이용하면 해당 메서드를 찾는 번거로움이 생길 수 있다.  
   
 추가로 @Component를 이용한다면 Main 또는 App 클래스에서 @ComponentScan으로 컴포넌트를 찾는 탐색 범위를 지정해주어야 한다.  
 하지만 SpringBoot를 이용중이라면 @SpringBootConfiguration 하위에 기본적으로 포함되어 있어 별도의 설정이 필요없다.  
   
## 요약
  ### @Bean, @Configuration
  - 개발자가 직접 제어가 불가능한 외부 라이브러리 또는 설정을 위한 클래스를 Bean으로 등록할 때 @Bean 어노테이션을 활용
  - 1개 이상의 @Bean을 제공하는 클래스의 경우 반드시 @Configuration을 명시해 주어야 함.
  
  #### @Component
  - 개발자가 직접 개발한 클래스를 Bean으로 등록하고자 하는 경우 @Component 어노테이션을 



 
