# ServletContext
- 톰캣이 실행되면서 생성된다.
- 서블릿 컨텍스트(ServletContext)란 하나의 서블릿이 서블릿 컨테이너와 통신하기 위해서 사용되어지는  
메서드들을 가지고 있는 클래스가 바로 ServletContext이다.   
- 하나의 web application 내에 하나의 컨텍스트가 존재한다. web application 내에 있는 모든 서블릿들을  
관리하며 정보공유할 수 있게 도와주는 역할을 담당하는 것이 바로 ServletContext이다.   
- 쉽게 말하면 웹 어플리케이션의 등록 정보라고 볼 수 있다.   
- 필터와 리스너 또한 등록하여 통신 간에 활용할 수 있다.  
   - 리스너는 서블릿 리스너, 세션 리스너 등의 EventListener 구현체는 뭐든지 등록할 수 있다.
   - 필터는 characterEncoding등 Filter 구현체는 뭐든지 등록할 수 있다.   
   
