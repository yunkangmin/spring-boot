부하 분산의 기본 개념과 용어에 대해 알아보자. 그리고 ELB 로드 밸런서를 생성한 뒤   
EC2 인스턴스를 연결하여 부하가 분산되도록 설정하는 방법, Sticky Session 기능을   
사용하는 방법에 대해 설명한다.    
  
ELB(Elastic Load Balancing)는 부하 분산과 고가용성(High Availability)을 제공하는   
서비스이다.   
  
> #### 프리티어에서 사용 가능   
> ELB는 프리티어에서 무료로 사용할 수 있다. 2014년 8월 기준으로 매달 ELB 750시간,   
> 데이터 처리 15GB를 무료로 사용할 수 있다.  

ELB는 고가의 L4/L7 장비(로드 밸런서)를 구입하거나 소프트웨어로 서버를 구축하지  
않아도 부하 분산 기능을 사용할 수 있고, 고가용성 서비스를 구축할 수 있다.   
   
예전에는 부하분산을 하려면 별도의 L4/L7 장비를 구입하거나 서버를 구입해야 했다.  
보통 회사에서는 개발 부서, 네트워크 장비 운영 부서가 모두 분리되어 있다.   
그래서 장비나 서버를 구매한다 하더라도 구매에서 설치까지 복잡한 절차와 오랜   
시간이 걸린다. 물론 비싼 장비 가격도 큰 부담이었고, 설치 및 운영도 전문가의   
도움이 필요했다. 그래서 부하 분산과 고가용성은 일부 네트워크 전문가들만의 영역  
이었다.  
  
클라우드 서비스 시대로 오면서 상황이 많이 바뀌었다. AWS에서는 클릭 몇 번만으로   
서버를 생성할 수 있는 것처럼 로드 밸런서인 ELB도 클릭 몇 번만으로 생성할 수 있다.   
설정 방법도 매우 간단해져서 네트워크 전문가에게 의존하지 않아도 된다. ELB를  
사용하면 고가의 장비 구입에 드는 비용을 절감할 수 있고, 서버 구축에 드는 노력과   
비용도 절감할 수 있다.   
  
ELB는 한 곳에 집중되는 HTTP, TCP, SSL 트래픽을 여러 EC2 인스턴스로 분산한다.  
그리고 서버가 정상적으로 가동중인지 확인(Health Check)하여 일부 EC2 인스턴스가   
중단되더라도 트래픽을 정상 EC2 인스턴스로만 보낸다. 이처럼 ELB로 부하를 분산하고  
고가용성 서비스를 구축할 수 있다.   
  
<img src="https://user-images.githubusercontent.com/33191974/157857110-3254e7be-2f71-4c8a-9662-bfaa6653947b.png" width="50%" height="50%"/>  
ELB는 리전별로 생성해야 하고, 여러 가용 영역(Availability Zone)에서 실행되는  
EC2 인스턴스로 부하를 분산시킬 수 있다. 따라서 EC2 인스턴스 한 두 개가 중단되는   
것이 아닌 가용 영역 전체가 중단되더라도 서비스를 정상적으로 제공할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157858133-5aa74e6e-cdd0-4b52-ab58-989b179b2d87.png" width="50%" height="50%"/>  

ELB는 외부 트래픽뿐만 아니라 인터넷이 연결되지 않는 내부 네트워크에서도 사용할   
수 있다.   
  
다음은 ELB 기본 개념이다.  
- L4(OSI Layer 4): OSI 레이어에서 4번째 전송 계층을 뜻한다. TCP, UDP 등의    
프로토콜이 대표적이며 포트 번호로 구분한다. 보통 OSI 레이어에서 3번째 네트워크  
계층의 IP와 묶어서 처리한다. L4 로드 밸런싱이라고 하며 IP 주소와 포트 번호를   
기준으로 트래픽을 분배한다.  
  
- L7(OSI Layer 7): OSI 레이어에서 7번째 애플리케이션 계층을 뜻한다. HTTP 프로토콜이   
대표적이다. L7 로드 밸런싱이라고 하면 HTTP 헤더의 내용을 기준으로 트래픽을 분배한다.   
  
- 로드 밸런싱 알고리즘: 트래픽을 각 EC2 인스턴스로 분배할 때 사용하는 알고리즘이다.   
ELB는 라운드 로빈(Round Robin) 알고리즘을 사용한다. 라운드 로빈은 우선 순위를   
두지 않고 순서대로 분배하는 방식이다.  
  
- 헬스 체크(Health Check): EC2 인스턴스가 정상적으로 가동 중인지 확인하는 기능이다.  
EC2 인스턴스가 중단되었다고 판단되면 해당 EC2 인스턴스는 트래픽 분배에서 제외된다.    
  
