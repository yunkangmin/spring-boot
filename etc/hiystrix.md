### hiystrix
hystrix는 netflix에서 만든 라이브러리로 마이크로 서비스 아키텍처에서 분산된 서비스간 통신이 원활하지 않은 경우에 각 서비스가  
장애 내성과 지연 내성을 갖게하도록 도와주는 라이브러이다.  
  
결국 키워드는 통신 문제 극복이다.  
   
### 라이브러리 적용 배경
기존의 모놀리틱 아키텍처에서는 A모듈의 A메서드에서 B모듈의 B메서드를 호출할 때, 이 메서드 호출에 실패하는 것은 아예 고려하지 않았다.  
그럴 일이 없었기 때문이다.  
그런데 마이크로 서비스 아키텍처에서는 다르다.  
주문 서비스가 배송 서비스의 API를 호출했을 때 실패할 수 있다는 것이다.  
위와 같은 상황에서 별다른 처리를 안했다면 배송 서비스에 문제가 있다는 이유로 주문 서비스도  
어디선가 문제가 생기게 될 것이고, 주문 서비스를 호출하는 어떤 서비스가 있다면 그 서비스마저도 문제가 생길 것이다.  
이렇게 마이크로 서비스에서는 각각의 서비스들이 독립적이지만, 장애가 전파되는 성질이 있다.  
그래서 이를 막기 위해서는 주문 서비스가 배송 서비스 API호출에 실패할 경우, 엑셀 파일로라도 남겨놨다가 배송할 수 있게  
전달해준다든지, 쇼핑몰 뷰어 서비스가 상품 추천 서비스 API호출에 실패할 경우, 디폴트로 상품 추천을 해준다든지 하는 일을  
해줘야 한다.  
위와 같은 일을 아주 간단한 코드만으로 처리할 수 있게 도와주는게 hystrix 라이브러리고 그 것을 스프링 부트에 연동해서 사용해 볼 것이다.  
  
### 사용법, 예제코드
테스트해보기 위해서 2개의 서비스가 필요하다.  
하나는 API caller, 하는 callee 역할  
caller 뷰어서비스  
callee 광고서비스  
  
두 개의 Spring Boot Web 프로젝트 생성하자  
application.properties에 server.port를 각각 지정해서 포트가 안겹치게 하자.  
예)  
callee 광고서비스  
server.port = 8080  
caller 뷰어서비스  
server.port = 8181  
  
왜냐하면 마이크로 서비스간의 호출이 실패했을 때 지연이나 장애로부터 극복하는게 hystrix 라이브러리의 기능이기 때문이다.  
  

```
-광고서비스
@RestController
public class AdsController {
   @GetMapping("/ads")
   public String getAds() {
      //throw new RunTimeException("My I/O Exception");
      return "정상적인 광고 리스트";
   }
}
```
   
광고 서비스에서는 별다른 설정이 필요없다. 그냥 REST API 테스트 서버로 대충 Controller 하나 만들고  
"localhost:8080/ads"를 GetMethod로 호출하면 정상적인 응답을 주는 코드이다.  
주석 처리되어있는 것을 보면 알 수 있듯이 이 서비스를 호출할 때마다 어떤 응답을 줄 지 바꾸면서 테스트할 것이다.  
정상적인 응답도 받아보고, Exception이 발생했을 때도 살펴보고, 응답이 정상적이지만 다소 시간이 걸리는 경우도  
Thread.sleep()을 통해서 해볼 것이다.  

```
-뷰어 서비스
<dependency> 
  <groupId>org.springframework.boot</groupId> 
  <artifactId>spring-boot-starter-webflux</artifactId> 
</dependency> 
<dependency> 
  <groupId>org.springframework.cloud</groupId> 
  <artifactId>spring-cloud-starter-netflix-hystrix</artifactId> 
</dependency> 
<dependency> 
  <groupId>org.springframework.cloud</groupId> 
  <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId> 
</dependency>
```

pom.xml에 dependency를 추가해준다.  
webflux는 단순히 외부 서비스를 호출하기 위한 용도로 WebClient를 써보기 위함이다.  
다른 블로그에서는 RestTemplate을 이용하는데 익숙하다면 그 것을 써도 좋다.  
그런데 레퍼런스에서 RestTemplate은 만료될 거고 WebClient를 쓰라고 하는 것 같아서 WebClient를 쓴다.  
hystrix-dashboard도 딱히 필요없지만 혹시 hystrix를 모니터링하거나 관리할 필요가 있을 때 쓸 수 있으니까  
넣어논 것이다.  

```
@EnableHystrix
@SpringBootApplication
public clss DisplayApplication {
   public static void main(String[] args) {
      SpringApplication.run(DisplayApplication.class);
   }
}
```

@SpringBootApplication이 있는 곳에 @EnableHystrix를 넣는다.  
@EnableHystrix를 적용하면 내부에 @EnableCircuitBreaker가 들어 있는데 그 것이 필요하다.  

