## Mockito란?
Mock(진짜 객체처럼 동작하지만 프로그래머가 직접 컨트롤할 수 있는 객체: 가짜 객체)을 지원하는 프레임워크.  
Mock 객체를 만들고 관리하고 검증할 수 있는 방법 제공.(가짜 객체를 만들어준다고 생각)  
  
구현체가 아직 없는 경우나, 구현체가 있더라도 특정 단위만 테스트하고 싶을 경우 주로 사용한다.  
  
## Test Double(Mockito)
스프링과 Junit을 이용해서 테스트 코드를 작성하다 보면 테스트 환경(database, api)을 구현하는 코드까지 작성해야 하고   
실제 테스트할 코드보다 환경을 구현하는 코드가 훨씬 더 복잡해지게 된다. 이런 문제 영역을 해결하기 위해 테스트 더블이라는  
것이 나왔고 Java 진영에서는 대표적으로 Mockito가 있다.  
  
## Mockito의 어노테이션  
Mockito를 사용하다 보면 주로 아래 어노테이션들을 접할 수 있게 된다.  
  
- @Mock  
- @MockBean  
- @Spy  
- @SpyBean
- @InjectMocks
  
위 어노테이션들은 이름도 비슷한데 쓰임새까지 비슷하다.  
하지만 엄연히 다른 어노테이션이고 저도 쓰다보니 계속 헷갈리는 부분이 많아서 각각 어노테이션들의 차이점과 정확한 사용법에  
대해 정리해보려 한다.  
  
## Test 대상 : 주문서 생성로직  
예제에서 다룰 Test 대상은 주문서 Service 클래스 중 생성로직이다.  

```
//클래스와 메소드들
OrderService
createOrder()

OrderRepository
findOrderList()
createOrder()

NotificationClient
notifyToMobile()
```
  
OrderService에서 OrderRepository와 NotificationClient를 사용한다.  
  
OrderService의 createOrder()는 아래 절차를 따른다.  

  1. OrderRepository.findOrderList()로 기존 주문서 조회
  2. 주문서가 있다면 중복으로 간주해 OrderDuplicateException 발생
  3. OrderRepository.createOrder()로 주문서 생성
  4. Argument로 넘어온 isNotify가 true이면 NotificationClient.notifyToMobile()를 이용해 알림 발생
  
위 절차를 따른 코드는 아래와 같다.  
메서드에 print문을 넣은 것은 테스트 코드에서 stub된 건인지 확인하기 위해서이다.  
stub되지 않았다면 아래 코드에서 작성된 print문이 실행된다.  
  
```java
@Service
@RequiredArgsContructor
public class OrderService {
  
  private final OrderRepository orderRepository;
  private final NotificationClient notificationClient;
  
  public void createOrder(Boolean isNotify) {
     List<Order> orderList = orderRepository.findOrderList();
     if(orderList.size() > 0) {
        throw new OrderDuplicationException();
     }
     
     orderRepository.createOrder();
     
     if(isNotify) {
        notificationClient.nitifyToMobile();
     }
  }
}

@Repository
public class OrderRepository {
   public List<Order> findOrderList() {
      System.out.println("real OrderRespository findOrderList");
      return Collections.emptyList();
   }
   
   public void createOrder() {
     System.out.println("createOrder success");
   }
}

@Component
public class NotificationClient {
   public void notifyToMobile() {
      System.out.println("notify to mobile success");
   }
}
```

## Test Double을 사용하는 이유
### 최초 OrderService의 테스트 작성
Test Double이 없이 위 로직을 테스트하려면 아마 아래와 같은 테스트 코드를 작성하게 될 것이다.  

```java
class BasicTests {
   private OrderService orderService;
   
   @Test
   void createOrderTest() {
      OrderRepository orderRepository = new OrderRespository();
      NotificationClient notificationClient = new NotificationClient();
      
      orderService = new OrderService(orderRepository, notificationClient);
      
      orderService.createOrder(true);
   }
}
```

그리고 위 test를 실행하기 위해서는 아래 조건들이 선행이 되어야 한다.  
  
  1. OrderRespository가 사용할 RDB conenction 세팅(위 코드에서는 간단하게 print 문으로 대체했지만 실제로는 연결이 필요)
  2. RDB에 로직 테스트 조건에 맞는 데이터 세팅
  3. NotificationClient가 사용할 Notification Server 연결 
  4. Notification이 성공했을 때의 데이터 롤백 처리

