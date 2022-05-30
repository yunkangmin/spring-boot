### @Mock @MockBean 
테스트 케이스를 작성하다가 좀 헷갈리는게 있었다.  
@Mock, @MockBean 차이가 뭐지??  
쓰긴 하고 있는데 알고 써야 하지 않을까라는 의문이 들었다.  
그래서 찾아봤다.  
  
먼저 Mock 객체를 선언할 때에는 2가지 방법이 있다.  

1. 첫번째 : mock()을 이용해서 선언
2. 두번째 : @Mock을 이용해서 선언  
  
이 2개의 테스트 케이스는 동일하게 동작한다.  
  
그리고 선언한 Mock 객체를 테스트를 실행하는 동안 사용할 수 있게 하기 위한 작업이 있다.  
바로 @RunWith(MockitoJUnitRunner.class)가 바로 그 역할을 해준다.  
또 다른 방법으로는 아래와 같은 메소드를 테스트 케이스 실행 전에 실행시키면 된다.  
  
그럼 @MockBean은 뭐지??  
  
@MockBean은 Mock과는 달리 org.springframework.boot.test.mock.mockito 패키지 하위에 존재한다.  
즉, spring-boot-test에서 제공하는 어노테이션이다.  
Mockito의 Mock 객체들을 Spring의 ApplicationContext에 넣어준다.  
그리고 동일한 타입의 Bean이 존재할 경우 MockBean으로 교체해준다.  
  
그럼 언제 @Mock을 쓰고 언제 @MockBean을 써야 하나?  
  
결론적으로 Spring Boot Container가 필요하고 Bean이 container에 존재해야 한다면 @MockBean을 쓰면 되고  
아니라면 @Mock을 쓰면 된다. 그런데 개인적으로는 그걸 잘 판단을 못하겠다.  
  
