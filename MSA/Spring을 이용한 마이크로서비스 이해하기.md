본 포스팅에서는 Springframework와 MSA의 관계에 대해 살펴본다.  
  
Springframework은 자바 기간 애플리케이션을 구축하는 사실상 표준 개발 프레임워크가  
되었다. Spring은 의존성 주입(Dependency injection)이라는 핵심 개념에 기반을 두고 있다.  
일반적인 자바 애플리케이션이 각 클래스가 애플리케이션의 다른 클래스와 명시적으로 링크된  
클래스로 분해된다. 링크는 코드에서 클래스 생성자를 직접 호출하는 것으로, 일단 코드가 컴파일  
되면 이러한 링크 지점은 변경될 수 없다. 이러한 이유로 DI를 활용한 개발 방법으로 다양한 자바  
클래스 사이의 매개체 역할을 수행하며 클래스 의존성을 관리하게 되었다.(xml파일만 수정하면 됨)  
  
스프링의 기능을 빠르게 추가해 사용성을 높인 덕분에, 엔터프라이즈 자바 애플리케이션을 개발하는  
개발자에게 더 가벼운 대안으로 빠르게 자리 잡았다. 다만 스프링 개발팀은 여전히 사용하지 않는  
블로트웨어(쓸데없이 메모리를 너무 많이 잡아먹는 프로그램)를 고민하게 되었다.  
  
스프링 개발팀은 많은 개발팀이 애플리케이션의 프레젠테이션과 비즈니스, 데이터 액세스 로직을  
함께 패키징하고 단일 산출물로 배포하는 모놀리식 애플리케이션에서 마이크로서비스로 전환되고  
있는 현 추세를 이해하고, Springframework을 작고 분산되어 클라우드에 쉽게 배포가능한 서비스를  
구축하려는 고도의 분산 모델로 이동하고 있다. 
  
스프링 부트는 스프링 프레임워크를 재구성한 프로젝트이다.  
스프링의 핵심 기능은 수용하면서도 많은 엔터프라이즈 기능을 제공하고 자바 기반의 REST 지향   
마이크로서비스 프레임워크를 제공한다. 스프링 부트의 가벼운 구조는 이후 클라우드 환경에 적용하기  
적절한 형태의 프레임워크로 성장한다.  
스프링 클라우드는 마이크로서비스가 일반적인 애플리케이션을 구축하는 패턴 중 하나로 성장하며  
Public, Private 클라우드에 마이크로서비스를 쉽게 운영하고 배포할 수 있게 해준다.
  
앞으로 다수의 시간동안 스프링 마이크로 서비스에 대해 포스팅 할 예정이다.
스프링 부트와 스프링 클라우드를 사용해 Public, Private 클라우드에 배포할 수 있는 마이크로서비스 
기반 애플리케이션을 구축해 볼 예정이다.   
다양한 요소를 다루겠지만, 특히 마이크로서비스를 설계하기 위한 요소(Inner, Outer, 방법론, MSA 대상  
결정), 실제 스프링 부트 프레임워크 설계과정, 다양한 리소스 동작 패턴 분석 및 배포를 위한 CI/CD 등을  
살펴볼 예정이다. 
  
사실 마이크로서비스를 이야기할 때 어느 하나의 언어 종속성을 방지할 수 있는 폴리그랏(polyglot)을  
이야기하고 이것은 프레임워크 종속성이 문제를 종종 야기하곤하지만 그럼에도 불구하고 스프링 부트의  
강점은 버릴 수 없는 강점들을 가지고 있다.  
  

## 스프링 부트 마이크로서비스 구축
스프링 부트의 가벼운 장점을 통해 클라우드 환경에 적용하기 용이하다는 점은 이미 살펴보았다.  
지금부터는 실제 마이크로서비스가 담당하는 일과 사용자가 요청을 할 경우 어떠한 흐름으로 처리되는지  
알아보도록 하자. 
  