- Connection Draining: Auto Scaling이 사용자의 요청을 처리 중인 EC2 인스턴스를   
바로 삭제하지 못하도록 방지하는 기능이다. 예를 들어 사용자 수가 줄어들면 Auto  
Scaling이 EC2 인스턴스를 삭제한다. 마침 사용자가 해당 EC2 인스턴스에서 파일을  
다운로드하고 있었는데 EC2 인스턴스가 삭제되면 파일 다운로드는 중간에 끊어진다.   
EC2 인스턴스를 삭제하기 전에 사용자의 요청을 처리할 수 있도록 지정한 시간만큼   
기다린다. 그리고 기다리는 동안에는 새로운 커넥션을 받지 않는다.  
예) 한 기술 회사에서 운영하는 서비스는 ELB(Elastic Load Balancer)뒤에 EC2   
인스턴스를 두고 부하를 분산하는 방식을 사용하고 있다. 이 때, EC2가 health   
check에 실패하여 unhealthy상태로 들어가게 되면 진행중이던 in-flight Request가  
끊어지는 이슈가 발생하게 된다. 이를 테면, 특정 EC2 인스턴스로부터 수십초가   
걸리는 파일 다운로드를 수행중이라고 했을 때, 파일을 다운로드받는 도중, 해당    
인스턴스가 unhealthy 상태로 바뀌게 되면 커넥션이 끊어지면서 파일 다운로드에   
실패하게 되는 것이다. 이러한 상황을 막기 위해 취할 수 있는 방법에는 어떤  
것이 있을까?   
Connection Draining은 특정 인스턴스가 unhealth 상태로 바뀌었더라도, 해당   
시점에 인스턴스가 처리하고 있는 in-flight request가 있다면, 해당 Request가  
처리될 때까지 커넥션을 유지한다. 해당 Connection을 유지하는 최대 시간(maxi  
mum timeout)을 명시적으로 설정할 수 있으며, maximum timeout을 초과할 경우에는  
인스턴스가 해당 커넥션을 강제 종료한다.   
  
- Sticky Sessions: 사용자의 세션을 확인하여 적절한 EC2 인스턴스로 트래픽을   
분배하는 기능이다(HTTP 쿠키(Cookie)를 이용한 세션). L7 로드 밸런싱의 기능이다.   
예를 들어 동일한 사용자가 서비스에 계속 접속할 때 처음 접속했던 EC2 인스턴스에   
연결시켜 준다. 이 기능을 사용하지 않으면 라운드 로빈 알고리즘에 따라 매번 다른   
EC2 인스턴스에 연결된다.   
  
- Latency: ELB 로드 밸런서와 EC2 인스턴스 간의 지연시간이다.  
- HTTP 2XX, 4XX, 5XX: EC2 인스턴스에서 리턴한 HTTP Response Code이다.   
- ELB HTTP 4XX, 5XX: ELB 로드 밸런서에서 리턴한 HTTP Error Code이다.   
- Surge Queue Length: ELB 로드 밸런서에서 EC2 인스턴스로 전달되지 못하고 큐에  
남아 있는 요청의 개수이다.    
- Spillover Count: 서지 큐가 꽉 차서 ELB 로드 밸런서가 거부한 요청의 개수이다.   
- 요금: ELB 로드 밸런서 실행 시간, 전송된 데이터 양에 따라 요금이 책정된다.  

> #### ELB Pre-warming   
> ELB는 하나의 장비가 아니라 내부적으로 여러 AWS 리소스가 조합된 서비스이다  
> (ELB는 IP 주소가 아닌 도메인으로 접속한다). 따라서 ELB로 들어오는 트래픽이  
> 급격히 늘어날 것으로 예상되면 미리 AWS에 ELB의 처리량을 늘려달라고 요청할 수   
> 있다. 서비스 출시 전 테스트 상황에서도 요청할 수 있다.   
> ELB Pre-warming은 AWS Support에 가입되어야 신청할 수 있다. https://aws.amazon.  
> com/support/createCase/?type=technical_support에 접속한 뒤 기술지원(Technical  
> Support) 문의에서 ELB 정보 및 트래픽 정보를 입력하여 신청하면 된다.   
> - ELB Name: exampleelb-2012921842.ap-northeast-elb.amazonaws.com  
> - Start date for elevated traffic pattern: 2014. 1. 10
> - End date for elevated traffic pattern: 2014. 1. 20  
> - Traffic delta OR request rate expected at surge(in Requests Per Second):    
> 10,000 in 5 minutes   
> - Average amount of data passing through the ELB per request/response pair(  
> In Bytes): 10Kbytes   
> - Rate of traffic increase: 50,000/sec   
> - Are keep-alives used on the back-end?: keep-alives 사용유무 입력   
> - Percent of traffic using SSL termination on the ELB: 트래픽에서 SSL termina  
> tion이 차지하는 비율입력   
> - Number of AZ's that will be used for this event/load balancer: ELB 로드   
> 밸런서가 트래픽을 분산하고 있는 가용 영역 개수 입력   
> - Is the back-end scaled to event/spike levels? If no, how many and what type  
> of instances and when will they be scaled? : Auto Scaling으로 확장하는 기준,  
> EC2 인스턴스 유형 및 개수 입력   
> - Use-case description: 사용 계획 입력  
> - Traffic pattern description: 트래픽 패턴 설명 입력 
> 내용을 모두 한글로 입력해도 되지만 제목이나 내용 첫머리에 "한글로 되어 있다",  
> "한국 고객이다" 등을 영어로 기재하면 좀 더 빨리 처리된다.   
































