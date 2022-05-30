윤용성
# 11번가 Spring Cloud 기반 MSA로의 전환  
## Why MSA in 11st?   
   - 초대형 거대 Monolithic System  
   - 8년넘게 사용해온 낙후된 S/W Stack   
   - 너무나 커진 200만 라인의 공통 모듈(팀이 중복적으로 개발하지 말자)    
   IDE에 뜨지도 않음    
   - 폭증하는 트래픽으로 수년 내에 감당하기 힘들것으로 예상되는 부하  
-> 이로인해...
- 많은 개발팀의 코드를 한 번에 배포  
   - 매주 목요일 밤샘 정기 배포, 잦은 장애   
- 개발자들이 새로운 기술을 접해 볼 기회 원턴 봉쇄  
   - Java 1.6, Spring 2.x or 외주 개발 Framework.  
- IDE에 띄우는 것 조차 버거운 개발 환경.   
   - 200만 라인의 공통 코드로 인해 항상 메모리 부족  
"나의 과감한 수정은 전사 장애다"  

## 어떻게 MSA를 도입할 것인가?   
"어떻게 서버 분리를 진행할 것인가?"  
   - 잘 사용되지 않는 코드까지 수정할 필요가 있을까?  
   - 새로운 Architecture에 대한 검증과 운영 경험 축적 필요  
   - 재개발 중에도 Biz 요구사항은 계속 반영 되어야 한다.  

## Project Vine   
11번가 점진적 MSA 전환 Project  
   - Strangler Application Pattern  
   - 열대 우림 지역의 Strangler Vine에서 차용  
   - Legacy 교살 전략  
      - 새로운 Application은 독립된 API 서버로 개발  
      - Legacy와 함께 운영  
      - 위의 과정을 반복     
   
## Hybrid MSA in 11st  
   - Legacy -> Hybrid MSA   
      - 업무 도메인 별로 (혹은 더 잘게) 서버 분리  
      - Legacy 코드에서는 새로운 API 서버들을 호출   
      - 기존 코드와 새로운 API 호출은 DB Flag를 통해서 Switchable하게  

<img src="https://user-images.githubusercontent.com/33191974/169226146-7c7b651f-75c4-4ad9-b303-92172b1dfcec.png" width="50%" height="50%"/>     

## MSA 플랫폼 Solution 선정   
- MSA를 위한 Best Practice를 바르게 적용하는 것이 필요  
   - Netflix OSS  
- 개발자들이 가장 친숙해 하는 환경  
   - Spring Boot  
사이의 교집합을 Spring Cloud Neflix라고 부른다.     

<img src="https://user-images.githubusercontent.com/33191974/169226583-dd6b9e8c-5c5e-486f-9241-47f2ec73f94b.png" width="50%" height="50%"/>   

## Deep Dive into(깊이 잠수)   
Hystrix, Ribbon, Eureka  

## Hystrix  
Netflix가 만든 Fault Tolerance Library    
   - 장애 전파 방지 & Resilience  

기능적 관점에서 본 Hystrix의 주요 4가지 기능   
- Circuit Breaker  
- Fallback  
- Thread Isolation   
- Timeout  

## Hystrix 적용하기   
Hystrix Annotation 적용  
- Hystrix Javanica, Spring Cloud Netflix에 포함되어 있음   
```java
@HystrixCommand  
public String anyMethodWithExternalDependency() {
    // REST API로 다른 서버 호출   
}
```
HystrixCommand 상속   
```java
public class SampleCommand extends HystrixCommand<String> {
    @Override
    protected String run() {
        // REST API로 다른 서버 호출  
    }
}
```

## Hystrix Command를 호출할 때 벌어지는 일  
1. 이 메소드를 Intercept하여 대신 실행한다.    
   - Thread Isolation  
2. 메소드의 실행결과 성공 혹은 실패(Exception) 발생여부를 기록하고  
통계를 낸다. 통계에 따라 Circuit Open 여부를 결정한다.   
   - Circuit Breaker   
3. 실패한 경우(Exception) 사용자가 제공한 메소드를 대신 실행한다.   
   - Fallback   
4. 특정시간 동안 메소드가 종료되지 않는 경우 Exception을 발생시킨다.   
   - Timeout  

