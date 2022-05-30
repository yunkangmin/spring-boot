스프링 부트에서의 설정은 크게 웹과 관련된 설정과 웹과 무관한 설정  
둘로 나누어서 관리하는 것을 권장한다.  
  
편의상 앞으로 웹과 관련된 설정 파일은 MVCConfig라고 하자.    
## WebMvcConfigurer
MVCConfig가 웹과 관련된 처리를 하기 위해서는 WebMvcConfigurer interface를 구현하는 것이 좋다.  
```
@Configuration
public class MVCConfig implements WebMvcConfigurer {
}
```
WebMvcConfigurer에는 Spring MVC에서 아주 유용하게 사용되는 기능들이 선언되어있어서 해당 기능을  
사용하려면 관련 메서드를 재정의하면 된다. 또한 WebMvcConfigurer의 모든 메서드가 default 메서드로  
선언되어 있어서 반드시 재정의해야하는 메서드는 없다. 필요할 때만 하면된다. 

## view controller
WebMvcConfigurer 활용의 가장 쉬운 예로 view controller에 대해 설정하는 addViewControllers 메서드를  
재정의해보자.  
  
웹 애플리케이션을 만들다 보면 굳이 거창한 Spring의 Controller를 거치지 않고 단순히 페이지만  
보여줘도 충분할 경우가 있다. 예를 들면 /login을 get 방식으로 요청하면 단지 login을 위한  
form을 보여주면 충분하다.   
  
이런 간단한 페이지라도 스프링이 서비스하려면 Controller가 필요한데 Controller 필요없이 단순한  
View만 존재하는 경우 요청을 받으면 바로 view name을 View Resolver에게 넘겨 버리는 것이  
View Controller이다.  
  
View Controller를 사용하기 위해서는 WebMvcConfigurer의 addViewControllers를 재정의한다.  
```
//Controller에 개별적인 handler method로 구현하는 형태
@GetMapping("/login"0
public String loginForm(Model model) {
  return "login";
}

//MVCConfig에서 View Controller로 처리하는 형태
public class MVCConfig implements WebMvcConfigurer {
  @Override
  public void addViewControllers(ViewControllerRegistry registry) {
    //login이라고 요청이 들어오면 login을 viewResolver에게 넘겨줌
    registry.addViewController("/login").setViewName("login");
  }
}
```

## JSP를 위한 ViewResolver 설정
JSP 파일을 사용하도록 ViewResolver 설정을 추가해주자.  
가장 간단한 방법은 application.properties에 아래와 같이 설정하는 것이다.  
```
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```
Handler Adapter에서 view 이름이 반환되면 resolver는 view 이름의 앞에 /WEB-INF/
views/를 붙이고 뒤에 .jsp를 붙여서 실제 호출될 jsp의 위치를 완성한다.  
즉, index라는 이름을 받으면 /WEB-INF/views/index.jsp를 찾게 되는 것이다.   
  
또는 명시적으로 ViewResolver를 추가할 수도 있다.  
```
@Configuration
public class MVCConfig implements WebMvcConfigurer {
  @Bean
  public ViewResolver internalResourceViewResolver() {
    //JSP를 뷰로 사용할 때 쓰인다.
    InteralResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
  }
  ...
}
```
  