테스트할 간단한 로직을 위해서 세팅할 사항만 해도 너무 많다.  
사실 로직이 간단해서 이 정도이고 여러 자원을 연결할수록 사전 작업은 많아지게 된다.  
  
### OrderService.createOrder()는 DB나 Noti가 어떻게 됬는지는 관심이 없다.
테스트를 실행하기 위해 위 항목들을 세팅하지만 정작 OrderService의 createOrder()에서는  
위 세팅 사항들, DB에 데이터가 제대로 들어갔는지, noti가 정상적으로 갔는지는 관심사가 아니다.  
  
OrderService의 createOrder()의 실제 관심사는 아래 항목들이다.  
 
 1. orderRepository.findOrderList()의 결과가 존재할 때 OrderDuplicateException이 발생하는지
 2. orderRepository.createOrder()가 1번 실행됬는지
 3. isNotify에 따라서 notificationClient.notifyToMobile()가 실행되는지
 
### 실제 관심사들을 테스트하기 위한 상황 설정
위의 실제 관심사들에 대해 테스트하기 위해서는 다음과 같은 상황 설정이 필요하다.(DB, Noti Server 세팅해야 하는 상황)  

 1. orderRepository.findOrderList()의 결과가 존재할 때 OrderDuplicationException이 발생하는지
    -> orderRepository.findOrderList가 빈 list를 return하는 상황
 
 2. orderRepository.createOrder()가 1번 실행됬는지
    -> orderRepository.createOrder가 실행될 때는 아무 동작 없이 성공했다고 가정하는 상황
 
 3. isNotify에 따라서 notificationClient.notifyToMobile()가 실행되는지
    -> notificationClient.notifyToMobile()이 실행될 때는 아무 동작없이 성공했다고 가정하는 상황
 
위 상황들을 DB와 Notificaiton Server에 미리 세팅해놓고 테스트 돌릴 때마다 다시 세팅해주는 것은 생각만 해도 너무 번거로운 일이다.  
이런 문제 영역을 메서드의 실제 내부 동작은 실행되지 않고 상황 설정만 할 수 있도록 해결한 것이 Test Double이다.  
  
## Java의 Test Double : Mockito
### 첫 번째 Refactoring : Mockito 적용
  
Mockito를 이용하여 Test Double이 적용되어 첫번째 Refactoring된 테스트 코드는 아래와 같다.  

```java
public class MockitoTests {
   private OrderService orderService;
   
   @Test
   public void createOrderTest() {
      //given
      //Mockito.mock()을 이용하여 mock() 객체 생성
      OrderRepository orderRepository = Mockito.mock(OrderRespository.class);
      NotificationClient notificationClient = Mockito.mock(Notification.class);
      //Mock 객체를 이용하여 OrderService 의존성 해결
      orderService = new OrderService(orderRepository, notificationClient);
      
      //Mock 객체의 기대행위 작성 : Stub
      Mockito.when(orderRepository.findOrderList()).then(invocation -> {
         System.out.println("I'm mock orderRepository");
         return Collections.emptyList();
      });
      
      Mockito.doAnswer(invocation -> {
         System.out.println("I'm mock notificationclient");
         return null;
      }).when(notificationClient).notifyToMobile();
      
      //when
      orderService.createOrder(true);
      
      //then
      Mockito.verify(orderRepository, Mockito.times(1)).createOrder();
      Mockito.verify(notificationClient, Mockito.times(1)).notifyToMobile();
   }
}
```

 1. Mockito.mock()을 이용하면 테스트 코드에서 행위 조작이 가능하고 형태만 같은 껍데기 mock 객체를 생성할 수 있다.
 2. 기존 실제 OrderRepository와 NotificationClient를 생성해서 의존성을 해결했던 것과는 달리 mock 객체를 생성하여 의존성을
    해결한다.
 3. mock 객체의 orderRepository.findOrderList()와 notificationClient.notifyToMobile()에 대한 기대 행위를 작성하여 테스트에
    원하는 상황을 설정할 수 있게 되었다. 이런 기대 행위를 작성하는 것을 Stub이라고 한다.
    