## Hystrix - Circuit Breaker  
- (1)일정시간 동안 (2) 일정 개수 이상의 호출이 발생한 경우, (3)   
일정 비율 이상의 에러가 발생하면 --> Circuit Open(호출차단)  
- (4)일정 시간 경과 후에 단 한 개의 요청에 대해서 호출을 허용하며  
(Half Oopen), 이 호출이 성공하면 --> Circuit Close(호출허용)  
hystrix.command.<commandKey>.  
(1)metrics.rollingStats.timeInMilliseconds - Default 10초  
(2)circuitBreaker.requestVolumeThreshold - Default 20개   
(3)circuitBreaker.errorThresholdPercentage - Default 50%  
(4)circuitBreaker.sleepWindowInMilliseconds - Default 5초  
"10초간 20개 이상의 호출이 발생한 경우, 50% 이상의 에러가 발생하면  
5초간 Circuit Open"  

## Hystrix - Circuit Breaker의 단위   
Hystrix Circuit Breaker는 한 프로세스 내에서 주어진 CommandKey  
단위로 통계를 내고 동작한다.   
- 즉, Circuit Breaker는 CommandKey 단위로 생성된다.  
```java
@HystrixCommand(commandKey = "ExtDep1")  
public String anyMethodWithExternalDependency1() {
    //추천 서버 호출 = 상품 추천 #1
}
@HystrixCommand(commandKey = "ExtDep1")   
public String anyMethodWithExternalDependency2() {
    //추천 서버 호출 - 상품 추천 #2
}
```
"동일한 Dependency를 갖는 Hystrix Command들을 동일한 CommandKey로  
묶는 것을 고려해야 함"  

## Hystrix - Fallback   
Fallback으로 지정된 메소드는 다음의 경우에 원본 메소드 대신 실행된다.   
- Circuit Open  
- Any Exception(HystrixBadRequestException 제외)   
- Semaphore / ThreadPoolRejection(나중 Isolation에서 설명)   
- Timeout   
```java
@HystrixCommand(commandKey = "ExtDep1",
                fallbackmethod="recommendFallback")   
public String anyMethodWithExternalDependency1() {
    //추천 서버 호출  
}                
public String recommendFallback() {
    return "<미리 준비된 상품 목록>";
}
```

## HystrixBadRequestException(사용자 에러)  
"사용자의 코드에서 HystrixBadRequestException을 발생시키면, 이 오류는  
Fallback을 실행하지 않으며, Circuit Open을 위한 통계에도 집계되지   
않는다."  
Method Caller의 실수(ex 잘못된 파라미터의 전달)의 경우  
HystrixBadRequestException을 Throw하게 작성 필요  
  
만약 Caller의 실수를 다른 Exception으로 던진다면...   
   - Circuit Breaker의 통계에 집계되어 Caller의 잘못으로 Circuit이  
   Open  
   - Fallback이 실행되어, 개발과정에 오류를 인지하지 못할 수 있다.  

## Hystrix - Timeout   
Hystrix에서는 Circuit Breaker 단위로 (CommandKey 단위로) Timeout을  
설정할 수 있다.  
```
hystrix.command.<commandKey>.
execution.isolation.thread.timeoutInMilliseconds   
  - Default 1초  
```  

Circuit Breaker의 Timeout 사용시 주의 사항  
   - Semaphore isolation인 경우 - 제 시간에 Timeout이 발생하지 않는  
   경우가 대부분이다.   
   - Default 값이 매우 짧다 - 1초   

## Hystrix - Isolation(격리)     
두 가지 Isolation 방식을 Circuit Breaker 별로 지정 가능   
   - Semaphore   
   - Thread(default) 

## Hystrix - Semaphore Isolation   

<img src="https://user-images.githubusercontent.com/33191974/169236974-864dded0-ab39-4ecd-9104-725eac64b389.png" width="50%" height="50%"/>  

- Circuit Breaker 1개당 1개의 Semaphore 생성   
- Semaphore 별로 최대 동시 요청 개수 지정   
- 최대 개수 초과 시 Semaphore Rejection 발생 - Fallback 실행   
- Command를 호출한 Caller Thread에서 메소드 실행   
* Timeout이 제 시간에 발생하지 못함   

## Hystrix - Thread Isolation   

<img src="https://user-images.githubusercontent.com/33191974/169240162-1b50641e-d409-4cde-978e-3dc246a03504.png" width="50%" height="50%"/>   

