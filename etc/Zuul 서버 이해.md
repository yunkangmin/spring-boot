## Zuul 서버 이행
### WHY?
Zuul 서버는 API Gateway이다.  
먼저 왜 API Gateway가 필요할까 이해하는게 중요하다.  
API Gateway는 API의 요청자인 Client(웹 어플리케이션 또는 모바일 앱)와 API의 제공자인  
backend service를 연결하는 중계자이다.   
  
우리가 부동산 중계인을 통해 거래를 하듯이, API를 사용하는 어플리케이션과 API를 제공하는  
backend service는 API Gateway를 통해 데이터를 유통한다.  
  
부동산 중계인을 이용하면 거래도 안전하고 상대방과 민감한 애기를 직접할 필요도 없으니 맘도 편하다.  
API Gateway가 필요한 이유도 안전한 API 유통과 Client 요청별로 유연하게 대처하기 위해서이다.    
  
Client 요청에 유연하게 대응한다는 의미는 Client 유형(웹 브라우저, 모바일 앱등)에 따라 맞는 API를  
연결한다거나, Client를 사용하는 사용자의 권한이나 속성에 따라 적절한 결과를 리턴한다는 것이다.  
  
API Gateway가 필요한 이유를 정리하면 아래와 같다.
- L/B & Routing : Client의 요청에 따라 적절한 backend service를 로드밸런싱하고 연결(라우팅)
- Logging : 유통되는 트래픽 로깅
- Circuit Break : Backend serivce 장애 감지 및 대처

모든 frontend의 요청을 라우팅하므로 다음의 usecase에도 활용할 수 있다.  
- 점진적으로 레거시 시스템을 신규 시스템으로 교체 : Strangle pattern 적용
- 트래픽 일부만 새로운 서비스로 라우팅 : Canari(카나리) 배포가능

참고로 오픈소스 API Gateway에는 kong, Netflix Zuul, Spring cloud gateway, ServiceComb EdgeService등이 있다.

### HOW?
Netflix zuul은 인증/인가, L/B & Routing, Logging, Circuit Break를 어떻게 제공할까?
zuul은 Java로 개발된 서버이고 커스터마이징할 수 있는 어플리케이션이다.  
class로 개발된 filter가 이용되며, overriding하여 필요한 수행을 추가할 수 있다.
보통 들어오는 요청에 대해 'pre' filter에서 인증/인가 처리를 하고  
routing filter에서 L/B, Routing, Circuit break를 처리하며  
Post filter에서 요청과 응답에 대한 Logging을 처리한다.  
L/B는 ribbon이라는 라이브러리가 사용되고, Routing은 zuul core 라이브러리가 사용되며,  
Circuit break는 Hystrix라이브러리가 사용된다.  
![image](https://user-images.githubusercontent.com/33191974/138826423-bae4d936-40fe-447f-9ab0-00a83a6e7910.png)  

전체적인 아키텍처는 일반적으로 아래와 같다.  
상품주문 항목 중 주소란 옆의 [주소찾기]를 클릭한 후, 찾을 주소 keyword를 입력하고 [검색]버튼 클릭 시  
'주소검색 마이크로서비스'의 '주소찾기 API'를 호출하는 경우를 예로 들어 설명한다.  
- Web 프로그램 또는 모바일앱에서 API를 호출 : /address/search_addr/{주소 keyword}
- zuul은 연결할 backend service의 id를 config 파일에서 읽어 라우팅

아래 예와 같이 '/order'로 시작하는 API를 호출하면 'http://order:9001/{api}/*' 를 라우팅한다.  
'/address/search_addr' API가 호출되었으므로 service id가 'address'인 마이크로서비스로 연결해야 한다.  
이때 zuul은 자동으로 eureka에서 service id가 'address'인 마이크로서비스를 찾아 그 주소로 라우팅한다.

```
zuul:
  host: 
    connect-timeout-millis: 3000
    socket-timeout-millis: 3000
  routes: 
    order: 
      path: /order/** 
      url: http://order:9001  
    address: 
      path: /address/**
      service-id: address
  retryable: true 
  ```
  ![image](https://user-images.githubusercontent.com/33191974/138827881-40e46f7a-7add-4227-bc45-62fa091ba1db.png)  
  config server는 git에서 config 파일을 읽고, config 변경시 Message broker(예: rabbitMQ, kafka등)에  
  변경내용을 반영한다.  
  그럼 각 마이크로서비스는 변경을 통지받고 config server를 통해 최신 config를 갱신하게 된다.  
  각 마이크로서비스는 ribbon을 이용하여 직접 다른 마이크로서비스를 load balancing하여 연결할 수 있다.  
  이 때 Hystrix를 통해 circuit break를 적용할 수도 있다.  
  sleuth는 zipkin server에 traffic 정보를 보내고, Zipkin 서버는 마이크로서비스 간의 tracing을 모니터링  
  할 수 있는 대시보드를 제공한다.  
  
  

 

