### mock객체에서 stub되지 않은 메소드
첫번째 refactoring한 코드에서 의문점이 한 가지 있다.  
Mockito.mock()은 껍데기만 있는 mock 객체를 만들어 주고 stub을 해야지만 원하는 상황대로 실행되는데  
OrderRepository의 createOrder()에 대해서는 아무 stub을 지정해주지 않았는 데도 테스트는 별 무리없이 성공했다.  
  
이는 Mockito의 기본전략이 Answers.RETURNS_DEFAULTS이기 때문이다.  
Mockito는 메소드의 type별로 정의된 DEFAULT 메소드가 있다.  
Answers.RETURNS_DEFAULTS은 stub되지 않은 메서드들에 대해서는 Mockito에서 메서드 타입별로 정의된 메서드들을 실행하게 된다.  
결국 orderRepository.createOrder()는 따로 stub되지 않았기 때문에 orderRepository.createOrder() 타입인 void에 맞는  
Default 메서드가 실행되어서 아무 일없이 진행된 것이다.  
  
## @Mock
### 두 번째 Refactoring : @Mock 어노테이션으로 개선
  
Mockito.mock()을 이용하여 Test Double을 구현할 수 있지만 Mockito는 어노테이션도 지원한다.  
  
```java
@ExtendWith(MockitoExtension.class)
public class MockAnnotationTests {

  @Mock
  private OrderRepository orderRepository;
  
  @Mock
  private NotificationClient notificationClient;
  
  private OrderService orderService;
  
  @Test
  public void createOrderTest_basic() {
    //given
    orderService = new OrderService(orderRepository, notificationClient);
    
    Mockito.when(orderRepository.findOrderList()).then(invocation -> {
    ...
  }
}
```

위처럼 @Mock을 이용해서 Mockito.mock() 코드를 대체할 수 있다.  
한 가지 주의할 점은 @ExtendWith(MockitoExtension.class)를 사용해야지만 테스트 시작 전 어노테이션을 감지해서  
mock 객체를 주입하기 때문에 꼭 함께 사용해야 한다.  

### 세 번째 Refactoring : @Mock ,@InjectMocks의 조합
두 번째 Refactoring에서는 @Mock으로 mock 객체를 생성하여 주입했지만  
테스트 코드에서는 이 mock객체들을 의존성으로 활용해 직접 OrderService를 생성하고 있다.  

#### @InjectMocks
@InjectMocks라는 어노테이션을 사용한다면 해당 클래스가 필요한 의존성과 맞는 Mock 객체들을 감지하여 해당 클래스의  
객체가 만들어질 때 사용하여 객체를 만들고 해당 변수에 객체를 주입하게 된다.  

```java
@ExtendWith(MockitoExtension.class)
public class MockAndInjectMocksTests {
  @Mock
  private OrderRepository orderRepository;
  
  @Mock
  private NotificationClient notificationClient;
  
  @InjectMocks
  private OrderService orderService;
  
  @Test
  public void createOrderTest_basic() {
     //given
     Mockito.when(orderRepository.findOrderList().then(invocation -> {
        System.out.println("I'm mock orderRepository");
        return Collections.emptyList();
     }
     
     Mockito.doAnswer(invacation -> {
        System.out.println("I'm mock notificationClient");
        return null;
     }).when(notificationClient).notifyToMobile();
     
     //when
     orderService.createOrder(true);
     
     //then
     Mockito.verify(orderRepository, Mockito.times(1)).createOrder();
     Mockito.verify(notificationClient, Mockito.times(1)).notifyToMobile();
  }
}
```

### 네 번째 Refactoring : @Spy  
OrderRepository의 메서드 중 createOrder()는 stub하고 findOrderList()는 실제 기능을 그대로 사용하고 싶은 경우가 생길 수 있고  
생각보다 빈번하게 발생한다.  
  
즉, 하나의 객체를 선택적으로 stub할 수 있도록 하는 기능이 있는데 @Spy(=Mockito.spy)이다.  
아래 예시는 OrderRepository.createOrder()는 stub하고 OrderRepository.findOrderList()는 그대로 기존 기능을 사용하도록  
구현된 테스트이다. 