- Circuit Breaker 별로 사용할 Thread Pool을 지정(ThreadPoolKey)   
- Circuit Breaker : Thread Pool = N : 1 관계 가능   
- 최대 개수 초과시 Thread Pool Rejection 발생 - Fallback 실행  
- Command를 호출한 Thread가 아닌 Thread Pool에서 메소드 실행   
"실제 메소드의 실행은 다른 Thread에서 실행되므로 Thread Local 사용 시  
주의 필요"   

## Ribbon   
Neflix가 만든 SoftWare Load Balancer를 내장한 RPC(REST) Libaray    
- Client Load Balancer with HTTP Client   

<img src="https://user-images.githubusercontent.com/33191974/169242045-dbc5397f-3cc4-4338-9e64-03fefb4c8ad1.png" width="50%" height="50%"/>  

## Ribbon in Spring Cloud   
Spring Cloud에서는 Ribbon 클라이언트를 사용자가 직접 사용하지 않음   
- Spring Cloud의 HTTP 통신이 필요한 요소에 내장되어 있음   
   - Zuul API Gateway  
   - Rest Template(@LocalBalanced)  
   - Spring Cloud Feign - 선언적 Http Client  

## Ribbon이 기존 Load Balancer와 다른 점   
Ribbon은 대부분의 동작은 Programmable 함.   
Spring Cloud에서는 아래와 같은 BeanType으로   
  
IRule - 주어진 서버 목록에서 어떤 서버를 선택할 것인가  
IPing - 각 서버가 살아있는 가를 검사   
ServerList<Server> - 대상 서버 목록 제공   
ServerListFilter<Server> - 대상 서버들 중 호출할 대상 Filter   
ServerListUpdater  
IClientConfig   
ILoadBalancer   

## Eureka   
Netflix가 만든 Dynamic Service Discovery(동적 서비스 검색)      
   - 등록 : 서버가 자신의 서비스 이름(종류)와 IP 주소, 포트를 등록   
   - 조회 : 서비스 이름(종류)을 갖고 서버 목록을 조회   

<img src="https://user-images.githubusercontent.com/33191974/169248117-14f62fd3-7e69-45d0-956e-96b328939c16.png" width="50%" height="50%"/>  

## Eureka in Spring Cloud   
Eureka가 Enable된 Spring Cloud Application은   
- Server 시작 시 Eureka 서버에 자동으로 자신의 상태 등록(UP)   
- 주기적 HeartBeat으로 Eureka Server에 자신이 살아 있음을 알림     
- Server 종료 시 Eureka 서버에 자신의 상태 변경(DOWN) 혹은 자신의  
목록 삭제   
- Eureka 상에 등록된 이름은 'spring.application.name'  

## Eureka + Ribbon in Spring Cloud  
하나의 서버에 Eureka Client와 Ribbon Client가 함께 설정되면 Spring   
Cloud는 다음의 Ribbon Bean을 대체   
1. ServerList<Server>   
   - 기본 : ConfigurationBasedServerList   
   - 변경 : DiscoveryEnabledNIWServerList   
2. IPing   
   - 기본 : DummyPing   
   - 변경 : NIWDiscoveryPing   
서버의 목록을 설정으로 명시하는 대신 Eureka를 통해서 Look Up해오는  
구현   

"이 세가지를 잘 적용하면 MSA 환경에서 Server들 간의 통신을 큰 문제없이  
잘 될 것 같다는 생각..."   

# Hystrix, Ribbon, Eureka 적용 in 11st   
## API Gateway   
MSA 환경에서 API Gateway의 필요성   
- Single Endpoint 제공   
   * API를 사용할 Client 들은 API Gateway 주소만 인지   
- API의 공통 로직 구현   
   *Logging, Authentication, Authorization   
- Traffic Control   
   *API Quota, Throttling    

## 자체 개발 VS Zuul   
SK planet은 자체 개발 API Gateway를 상용 운영중   
- 11번가를 제외한 서비스의 Open API 제공  
- Node.js / Koa Framework 기반   
- 100% 자체 개발 from Scratch   
- Logging, Authorization, Throttling 등..  
"100% 자체 개발한 API Gateway를 두고 Zuul을 도입해야 할까?"  

## Spring Cloud Zuul   
Spring Cloud Zuul은 API Routing을 Hystrix, Ribbon, Eureka를 통해서 구현   
- Spring Cloud와 가장 잘 Integration 되어 있는 API Gateway   
  
