# getParameter()   
1. java.lang.String getParameter(java.lang.String name)
- 반환 유형이 String이다.   
2. 클라이언트의 HTML 페이지에서 필요한 정보를 얻는데 사용한다.   

# getAttribute()   
attribute란 Servlet 간 공유하는 객체이다.   
1. java.lang.Object getAttribute(java.lang.String name)   
- 반환 유형이 Object이다.  
2. 이전에 다른 JSP 또는 Servlet 페이지에 설정된 매개 변수를 가져오는 데 사용   
한다.  
- 이전의 setAttribute() 속성을 통한 설정이 있어야 한다. 그렇지 않다면 null     
값을 가져온다.   
3. Controller Servlet 등에서 View로 전달할 때 사용한다.   



















