### [JPA] QueryDSL에서 Pageable 객체를 이용한 Sort 방법
우선 Order, Path, fieldName을 전달하면 OrderSpecifier 객체를 리턴하는 [Util 클래스](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tmondev&logNo=220976934466)를 작성해서  
Sort 시마다 사용할 수 있도록 한다. 

- Path 파라미터는 compileQuerydsl 빌드를 통해서 생성된 Q타입 클래스(QueryDSL에서 사용하는 엔티티를 기반으로  
  생성된 쿼리용 클래스를 말함)의 객체이다.
- Sort의 대상이 되는 Q타입 클래스 객체를 전달한다.

```java
public class QueryDslUtil {
    
  public static OrderSpecifier<?> getSortedColumn(Order order, Path<?> parent, String fieldName) {
      Path<Object> fieldPath = Expressions.path(Object.class, parent, fieldName);
      return new OrderSpecifier(order, fieldPath);
  }
  
}
```

#### 출처 https://uchupura.tistory.com/7