<img src="https://user-images.githubusercontent.com/33191974/169254182-9c170dcd-abe8-475e-9c26-01a29c98ddec.png" width="50%" height="50%"/>   

## Hystrix, Ribbon, Eureka in Spring Cloud Zuul   
    
<img src="https://user-images.githubusercontent.com/33191974/169255383-6882af3e-090e-4160-a156-453450644c01.png" width="50%" height="50%"/>   

## Hystrix Circuit Breaker in Spring Cloud Zuul   
Reminder   
- Hystrix Circuit Breaker는 CommandKey 이름 단위로 동작한다.   
Spring Cloud Zuul에서 Hystrix CommandKey는   
- 각 **서버군 이름**(Zuul 용어로 Service ID, Eureka에 등록된 서버군 이름)  
이 사용됨  
- 앞선 그림을 예를 들면 3개의 CommandKey가 사용   
hystrix.command.[product|display|member].xxxxxx

## Hystrix Isolation in Spring Cloud Zuul  
Reminder   
- Hystrix Isolation은 Semaphore/Thread 두가지 모드가 있음  
- Semaphore는 Circuit Breaker와 1:1   
- ThreadPool은 별도 부여된 ThreadPoolKey 단위로 생성된다.   
  
Spring Cloud Zuul에서 Hystrix Isolation은   
- Semaphore Isolation을 기본으로 한다.  
- Hystrix의 원래 Default는 Thread Isolation   

## Hystrix Isolation in Spring Cloud Zuul  

<img src="https://user-images.githubusercontent.com/33191974/169272884-89e799f4-2c7b-474c-a647-2621c9ae2408.png" width="50%" height="50%"/>   
  
Spring Cloud Zuul의 기본 설정으로는 Semaphore Isolation   
- 특정 API 군의 장애(지연)등이 발생하여도 Zuul 자체의 장애로 이어지지   
않음   

Spring Cloud Zuul에서 우리는 Thread Isolation을 사용하고 싶다는 생각...  
  
Thread Isolation을 통해서  
- Hystrix Timeout을 통해서 특정 서버군의 장애시에도 Zuul의 Container  
Worker Thread의 원활한 반환   
```
zuul: 
    ribbon-isolation-strategy: thread   
```    

<img src="https://user-images.githubusercontent.com/33191974/169275228-03a56a1e-6358-4d2d-9caa-7743b9595201.png" width="50%" height="50%"/>  

Spring Cloud Zuul의 Hystrix Isolation을 Thread로 바꾸면   
- Server 전체의 한 개의 Thread Pool이 생겨버리게 구현   
- 'RibbonCommand'라는 이름의 ThreadPool  
"이건 우리가 원하던 모습이 아니다"   
- 서버군(Service ID) 별로 Thread Pool을 분리해주는 것이 필요하다는 팀의  
판단  
- Spring Cloud Netflix 프로젝트에 Pull Request를 통해 해당 코드 수정   
  
zuul:  
    threadPool:
        useSeparateThreadPools: true
        threadPoolKeyPrefix: zuulgw  

<img src="https://user-images.githubusercontent.com/33191974/169275851-39ba2ae4-3e59-426a-b3b8-82af6f1e7e9b.png" width="50%" height="50%"/>    

## Server to Server 호출 in MSA  
MSA 플랫폼 외부의 호출은 API Gateway를 단일 창구로 사용   
"MSA 플랫폼 내부의 API Server 간의 호출은 어떻게 할 것인가?"   

<img src="https://user-images.githubusercontent.com/33191974/169276438-09d0ddc5-2812-4933-8e60-cd06058feb9c.png" width="50%" height="50%"/>  

"Ribbon + Eureka 조합으로 Peer to Peer 호출을 하기로 결정"   
Spring Cloud에서는 다음의 Ribbon + Eureka 기반의 Http 호출 방법을  
제공   
- @LoadBalanced Rest Template   
   * @LoadBalanced 주석을 붙이는 경우 RestTemplate이 Ribbon + Eureka  
   기능을 갖게 됨   
   * RestTemplate이 Bean으로 선언된 것만 적용 가능   
- Spring Cloud Feign   

## Spring Cloud Feign   
Declarative Http Client   
- Java Interface + Spring MVC Annotation 선언으로 Http 호출이 가능한   
Spring Bean을 자동 생성   
- OpenFeign 기반의 Spring Cloud 확장  
- Hystrix + Ribbon + Eureka와 연동되어있음   

