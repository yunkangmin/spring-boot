대부분의 운영체제는 실행되는 프로그램들에게 유용한 정보를   
제공할 목적으로 환경변수를 제공한다. 환경변수는 운영체제에서  
이름(Name)과 값(value)로 관리되는 문자열 정보이며 운영체제가  
설치될 때 기본적인 내용이 설정되지만, 사용자가 직접 설저하거나  
응용프로그램이 설치될 때 자동적으로 변경되기도 한다. 자바에서는  
OS의 환경변수의 값을 System.getenv()라는 메서드를 통해서  
불러올 수 있다.

# 환경 변수 읽기 System.getenv()
```java
System.out.println("전체 OS 환경변수 값 : "  + System.getenv());
System.out.println("OS 환경변수 NVM_SYMLINK 값: " + System.getenv("NVM_SYMLINK")):
```
자바에서 OS의 환경 변수의 값이 필요할 경우 System.getenv() 메서드  
를 사용하면 된다. 매개값으로 환경변수 이름을 주면 값이 매개변수로  
넘긴 환경 변수의 이름이 리턴된다. 시스템마다 환경변수의 값은  
다를 수 있다.  

# 정리
스프링 부트에서 System.getenv()를 사용하여 가져올 수도 있지만 
@Value를 사용해서 OS 환경변수를 가져올 수도 있다([우선순위주의](https://wordbe.tistory.com/entry/Springboot-%EC%99%B8%EB%B6%80%EC%84%A4%EC%A0%95-%ED%94%84%EB%A1%9C%ED%8C%8C%EC%9D%BC)). 