```
@RestController
public class DisplayController {
   @Autowired
   private DisplayService displayService;
   
   @GetMapping("/ads")
   public String getAds() {
      return displayService.getAds();
   }
}
```

마찬가지로 Controller 생성해서 displayService의 getAds()를 호출하게 했다.  
  
```
@Service
public class DisplayService {
   private WebClient webClient = WebClient.builder().baseUrl("http://localhost:8080").build();
   //webClientBuilder를 가져오는 데 기본이 되는 URL는 저거로 설정하고 클라이언트를 가져옴.

   @HystrixCommand(fallbackMethod = "getAdsFallback")
   public String getAds() {
      return webClient.get() //get 방식으로 가져옴 
      .uri("/ads") //baseUrl 이후의 uri는 /ads로 설정
      .retrieve() //클라이언트 메시지를 보냄
      .bodyToMono(String.class) //body 타입은 String
      .block(); //가져왔다면 리턴
    }
    
    private String getAdsFallback() {
       return "fallback";
    }
}
```

Service는 위와 같다.  
주의 깊게 봐야할 것은 일단 @Service이다.  
Hystrix는 기본적으로 @Component와 @Service를 찾고 그 안에 있는 @HystrixCommand를 찾아 동작한다.  
따라서 반드시 @Service에다가 작성하도록 하자.  
다른 서비스를 호출하는 메서드에 @HystrixCommand를 붙여주고 속성으로 fallbackMethod의 이름을 달아줬다.  
이렇게 설정하면 "getAds()는 Hystrix의 CircuitBreaker의 기능(장애 내성)을 사용할 것이고 만약 서킷이 오픈되거나  
실패할 경우 fallbackMethod로 지정한 getAdsFallback() 메서드가 실행될 것이다"라는 뜻이다.  
  
fallbackMethod가 무엇이냐면,   
fallbackMethod는 그냥 해당 메서드 호출에 실패하면 대신 동작할 예외처리 메서드같은 것이다.  
호출에 실패한다는 것은 어떤 기준이 있어야 한다.  
3초 안에 응답이 안왔다든지, 예외가 생겼다든지 아예 서비스가 떠있지 않다든지 하는 기준 말이다.  
그 기준에 대한 설정값을 기본적으로 histrix가 디폴트로 해준 것이 있고 직접 설정할 수도 있다.  
별도의 설정을 하지 않으면 타임아웃은 1초다.   
1초내로 응답을 받지 못하면 fallbackMethod가 호출된다.  
아쉽게도 정상적으로 응답했는데 1.00001초만에 응답이 왔다면 실패다.  
경우에 따라 서비스의 응답시간이 긴 서비스들도 있으니 주의해서 설정해야 한다.  
  
서킷오픈은 무엇이냐면,  
API호출에 대한 통계를 기반으로 상대 서비스에 대한 이상이 있다고 감지하고,  
즉시 에러를 리턴해버리게 하는 상태로 만드는 것이다.  
예를 들어 10초 동안 20회 이상 API 호출이 일어난 경우에 통계를 내서 50%, 즉 반 이상 호출에 실패했다면  
호출한 서비스에 이상이 있다고 감지하고 그 이후 5초 동안에는 서비스 호출을 시도하지도 않고 "즉시" fallbackMethod를   
리턴해버리는 것이다.(서킷 오픈)  
그리고 5초 이후에는 다시 10초 동안 20회 이상 API호출이 일어났는지 이런 통게를 내는 것이 아니라 1회의 호출만 해보고  
정상으로 돌아왔으면 다시 통계를 낼 준비를 하는 것이고, 만약 1회의 호출이 또 다시 실패한다면 다시 5초 동안 fallbackMethod를  
호출하는 것이다.(서킷 하프 오픈)  
  
이 두 개의 기능(fallback, circuit open)으로 장애를 극복하는게 핵심이다.  
*참고로 fallbackMethod와 circuit open은 무관하다. 연관은 있을 수 있지만 일단은 fallbackMethod가 일어났다고 해서 서킷이  
오픈된 것이냐? 그건 또 아니기 때문이다. 서킷은 통계를 기반으로 동작하고 fallback은 실패를 기반으로 동작한다.  
  
위의 예제를 돌려보면 어떨까?  
-> localhost:8181/ads  
결과: 정상적인 광고 리스트  
  
이렇게 정상적으로 리스트를 받는 모습이다.  
그러면 아까 주석으로 했던 throw new RuntimeException()을 통해 Exception을 발생하면 어떻게 될까?  
fallbackMethod가 호출되면서 아래와 같은 결과가 나타난다.  
  
-> localhost:8181/ads  
결과: fallback  
  
