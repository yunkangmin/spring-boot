# ApplicationContext란?
스프링 애플리케이션 전반에 걸쳐 모든 구성요소의 제어 작업을 담당하는 IOC 엔진이다.   
  
IOC 방식을 따라 만들어진 일종의 빈 팩토리이다. 빈 팩토리하고 말할 때는 빈을 생성하고 관계를 설정하는  
IOC의 기본 기능에 초점을 맞춘 것이고, 애플리케이션 컨텍스트는 별도의 정보를 참고해서 빈의 생성, 관계  
설정 등의 제어를 총괄한다.   
  
또한 싱글톤을 저장하고 관리하는 싱글톤 레지스트리이다. 스프링은 기본적으로 별다른 설정을 하지 않으면   
내부에서 생성하는 빈 오브젝트를 모두 싱글톤으로 만든다.  
  
매번 클라이언트에서 요청이 올 때마다 각 로직을 담당하는 오브젝트를 새로 만들어서 사용한다고 생각해보자  
요청 한 번에 5개의 오브젝트가 새로 만들어지고 초당 500개의 요청이 들어오면, 초당 2500개의 새로운 오브  
젝트가 생성된다. 서버가 감당하기 힘들다. 그래서 필요한 것이 싱글톤이다.   

---
다른 정의  
Bean Factory와 마찬가지로, Bean 객체를 생성하고 관리하는 기능을 가지고 있다.  
뿐만 아니라 트랜잭션 관리, 메시지 기반의 다국어 처리, AOP 처리등등 DI(Dependency Injectino)과  
IOC(Inverse of Conversion) 외에도 많은 부분을 지원하고 있다.   
  
아무래도 컨테이너가 구동되는 시점에 객체들을 생성하는 Pre-Loading 방식이 Bean Factory와 가장  
큰 차이점이 아닌가 싶다.

# Bean Factory
Bean Factory는 스프링 설정파일에 등록된 Bean 객체를 생성하고 관리하는 기본적인 기능만 제공한다.  
컨테이너가 구동될 때 Bean 객체를 생성하는 것이 아니라 클라이언트의 요청에 의해서 Bean 객체가 사용되는  
시점(Lazy Loading)에 객체를 생성하는 방식을 사용하고 있다.  
일반적으로 스프링 프로젝트에서는 사용될 일이 없지만 ApplicationContext는 BeanFactory를 상속받고 있다.      

# Application Context의 종류
ApplicationContext는 기본적으로 AbstractApplicationContext의 Interface를 구현한 구현체이다.   
기본적인 구현함수로 getBean("")등이 있겠으나 자세한 내용은 링크를 참조하기 바란다.  
- 구현 클래스
   - GenericXmlApplicationContext : 파일 시스템이나 클래스 경로에 있는 XML 파일을 로딩하여 구동하는    
   컨테이너다.
   - XmlWebApplicationContext : 웹 기반의 스프링 어플리케이션을 개발할 때 사용하는 컨테이너다.  
   
일반적으로 Spring MVC의 환경을 제작하면 대부분 XmlWebApplicationContext가 자동으로 생성되어 사용된다.    

# 스프링 컨테이너란?
스프링 프레임워크는 스프링의 빈을 생성하고 관리하는 컨테이너를 가지고 있고 이를 통해서 스프링의 주개념인  
IOC나 AOP에 대해서 관리하곤 하는데 그 주체(컨테이너)가 바로 ApplicationContext이다.   

# 스프링 컨테이너의 종류
스프링 컨테이너의 종류는 BeanFactory와 이를 상속한 ApplicationContext 2가지 유형이 존재한다.  


















  

  