## Spring Cloud Feign with Eureka and Ribbon   
Ribbon + Eureka 없이 사용하기   
```java
//url 직접 명시   
@FeignClient(name = "product", url = "http://localhost:8081")
public interface FeignProductRemoteService {
    
    @RequestMapping(path = "/products/{productId}")  
    String getProductInfo(@PathVariable("productId") String productId);
}
```
Ribbon + Eureka 연동   
```java
//url 생략  
@FeignClient(name = "product")   
public interface FeignProductRemoteService {

    @RequestMapping(path = "/products/{productId}")  
    String getProductInfo(@PathVariable("productId") String productId);
}
```

## Spring Cloud Feign with Hystrix   
Hystrix 연동하기   
- Hystrix가 Classpath에 존재   
- feign.hystrix.enabled=true(메서드 하나하나에 서킷브레이커가 붙는다)    

## Spring Cloud Feign with Hystrix, Ribbon, Eureka   
- 호출하는 모든 메소드는 Hystrix Command로 실행   
   *Circuit Breaker, Timeout, Isolation, Fallback 적용   
- 호출할 서버는 Eureka를 통해 얻어서, Ribbon으로 Load Balancing되어 호출   

<img src="https://user-images.githubusercontent.com/33191974/169279835-cc647cc1-7b06-4c06-8391-0542a143cff4.png" width="50%" height="50%"/>  

## 장애 시나리오   
"Hystrix, Eureka, Ribbon을 사용한 우리 System은 어느 정도 Resilient(  
회복력있는)한거야?"      

<img src="https://user-images.githubusercontent.com/33191974/169280557-74f4e1ed-9c30-432d-9774-798c2b58c415.png" width="50%" height="50%"/>  

특정 API 서버의 인스턴스가 한 개 DOWN된 경우   
EUREKA - Heartbeat 송신이 중단 됨으로 일정 시간 후 목록에서 사라짐   
RIBBON - IOException이 발생한 경우 다른 인스턴스로 Retry  
Hystrix - Circuit은 오픈되지 않음(Error = 33%)  
          Fallback, Timeout은 동작  

<img src="https://user-images.githubusercontent.com/33191974/169281546-02fd1ddb-9f0c-4cf9-9d0e-a7831558a527.png" width="50%" height="50%"/>      

특정 API가 비정상 동작하는 경우(지연, 에러)  
Hystrix - 해당 API를 호출하는 Circuit Breaker 오픈 Fallback, Timout  
도 동작   

# 11번가 환경구성   
## 11st with Spring Cloud   

<img src="https://user-images.githubusercontent.com/33191974/169282005-8040f14a-616d-4317-a7f7-8080f878558b.png" width="50%" height="50%"/>  

모든 MSA Platform 내의 서버는 Eureka Client를 탑재  
API Server들간의 호출도 Spring Cloud Feign을 통해 Hystrix + Ribbon +  
Eureka 조합으로 호출   

<img src="https://user-images.githubusercontent.com/33191974/169282708-3fe69441-45e9-4110-9051-c949d7ef4682.png" width="50%" height="50%"/>  

모든 MSA Platform 내의 서버는 Spring Cloud Config Client를 탑재   
  
## Spring Cloud Config   
Git 기반의 Config 관리   
플랫폼 내의 모든 Server들은 Config Client를 탑재   
서버 시작 시 Config 서버가 제공하는 Config들이 PropertySource로 등록됨  

<img src="https://user-images.githubusercontent.com/33191974/169283470-4aa14174-bca8-456e-bac3-9f6c753867c5.png" width="50%" height="50%"/>     

<img src="https://user-images.githubusercontent.com/33191974/169283933-a83bf515-6d4a-46a9-a26d-d0fdf2258ef0.png" width="50%" height="50%"/> 

MSA 운영 환경을 위한 전용 모니터링 도구들   
- Zipkin, Turbin, Spring Boot Admin     

# 모니터링   
"기존 모니터링 시스템으로는 5%가 부족해!"  

## 분산 Tracing(추적)에서 서버간 Trace 정보의 전달   

<img src="https://user-images.githubusercontent.com/33191974/169284356-d62fa88c-d798-421c-abe1-4062c660b6aa.png" width="50%" height="50%"/>   