```java
@ExtendWith(MockitoExtension.class)
public class SpyTest {
   //@Spy로 spy객체 주입
   @Spy
   private OrderRepository orderRepository;
   
   @Spy
   private NotificationClient notificationClient;
   
   @InjectMocks
   private OrderService orderService;
   
   @Test
   public void createOrderTest_basic() {
      //given
      //spy 객체인 orderRepository의 createOrder만 stub
      //findOrderList는 stub하지 않고 기존 기능 사용
      Mockito.doAnswer(invocation -> {
         System.out.println("I'm spy orderRepository createOrder");
         return null;
      }).when(orderRespository).createOrder();
      
      Mockito.doAnswer(invocation -> {
         System.out.println("I'm spy notificationClient");
         return null;
      })when.(notificationClient).notifyToMobile();
      
      //when
      orderService.createOrder(true);
      
      //then
      Mockito.verify(orderRepository, Mockito.times(1)).createOrder();
      Mockito.verify(notificationClient, Mockito.times(1)).notifyToMobile();
    }
 }
 ```
 
  1. @Spy로 spy 객체 주입(Mockito.spy()로 직접 작성해도 된다.)
  2. spy 객체인 orderRepository에서 createOrder()만 stub하고 findOrderList()는 기존 기능을 사용하도록 상황 설정
 
## SpringBootTest & Mockito
 ### SpringBootTest 문제 영역 : @Autowired
 @SpringBootTest는 SpringBoot 컨텍스트를 이용하여 테스트를 가능하도록 해주는 어노테이션이다.  
 즉, @Autowired라는 강력한 어노테이션으로 컨텍스트에서 알아서 생성된 객체를 주입받아 테스트를 진행할 수 있도록 한다.  
 
 ```java
 @SpringBootTest
 class BasicSpringTests {
 
    @Autowired
    private OrderService orderService;
    
    @Test
    void createOrderTest() {
       orderService.createOrder(true);
    }
 }
 ```
 
 하지만 이 방식은 위 '최초 OrderService의 테스트 작성'에서와 동일한 문제점을 가지고 있으며 테스트가 실행되기 위해서   
 해야할 일도 많다.  
 
 ### SpringBootTest 첫 번째 Refactoring : @MockBean
 위에서 말한 @Mock과 비슷한 @MockBean이라는 어노테이션이 있다.  
 이름이 비슷한 것처럼 실제로도 비슷하게 동작한다.  
 하지만 @MockBean은 경로(org.springframework.boot.test.mock.mockito.MockBean)를 봐도 알 수 있듯이  
 @Mock과는 다르게 spring 영역에 있는 어노테이션이라는 것을 알 수 있다.   
 @MockBean은 스프링 컨텍스트에 mock 객체를 등록하게 되고 스프링 컨텍스트에 의해 @Autowired가 동작할 때 등록된  
 mock 객체를 사용할 수 있도록 동작한다. 
 
 ```java
 @SpringBootTest
 public class MockBeanTests {
 
   //@MockBean으로 mock 객체를 스프링 컨텍스트에 등록
   @MockBean
   private OrderRepository orderRepository;
   
   @MockBean
   private NotificationClient notificationClient;
   
   //@MockBean으로 등록한 mock 객체를 주입받아서 의존성 해결
   @Autowired
   private OrderService orderService;
   
   @Test
   public void createOrderTest_basic() {
      //given
      Mockito.when(orderRepository.findOrderList()).then(invocation -> {
         System.out.println("I'm mockBean orderRepository");
         return Collections.emptyList();
      });
      
      Mockito.doAnswer(invocation -> {
         System.out.println("I'm mockBean notificationClient");
         return null;
      }).when(notificationClient).notifyToMobile();
      
      //when
      orderService.createOrder(true);
      
      //then
      Mockito.verify(orderRepository, Mockito.times(1)).createOrder();
      Mockito.verify(notificationClient, Mockito.times(1)).notifyToMobile();
   }
 }
 ```
 
 @MockBean으로 @InjectMocks은 동작하지 않나요?  
 이제부터 @MockBean과 @Mock의 차이와 정확한 사용법에 대해 나오고 더불어 점점 헷갈려가기 시작한다.  
   
 @InjectMocks는 Mockito에 의해서 클래스가 초기화될 때 해당 클래스 안에서 정의된 mock 객체를 찾아서 의존성을 해결한다.  
 즉, @InjectMocks는 @Mock으로 가능하고 스프링 컨텍스트에 등록된 mock 객체(@MockBean)객체는 찾을 수 없다.  
   
 @MockBean은 @Autowired처럼 스프링이 제공하는 의존성 해결방법으로만 mock 객체를 찾을 수 있다.  
 