1. HTTP GET 요청 : 클라이언트가 hello 마이크로서비스에 HTTP GET 요청을 전송한다.  
(http://localhost:8080/hello/nara/son)
2. 스프링 부트 마이크로서비스 흐름
   1. 경로 매핑 : 스프링 부트는 HTTP 요청을 파싱하고 HTTP 동사와 URL, URL에 정의된  
   매개변수를 기반으로 경로를 매핑한다. 경로는 스프링 RestController 클래스의 메서드에  
   매핑된다.
   2. 매개변수 분해 : 스프링 부트가 경로를 인식하면 경로 내부에 정의된 매개변수를 작업을  
   수행할 자바 메서드에 매핑한다.  
   3. JSON -> 자바 객체 매핑 : HTTP PUT이나 POST는 HTTP 본문(body)에서 전달된 JSON을  
   자바 클래스에 매핑한다. 
   4. 비즈니스 로직 실행 : 모든 데이터가 매핑되면 스프링 부트는 비즈니스 로직을 전달한다.  
   5. 자바 -> JSON 객체 매핑 : 비즈니스 로직이 실행되면 스프링 부트는 자바 객체를 JSON으로  
   변환한다.  
3. HTTP GET 응답 : 클라이언트가 서비스에서 JSON으로 응답을 받는다. 호출 성공과 실패는 HTTP  
상태 코드로 반환된다.  
(HTTP STATUS:200{"message":"Hello nara son"})

```
@SpringBootApplication
//스프링 부트에 이 클래스의 코드를 스프링 RestController 클래스로 노출하도록 지정한다.
@RestController
@RequestMapping(Value = "hello")
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }
  
  @RequestMapping(value = "/{firstName}/{lastName}", method = RequestMethod.GET)
  public String hello(@PathVariable("firstName") String firstName,
                      @PathVariable("lastName") String lastName) {
    return String.format("{\"message\":\"Hello %s %s\"}", firstName, lastName);
  }
}
```


## 마이크로서비스를 도입하는 이유
SOA 사상이 등장한지도 얼마 지나지 않아 MSA 사상이 등장하게 되었다.  
빅데이터 기반의 AI, 딥러닝, 머신러닝등 새로운 신기술들이 클라우드 사상을 뒷받침하는  
대용량 트래픽을 기반으로하다보니 이를 처리하기 위한 사항으로 등장하게 된 것이 SOA 사상이고  
이를 조금 더 세분화한 사상이 마이크로서비스 MSA이다.  
마이크로서비스 사상의 개발 방법론은 아래와 같은 주요사항을 기반으로 진행한다.
1. 업무의 복잡성 : 기존의 [모놀로식 개발 방식](https://lion-king.tistory.com/entry/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C-%EC%84%9C%EB%B9%84%EC%8A%A4-vs-%EB%AA%A8%EB%86%80%EB%A6%AC%EC%8B%9D-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-MicroService-vs-Monolithic-Architecture-%EA%B0%84%EB%8B%A8-%EC%86%8C%EA%B0%9C-%EB%B0%8F-%EC%A3%BC%EA%B4%80%EC%A0%81-%EC%9D%98%EA%B2%AC)을 더 이상 표준이 아니다.   
개발된 서비스는 다른 서비스와 통합되어 서로 통신하기를 원한다.
2. 빠른 배포 : 빠른 시간 내의 원하는 서비스에 대한 빌드 및 배포를 원하며, 전체를 배포하는 정기 배포와  
다른 비정기 배포의 위험 범위를 줄이고자 노력한다. 이를 위해 새로운 기능을 신속하게 제공하도록  
분리된 서비스를 구성하고 재배치할 수 있다.  
3. 성능 및 확장성 : 늘어나는 유입량을 처리하기 위한 오케스트레이션 그리고 이를 통한 성능 향상을 원하며  
반대로 사용하지 않을 경우 자원을 반납하는 기능을 요구한다. 수평적 확장을 위한 작은 서비스 단위를  
활용할 수 있다. 
4. 무중단 서비스 : 장애 발생 시 Automation된 처리 과정을 제공하여 무중단 서비스를 제공하기를 원한다.  
분리된 서비스를 통해 한 부분의 장애로 인해 전체로 확장되는 문제를 줄일 수 있다. 

## 정리
즉 마이크로서비스를 이야기할 때  
작고(small) 단순하며(simple) 분리된(decoupled) 서비스 = 확장 가능하고(scalable) 회복적이며(resilient)  
유연한(flexible) 애플리케이션


## 스프링 마이크로서비스 패턴
견고한 마이크로서비스 아키텍처를 만들기 위해서는 반드시 설계해야 하는 분야가 있다. 
사실 이 것은 마이크로서비스가 등장하기 이전부터 원해왔던 당연한 요구사항이지만, 마이크로서비스  
사상에서 본격적으로 반영되기 시작했다고 볼 수 있다. 

>"바로 적정 크기를 유지하고 있는가? 오토 스케일을 관리할 수 있는 주체가 있는가? 장애 발생 서버를  
>빠르게 회복할 수 있는가? 개발/검증/운영 서버를 동일한 싱크로 유지하는 것이 손쉽게 가능한가?  
>비동기 프로세싱과 이벤트를 사용해 의존성을 최소화하고 확장 가능한가?"

사실 너무나 원하는 이상적인 애플리케이션구조이다 보니 한 번쯤 개발, 구축, 운영하는 입장에서 이를  
고민해본 경험이 있을것이다. 마이크로서비스는 이를 고민하고 배치한 실전 아키텍처라고 볼 수 있다. 
지금부터는 이러한 기술을 아키텍처에 반영하기 위해 고민해야 할 6가지 패턴에 대해 다뤄보도록 한다.  

1. 마이크로서비스 핵심 개발 패턴    
마이크로서비스를 설계할 때 서비스가 어떻게 소비되고 통신되는지 생각해봐야 한다.   
![image](https://user-images.githubusercontent.com/33191974/143824154-269a6717-ee26-479d-9535-4afbbb2018cd.png)
   1. 서비스 세분성 : 비즈니스 영역을 마이크로서비스로 분해해서 각 마이크로서비스가 적정 수준의  
   책임을 나눠 갖게 하는 방법은 무엇인가? 너무 넓게 나누면 유지보수와 변경에 어려움이 증가하고  
   너무 좁게 나누면 복잡성이 증가한다. 이에 대한 기준을 세우는 것이 중요한 이유이다. 
   2. [통신 프로토콜](https://coding-factory.tistory.com/346) : 마이크로서비스와 데이터 교환을 위한 XML이나 JSON, 아니면 Thrift같은 바이너리 
   프로토콜을 사용하는가? JSON이 가장 이상적인 이유와 마이크로서비스와 데이터를 교환하는데 일반적으로  
   선택되는 이유는 무엇일까?
   3. 인터페이스 설계 : 개발자가 서비스 호출에 사용하는 실제 서비스 인터페이스를 설계하는 최선의  
   방법은 무엇인가? **서비스 의도를 전달하도록 서비스 URL을 구조화하는 방법은 무엇인가? 버전 관리는?**
   4. 서비스 간 이벤트 프로세싱 : 서비스 간 하드 코딩된 의존성을 최소화하고 애플리케이션 회복성을  
   높이기 위한 마이크로서비스를 분리하는 방법은 무엇인가?
   
2. 마이크로서비스 라우팅 패턴  
[서비스 디스커버리](https://bcho.tistory.com/1252)와 라우팅은 모든 대규모 마이크로서비스 애플리케이션의 핵심이다.   
![image](https://user-images.githubusercontent.com/33191974/143826153-9319c627-562b-400e-983f-e34ee29bd49b.png)  
3. 마이크로서비스 클라이언트 회복성 패턴  
마이크로서비스를 사용하면 오작동하는 서비스에서 서비스 호출자를 보호해야 하며, 서비스 하나가  
느려지거나 다운되면 해당 서비스 이상의 중단이 발생할 수 있다는 것을 기억해야 한다.  
![image](https://user-images.githubusercontent.com/33191974/143826590-eaf05943-726f-4ffa-8dfb-1b6d31541854.png)  
4. 마이크로서비스 보안 패턴  
토큰 기반의 보안 체계를 사용하면 클라이언트의 자격 증명을 전달하지 않고 서비스 인증과 인가를 구현할  
수 있다.  
![image](https://user-images.githubusercontent.com/33191974/143826744-a5f3e590-79a6-47b3-b8ea-939bd9129d2e.png)  
  1. 인증 : 서비스를 호출하는 서비스 클라이언트가 자신이라는 것을 어떻게 알 수 있을까?
  2. 인가 : 마이크로서비스를 호출하는 서비스 클라이언트가 수행하려는 작업을 수행할 자격이 있는지  
  어떻게 알 수 있을까?
  3. 자격증명 관리와 전파 : 서비스 클라이언트가 한 트랜잭션과 관련된 여러 서비스 호출에서 자격 증명을  
  항상 제지하지 않아도 될 방법은 무엇인가? 특히 OAuth와 자바스크립트 웹 토큰(JWT)같은 토큰 기반 보안  
  표준을 사용해 토큰을 얻는 방법은 어떻게 처리되는가?
5. 마이크로서비스 로깅 및 추적 패턴  
엄밀하게 설계된 로깅 및 추적 전략으로 여러 서비스와 연관된 트랜잭션을 관리할 수 있다.   
![image](https://user-images.githubusercontent.com/33191974/143828172-aa520aa4-87f1-48c3-929d-03bdcbfe7c69.png)  
   1. 로그 상관관계 : 단일 트랜잭션에 대해 여러 서비스 간 생성된 모든 로그를 함께 연결하는 방법은  
   무엇인가?
   2. 로그 수집 : 수집된 로그를 단일 데이터베이스로 취합할 수 있을까? 취합된 로그를 검색할 수 있을까?  
   3. 마이크로서비스 추적 : 트랜잭션과 연관된 모든 서비스에서 클라이언트 트랜잭션 흐름을 시각화하고  
   서비스 성능을 측정할 수 있을까?  
6. 마이크로서비스 빌드 및 배포 패턴  
마이크로서비스와 마이크로서비스가 실행되는 서버의 배포가 환경 간 하나의 원자산출물이 되어야 한다.    
![image](https://user-images.githubusercontent.com/33191974/143829441-f891e215-a1ef-44c9-8097-a7bfc0773ec3.png)  
   1. 빌드 및 배포 파이프라인 : 조직의 모든 환경에서 원클릭 빌드와 배포를 강조하는 반복 가능한  
   빌드 및 배포 프로세스를 어떻게 구현할까?
   2. 코드형 인프라스트럭처 : 소스 제어를 사용해 실행하고 관리할 수 있는 코드로 서비스 프로비저닝을  
   처리하는 방법은 무엇인가?(코드로 프로비저닝하는 방법)
   3. 불변 서버 : 마이크로서비스 이미지를 생성하고 배포한 후 변경하지 못하게 하려면 어떻게 해야  
   하는가?  
   4. 피닉스 서버 : 서버가 오래 실행될수록 구성 편차가 발생할 가능성이 높아지는데 마이크로서비스를  
   실행하는 서버를 정기적으로 종료하고 [불변 이미지](https://velog.io/@seunghyeon/%EB%B6%88%EB%B3%80-%EC%9D%B8%ED%94%84%EB%9D%BC-Immutable-Infrastructure)를 재생성하려면 어떻게 해야 하는가?   
## 스프링 클라우드로 마이크로서비스 구축
스프링 클라우드는 다양한 마이크로서비스 구축 패턴을 제공한다. Pivotal, HashiCorp, Netflix같은  
오픈소스 회사의 프로젝트를 수용하며 스프링 애플리케이션에서 쉽게 설정하고 구성하게 만들어  
마이크로서비스 애플리케이션의 구축과 배포에 필요한 모든 인프라스트럭처를 구성하는데 매진하지 않고  
코드를 작성하는데 집중할 수 있도록 도와준다.   
![image](https://user-images.githubusercontent.com/33191974/143830968-62312d77-cc22-4ead-a55a-097eb85597ed.png)  
위 이미지와 같이 다양한 패턴을 구현한 스프링 클라우드 프로젝트를 서로 매핑하여 설계할 수 있다.  
다양한 기술에 대한 Overview형태로 짚어보고 실제 사용 방법에 대해서는 이후 포스팅에서 상세히  
다룬다.  
1. 스프링 부트  
스프링 부트는 마이크로서비스 구현에 필요한 핵심 기술이다. 스프링 부트로 REST 기반 마이크로서비스를  
구축하는 주요 작업을 단순화해 마이크로서비스를 상당히 쉽게 개발할 수 있다. 스프링 부트에서는 HTTP  
형식의 동사(GET, PUT, POST, DELETE 등)를 URL에 매핑하고 JSON 프로토콜을 자바 객체로 직렬화할 뿐만  
아니라 자바 예외를 표준 HTTP 에러코드에 매핑하는 작업도 아주 간편하게 처리할 수 있다.  
2. 스프링 클라우드 컨피그    
스프링 클라우드 컨피크는 중앙 집중식 서비스로 애플리케이션 구성 데이터 관리를 담당하고 애플리케이션  
데이터를 마이크로서비스와 완전히 분리하는 역할을 담당한다. 스프링 클라우드는 고유의 관리 저장소가  
존재하며, 오픈소스 프로젝트와 통합도 가능하다.(마이크로서비스를 구성하는 항목(서버, DB등))
통합 가능한 오픈소스 프로젝트는 Git, Consul이 있다.(구성관리 내용을 소스로 관리)   
유레카는 Consul과 유사한 서비스 디스커버리를 제공하는 Netflix OSS로서 유레카 내부에도 스프링 클라우드  
컨피그와 함께 사용될 수 있는 Key-Value 저장소(데이터베이스)가 존재한다.  
3. 스프링 클라우드 서비스 디스커버리  
스프링 클라우드의 서비스 디스커버리를 사용하면 서비스를 사용하는 클라이언트에 서버가 배포된 물리적  
위치를 추상화할 수 있다.서비스 Consumer는 물리적 위치보다 논리적 이름을 사용해 서버의 비즈니스 로직을  
호출하게 된다. 스프링 클라우드의 서비스 디스커버리는 서비스 인스턴스가 시작하고 종료할 때 등록, 삭제를  
처리하게 되며, 서비스 디스커비리 엔진의 구현을 위해 콘설과 유레카(@EnableEurekaClient)를 사용할 수  
있다.  
4. 스프링 클라우드 / 넷플릭스 히스트릭스와 리본  
스프링 클라우드는 [Netflix OSS](https://bravenamme.github.io/2020/07/21/msa-netflix/)와 긴밀하게 통합되어 사용할 수 있다.  
마이크로서비스 클라이언트의 회복성 패턴을 위해 스프링 클라우드는 넷플릭스 히스트릭스 라이브러리  
(@HystrixCommand(threadPoolKey="helloThreadPool")// 메서드에 대한 호출을 히스트릭스 회로 차단기로  
감싼다.)와 리본 프로젝트를 포함해서 마이크로서비스 안에서 간단히 사용할 수 있도록 제공한다. 
넷플릭스 히스트릭스 라이브러리를 사용하면 Circuit Breaker(@EnableCircuitBreaker)와 Bulkhead같은  
서비스 클라이언트의 회복성 패턴을 구현할 수 있게 한다.  
넷플릭스 리본 프로젝트는 유레카 같은 서비스 디스커버리 에이전트를 단순하게 통합할 뿐만 아니라  
Consumer가 서비스호출에 대한 클라이언트 측 부하 분산 기능도 제공한다.  
5. 스프링 클라우드 / 넷플릭스 주울  
스프링 클라우드는 Netflix Zuul 프로젝트를 사용해 마이크로서비스 애플리케이션을 위한 서비스 라우팅  
기능을 제공한다. Zuul은 서비스 요청을 대리해서 마이크로서비스에 대한 모든 호출이 한 곳을 통해 대상  
서비스에 도달하게 해주는 API Gateway이다. API Gateway는 서비스의 단일 엔드포인트를 제공하는 한편,  
공통 모듈인 인증, 인가, 필터링, 라우팅 규칙 등 표준 서비스 정책을 반영할 수 있다.  

...
   

[출처](https://waspro.tistory.com/451)
 


















































  