그러면 이건 어떨까?   
광고서비스에서 Exception이 발생하는게 아니라 Thread.sleep(2000); 를 통해 2초 후에 정상적인 응답을 주는 것이다.  
hystrix는 기본적으로 타임아웃을 1초로 보기 때문에 응답을 2초 후에 주는 서비스가 있지만 응답을 받지 못한 것으로 보고  
fallback이 실행된다.  
  
그러면 Thread.sleep(2000);을 적은 광고 서비스는 어떻게 될까?  
(즉 localhost:8080/ads을 직접 호출)  
정답은 그냥 남은 코드도 진행하고 리턴도 해준다!  
호출한 서비스가 hystrix를 설정했는지 안했는지 상관이 없다는 것이다.  
이게 hystrix의 전부고 설정값에 대한 이해만 하면 된다.  
  
```
@Service
public class DisplayService {
   private WebClient webClient = WebClient.builder().baseUrl("http://localhost:8080").build();
   
   @HystrixCommand(commadKey = "hello", fallbackMethod = "getAdsFallback")
   public String getAds() {
      return webClient.get()
      .uri("/ads")
      .retrieve()
      .bodyToMono(String.class)
      .block();
   )
   
   @HystrixCommand(commandKey = "hello", fallbackMethod = "getAdsFallback2")
   public String test() {
      return webClient.get()
                      .uri("/hello")
                      .retrieve()
                      .bodyToMono(String.class)
                      .block();
   }
   
   private String getAdsFallback() {
      return "fallback";
   }
   
   private String getAdsFallback2() {
      return "fallback2";
   }
   
}
```

@Hystrix의 속성으로 commandKey가 있는데 따로 설정하지 않으면 메서드명으로 설정된 것으로 간주한다.  
이것은 서킷오픈할 때 같은 key를 가지는 메서드들은 같이 통계가 매겨진다.  
test()을 호출하다가 실패하면 getAdsFallback2가 실행되겠지만 실패 통계는 hello에 매겨진다는 것이다.  
  
아래와 같이 hystrixCommand 설정을 할 수 있다.  
```
import org.springframework.stereotype.Service;
import org.springframework.web.reactive.function.client.WebClient;

import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;

@Service
public class DisplayService {
   private WebClient webClient = WebClient.builder().baseUrl("http://localhost:8080").build();
   
   @HystrixCommand(commandKey = "hello", fallbackMethod = "getAdsFallback",
                   commandProperties = {
                       @HystrixProperties(name = "execution.isolation.thread.timeoutInMilliseconds", value = "3000")
                       @HystrixProperties(name = "circuitBreaker.errorThresholdPercentage", value = "10")
                       @HsytrixProperties(name = "circuitBreaker.requestVolumeThreshold", value = "5")
                       @HystrixProperties(name = "circuitBreaker.sleepWindowInMilliseconds", value = "1000")
                    })
    public String getAds() {
       return webClient.get()
       .uri("/ads")
       .retrieve()
       .bodyToMono(String.class)
       .block();
    }
    
    private String getAdsFallback() {
       return "fallback";
    }
 }
 ```
 execution.isolation.thread.timeoutInMilliseconds : 3000 -> 기본 타임아웃이 1초인 것을 3초로 늘려줌
 errorThresholdPercentage : 10 -> 통계낼 때 에러 비율로 10%이상 호출에 문제가 있으면 서킷을 열라는 의미이다.
 metrics.rollinStats.timeInMilliseconds : 10000 -> 10초 동안 통계
 requestVolumeThreshold : 5 -> 5회이상 호출되면 통계 시작
 circuitBreaker.sleepWindowMilliseconds : 10000 -> 서킷이 한번 열리면 10초 유지
 ```
 @Service
public class DisplayService {
   private WebClient webClient = WebClient.builder().baseUrl("http://localhost:8080").build();
   private static final Logger logger = LoggerFactory.getLogger(DisplayService.class);
   
   @HystrixCommand(commandKey = "hello", fallbackMethod = "getAdsFallback",
                   commandProperties = {
                       @HystrixProperties(name = "execution.isolation.thread.timeoutInMilliseconds", value = "3000")
                       @HystrixProperties(name = "circuitBreaker.errorThresholdPercentage", value = "10")
                       @HsytrixProperties(name = "circuitBreaker.requestVolumeThreshold", value = "5")
                       @HystrixProperties(name = "circuitBreaker.sleepWindowInMilliseconds", value = "1000")
                    })
    public String getAds() {
       return webClient.get()
       .uri("/ads")
       .retrieve()
       .bodyToMono(String.class)
       .block();
    }
    
    private String getAdsFallback(Throwable t) {
       logger.info(t.getMessage());
       return "fallback";
    }
 }
 ```
 아까 소스에서 조금 달라진 것이 있다.
 fallbackMethod의 파라미터로 Throwable을 입력했다.
 fallbackMethod에 Throwable을 입력하면 저절로 발생한 예외가 들어가게 된다.
 