## SpringBootTest 두 번째 Refactoring : @SpyBean
위에서 나온 @MockBean에서 @Spy의 개념만 변형된 것이 @SpyBean이다.  

```java
@SpringBootTest
public class SpyBeanTests {
   
   //@SpyBean으로 spy객체를 스프링 컨텍스트에 등록
   @SpyBean
   private OrderRepository orderRepository;
   
   @SpyBean
   private NotificationClient notificationClient;
   
   //@SpyBean으로 등록한 spy 객체를 주입받아서 의존성 해결
   @Autowired
   private OrderService orderService;
   
   @Test
   public void createOrderTest_basic() {
      //given
      //findOrderList()만 stub되어 있음
      //orderRepository.createOrder()는 stub x
      Mockito.when(orderRepository.findOrderList()).then(invocation -> {
         System.out.println("I'm spy orderRepository");
         return Collections.emptyList();
      }
      
      Mockito.doAnswer(invocation -> {
         System.out.println("I'm spy notificaitonClient");
         return null;
      }).when(notificationClient).notifyToMobile();
      
      //when
      orderService.createOrder(true);
      
      //then
      Mockito.verify(OrderRepository, Mockito.times(1)).createOrder();
      Mockito.verify(notificationClient, Mockito.times(1)).notifyToMobile();
    }
 }
 ```
 ### @SpyBean 사용 시에 유의해야할 점
 @MockBean에서 Spy의 개념만 변경된 것이기 때문에 이해하기 쉬울 수도 있지만 꼭 유의해야 할 점이 있다.  
 @SpyBean이 Interface일 경우에는 해당 Interface를 구현하는 실제 구현체가 꼭 스프링 컨텍스트에 등록되어 있어야 한다.  
   
 아래 코드가 그 예이다.  
 
 ```java
 public interface SampleRepository {
    public String sampleMethod();
 }
 
 @RequiredArgsConstructor
 @Service
 public class SampleService {
    private final SampleRepository sampleRepository;
    
    public void sampleServiceMethod() {
       sampleRepository.sampleMethod();
    }
  }
  
  @SpringBootTest
  public class SpyBeanFailTests {
     @SpyBean
     //@MockBean
     private SampleRespository sampleRepository;
     
     @Autowired
     private SampleService sampleService;
     
     @Test
     public void failTest() {
        sampleRepository.sampleMethod();
        sampleService.sampleServiceMethod();
     }
  }
 ```
 테스트 코드를 실행하면 error가 난다.  
 위 예제는 SampleRepository의 구현체가 없고 interface만 있는 상태에서 @SpyBean을 사용하려고 하는 예제이다.  
   
 @SpyBean은 실제 구현된 객체를 감싸는 프록시 객체 형태이기 때문에 스프링 컨텍스트에 실제 구현체가 등록되어 있어야 한다.  
 하지만 위 예제는 스프링 컨텍스트에서 실제 구현체를 일단 꺼내와야하는데 등록된 구현체가 없으므로 SampleService의 의존성  
 또한 해결되지 않은 체로 실패하게 된다.  
   
 하지만 위 예제를 @MockBean으로 변경하게 되면 기존 스프링 컨텍스트에 등록된 구현체를 사용하는 것이 아닌 껍데기만 가진  
 mock 객체를 스프링 컨텍스트에 등록하는 것이기 때문에 SampleService의 의존성도 mock 객체로 해결되고 위 테스트는 성공하게 된다.  
   
 ### 질문
 @Mock에서 Answer.RETURNS_DEFAULTS가 디폴트 값이기 때문에, stubbing되지 않은 메서드는 원래의 메서드가 호출된다고  
 하셨는데 그러면 @Spy랑 @Mock이랑 같다고 볼 수 있는거 아닌가요?  
 @Spy는 선택적으로 stub을 할 수 있다고 했는데 ..  
    
 -> 말씀처럼 동작했다면  
 @Mock 사용시 "creatOrder success"라는 로그가 찍혔어야 되는 실행시켜보면 찾을 수 없는 걸 확인할 수 있다.  
 그리고 원래의 메서드가 호출되는 것이 아닌 본문에 적혀있는대로  
 "Mockito에서 메서드 타입별로 정의된 메소드들을 실행하게 된다."를 다르게 되어 해당 결과가 나오게 된 것이다.  
 
