## Feign Client란?
1. Feign Client는 web service 클라이언트를 보다 쉽게 작성할 수 있도록 도와준다.  
2. interface를 작성하고 annotation을 붙여주면 세부적인 내용없이 사용할 수 있기 때문에  
   코드 복잡도가 낮아진다.
3. netflix에서 만들어졌고, spring-cloud-starter-openfeign으로 스프링 라이브러리에서 사용할 수 있다.

### Client 작성
1. 인터페이스로 작성한다.  
   @FeignClient을 작성하면 인터페이스를 클래스로 구체화시킬 필요가 없다.  
   annotation이 작성해준다.
2. annotation을 통해 요청할 url을 넣어준다.
3. 메서드 위의 annotation을 통해 요청할 세부 URL을 설정한다.

```java
@FeignClient(name="feign", url="http://localhost:8080")
public interface TestClient {
  
   @GetMapping("/testfeign")
   String testFeign();
}
```


### FeignClient 파라미터에 따른 호출 성공여부 분석
```java
@FeignClient(name = "test-api", url = "${feign.test-api.url}")
public interface TestClient {
   
   @GetMapping("/test")
   String authTest();
   
   //외부 API의Method 명과 상관이 있을까?
   @GetMapping("/test")
   String authTest2();
   
   @GetMapping("/test/user")
   String paramTest(@RequestParam String userID);
   
   //@RequestParam 어노테이션의 name을 꼭 써야할까?
   @GetMapping("/test/user")
   String paramTest2(@RequestParam(name = "userID") String userID);
   
   //@RequestParam 어노테이션의 name을 안 썻을 때 파라미터 변수명과 상관이 있을까?
   @GetMapping("/test/user")
   String paramTest4(@RequestParam String user);
   
   //외부 API의 파라미터 변수명과 상관이 있을까?
   String paramTest3(@RequestParam(name ="userID") String user);
}
```
- 결과  
  1. AuthTest() : 성공
  2. AuthTest2() : 의문 1. 외부 api의 메소드과 다르다면? -> 성공
  3. paramTest(@RequestParam String userID) : 성공
  4. paramTest2(@RequestParam(name = "userID") String userID) : name속성을 추가한다면? -> 성공
  5. paramTest4(@RequestParam String user) : 실패 -> 외부 API 파라미터명과 다르다.
  6. paramTest3(@RequestParam(name = "userID") String user) : 파라미터명이 다를때, name 속성을 > 추가한다면? ->  
   성공
- 정리하자면 파라미터명이 외부 API 파라미터 명과 동일하다면 @RequestParam을 안써도 되지만  
@RequestParam을 사용하는 이유는 파라미터를 꼭 받아야 하기 때문이다.  
-> @RequestParam을 사용하되 name 속성은 파라미터명이 외부 API 파라미터 명과 같다면 안써도 된다.  
  
   1. PathVariable 에 값이 들어가지 않는 경우  
    Spring MVC Controller 에서는 @PathVariable, @QueryParam 에 value(name) 을 넣지 않아도, field 의  
    이름이 default 로 설정되어 유입된 값이 셋팅이 되지만, feign 에서는 value(name) 값을 넣어줘야  
    @GetMapping @PostMapping 등에 있는 url 에 자동으로 넣어줍니다. default 로 넣어줄거라 생각하고  
    안넣어주면 에러가 발생됩니다. Feign client method parameter 에 @PathVariable, @QueryParam, @RequestBody와  
    같은 annotation 을 설정하지 않음
 
   2. 아무것도 설정하지 않으면 @RequestBody 로 생각해서 Http Body 로 데이터를 보낼려고 합니다.  
   이 때, parameter 가 2개 이상이 되면 Method has too many Body parameters 이라는 에러가 발생됩니다.
   @PostMapping(value = "/anything")
   void anything(ExampleRequest request, ExampleRequest request2);
   
   3. 파라미터에 임의의 클래스를 사용하고 싶은 경우  
   여러개의 paramater를 주입하고 싶은 경우에는 @SpringQueryMap이라는 어노테이션을 사용하면 된다.  
   Map 객체를 만들어서 넣어도 되며 Class를 따로 선언해서 만들어도 된다. [참고링크](https://cloud.spring.io/spring-cloud-openfeign/reference/html/#feign-querymap-support)
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