서버 간의 Trace 정보의 전달은 사용 Protocol의 헤더를 통해 전달 필요  
- ex) Http Header   

## In Process에서 Trace 정보의 전달 문제   

<img src="https://user-images.githubusercontent.com/33191974/169285331-efe1c932-e939-4cb4-8ae8-c918d6512578.png" width="50%" height="50%"/>   

다양한 Library에 의한 Thread 변경으로 인한 Trace 정보의 전달이 어려움  
- 단순한 Thread Local에 저장하는 방식을 사용하기 어렵다.   
- Hystrix, RxJava, @Async, ...   

## Spring Cloud Sleuth   
Spring Cloud에서 제공하는 Distributed Tracing(분산 추적) 솔루션   
- 대부분의 내외부 호출 구간에서 Trace 정보를 생성 및 전달   
- Log에 남기거나 수집 서버(Zipkin)에 전송하여 검색/시각화   

Spring Cloud Sleuth가 지원되는 Component?  
- Zuul, Servlet, RestTemplate, Hystrix, Feign, RxJava  

## Spring Cloud Sleuth - Logging  
Spring Cloud Sleuth 사용시 Application 로그에 Trace ID(UUID)가 함께   
출력   

<img src="https://user-images.githubusercontent.com/33191974/169294452-3fb66522-a40a-4b5a-b83a-7dce79dfb7b5.png" width="50%" height="50%"/>  

"로그 수집/검색 시스템이 있다면 동일 요청에 해당하는 전체 서버의 로그  
분석 가능"  

## Spring Cloud Sleuth with Zipkin  

<img src="https://user-images.githubusercontent.com/33191974/169295308-4d96db4a-0e21-43e0-aae5-46449317f830.png" width="50%" height="50%"/>  

## DB 호출 구간을 위한 간단한 확장  

<img src="https://user-images.githubusercontent.com/33191974/169295750-136d67ed-62ff-4975-a30d-4c6f877c7482.png" width="50%" height="50%"/>    





--- 

# 분산추적, Distributed Tracing   
클라우드 환경이 대세가 되면서 자연스럽게 MSA의 사용량 역시 증가하게 되었고  
증가한 만큼 우리는 수많은 마이크로서비스들의 트랜잭션에 대해 추적을 할  
필요가 생겼다.   
  
기존의 모노리틱 아키텍체에서는 하나의 애플리케이션 안에서 모든 동작이   
이루어졌기 때문에 트랜잭션의 추적이 아주 쉬웠지만 MSA 아키텍처의 사용으로  
수많은 트랜잭션을 로깅한다는 것은 매우 어려운 일이었다.  
  
이 때 등장한 개념이 분산추적(Distributed Tracing)이다.   
  
분산추적이란 하나의 애플리케이션을 구성하는 모든 MSA 사이에서 발생하는  
트랜잭션을 추적하고 분석하는 것이다.   
  
<img src="https://user-images.githubusercontent.com/33191974/169291751-8956d0de-b9f5-4723-9471-ad8dd3f2e2fe.png" width="50%" height="50%"/>    

## 분산추적을 사용하는 이유  
데브옵스 환경에서 프로젝트를 진행하는 팀이라면 분산추적을 이용하여 애플  
리케이션을 모니터링할 수 있다. MSA 개발환경에서의 최적화된 기술이라고  
할 수 있다.     
 
---

마이크로서비스는 여러 개의 서비스가 분산되어있기 때문에 각 서비스들의   
로그를 한 곳에 모아 이들의 관계를 분석하고 한 눈에 서비스의 로그들을   
볼 수 있어야 한다.   

모놀리식 아키텍처와 달리 마이크로 서비스는 하나의 호출이 gateway로   
들어오면 내부 로직에 따라서 여러 마이크로서버를 거친 후에 응답을 하게  
되는 경우가 빈번하다. 이런 경우에 어떤 서비스를 순서대로 거치는지,  
그 과정에서 어떤 서비스에서 문제가 생기는 것인지를 파악할 수 있어야 한다.  
  
트랜잭션이 여러 컴포넌트의 조합을 통해서 발생하기 때문에 전통적인 APM(  
Application Performance Monitoring) 도구를 이용해서 추적하기가 어려워  
`분산 로그 추적 시스템`이 생겨났다고 한다. 그 중에서도 대표적인 오픈소스  
인 `zipkin`이 있다.  






























































