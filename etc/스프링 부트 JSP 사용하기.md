https://goddaehee.tistory.com/204?category=367461

- 톰캣 jasper란??
  https://12bme.tistory.com/554  
  
  재스퍼(Jasper)  
  톰캣의 JSP 엔진이다. 재스퍼는 JSP파일을 파싱하여 서블릿(JavaEE) 코드로 컴파일한다. JSP 파일의 변경을 감   지하여 리컴파일 작업도 수행한다.  
 
- 톰캣기반 자바 웹어플리케이션에서는 보안상 jsp 위치를 URL로 직접접근할 수 없는 WEB-INF폴더 아래 위치시킨   다.

- pring 애플리케이션 시작시 application.properties 파일에 정의된 내용을 로드한다.
  (스프링부트의 AutoConfiguration을 통해 자동 설정한 속성값들이 존재하며, application.properties의 해당     값들은 오버라이드 한다.)

- server.port  
  별다른 설정을 하지 않으면 default 포트는 8080이다.  
  Spring Boot에 기본적으로 내장되어있는 Tomcat과 Jetty와 같은 WAS의 포트번호를 임의로 변경 할 수 있다.  
  ```
  server.port = 8888
  ```





