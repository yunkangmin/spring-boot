# 1. 스프링 어플리케이션에 application context는 2개가 들어간다. 
- ContextLoaderListener에 의해서 만들어지는 Root WebApplicationContext
- DispatcherServlet에 의해서 만들어지는 WebApplicationContext

1. Root WebApplicationContext -> 이름 그대로 최상단에 위치한 Context이다. 
   - 서비스 계층이나 DAO를 포함한, 웹 환경에 독립적인 빈들을 담아둔다.
   - 서로 다른 서블릿 컨텍스트에서 공유해야 하는 빈들을 등록해놓고 사용할 수 있다.
   - Servlet context에 등록된 빈들을 이용 불가능하고 servlet context와 공통된 빈이 있다면   
   servlet context 빈이 우선된다. 
   - WebApplication 전체에 사용가능한 DB 연결, 로깅 기능들이 이용된다.   
2. WebApplicationContext -> 서블릿에서만 이용되는 Context이다.   
   - DispatcherServlet이 직접 사용하는 컨트롤러를 포함한 웹 관련 빈을 등록하는 데 사용한다. 
   - DispatcherServlet은 독자적인 WebApplicationContext를 가지고 있고, 모두 동일한 Root Web  
   ApplicationContext를 공유한다.   

# WebApplicationContext VS ApplicationContext
스프링에서 말하는 ApplicationContext는 스프링이 관리하는 빈들이 담겨 있는 컨테이너라고 생각하면 된다.  
스프링 안에는 여러 종류의 ApplicationContext 구현체가 있는데, ApplicationContext라는 인터페이스를  
구현한 객체들이 다 이 ApplicationContext이다. WebApplicationContext는 ApplicationContext를 확장한   
WebApplicationContext 인터페이스의 구현체를 말한다. WebApplicationContext는 ApplicationContext에  
getServletContext() 메서드가 추가된 인터페이스이다. 이 메서드를 호출하면 ServletContext가 반환된다.  
**결국 WebApplicationContext는 스프링 ApplicationContext의 변종이면서 ServletContext와 연관관계가  
있다는 정도로 정리가 된다.** 이 메서드가 추가됨과 동시에 [서블릿 컨텍스트](https://github.com/yunkangmin/spring-boot/blob/main/etc/ServletContext.md)를 이용한 몇 가지 빈 생애 주기  
스코프(application, request, session 등)가 추가되기 한다.

# Child WAC(WebApplicationContext)은 여러 개가 생성될 수 있다.  
이론적으로 DispatcherServlet은 여러 개 등록될 수 있다. 왜 그래야 하는지는 생각해보면 많은 이유가 있겠지만  
아무튼 기술적으로 가능하고 그런 의도를 가지고 설계되었다. 그리고 각각 DispatcherServlet은 독자적인 WAC를  
가지고 있고 모두 동일한 Root WAC를 공유한다. 스프링의 AC(WAC도 해당됨)는 ClassLoader와 비슷하게 자신과   
상위 AC에서만 빈을 찾는다. 따라서 같은 레벨의 형제 AC의 빈에는 접근하지 못하고 독립적이다.   
