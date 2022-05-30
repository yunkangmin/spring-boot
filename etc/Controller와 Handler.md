## Controller와 Handler
클라이언트의 요청을 실제로 처리하는 것은 컨트롤러이고 DispatcherServlet은 클라이언트의  
요청을 전달받는 창구 역할을 한다. 앞서 설명했듯이 DistpaterServlet은 클라이언트의   
요청을 처리할 컨트롤러를 찾기 위해 HandlerMapping을 사용한다.  
  
컨트롤러를 찾아주는 객체의 타입은 ControllerMapping이어야 할 것 같은데 실제 타입은  
HanlderMapping이다.  
왜냐하면 스프링 MVC는 웹 요청을 처리할 수 있는 범용적(여러 분야나 용도로 널리 쓰이는 것)인  
프레임워크를 제공하고 있다.    
이런 이유로 스프링 MVC는 웹 요청을 실제로 처리하는 객체를 핸들러(Handler)라고 표현하고 있으며  
@Controller 적용 객체나 Controller 인터페이스를 구현한 객체 모두 스프링 MVC 입장에서는 핸들러가 된다.  
따라서, 특정 요청 경로를 처리해주는 핸들러를 찾아주는 객체를 HandlerMapping이라고 부른다.  
DispatcherServlet은 핸들러 객체의 실제 타입에 상관없이, 실행 결과를 ModelAndView라는 타입으로만  
받을 수 있으면 된다. 그런데 핸들러의 실제 구현 타입에 따라 ModelAndView를 리턴하는 객체도 있고  
그렇지 않은 구현 객체도 있다.  
따라서 핸들러의 처리 결과를 ModelAndView로 변환해주는 객체가 필요하며, HandlerAdapter가 바로  
이 변환을 수행해준다.  
  
핸들러 객체의 실제타입마다 그에 알맞는 HandlerMapping과 HandlerAdapter가 존재하기 때문에   
사용할 핸들러의 종류에 따라 해당 HandlerMapping과 HandlerAdapter를 스프링 빈으로 등록해주어야 한다. 

### HandlerMapping
DispatcherServlet으로 들어온 모든 요청을 각각의 Controller로 위임 처리를 하는 HadlerMapping 설정을  
바로 DispatcherServlet 설정 파일에서 한다.  
  
스프링 프레임워크에서 제공하는 HandlerMapping 클래스는 4가지가 있다.
1. BeanNameUrlHandlerMapping
2. ControllerClassNameHandlerMapping
3. SimpleUrlHandlerMapping
4. DefaultAnnotationHandlerMapping

...



























