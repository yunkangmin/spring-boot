Route 53의 기본 개념에 대해 알아보자. 그리고 Hosted Zone과 A 레코드를 생성하는   
방법, S3, CloudFront와 연동하는 방법에 대해 설명한다. 또한, DNS Failover,   
Latency Based Routing, Weighted Round Robin, Geo Routing을 설정하는 방법과  
Route 53에서 도메인을 구입하는 방법에 대해 알아보자.   
 
Route 53는 EC2, ELB, S3, CloudFront와 연동 가능한 DNS 서비스이다.  

> #### 프리티어에서 사용 불가   
> Route 53은 프리티어에서 무료로 사용할 수 없다.   

Route53은 AWS를 사용해 대규모 글로벌 서비스를 할 때 유용하다. AWS 리소스들과  
연동할 수 있는 것과 더불어 글로벌 서비스에 꼭 필요한 기능들을 제공해준다.   
  
DNS 서비스는 도메인에 연결된 IP 주소를 알려준다. 보통 일반적으로 사용하는   
DNS 서비스 혹은 직접 구축한 DNS 서버는 도메인당 IP 주소 한 개만 설정할 수 있다.   
그래서 DNS 서버에 쿼리를 하면 항상 같은 IP 주소만 알려준다.   
  
Route 53은 일반적인 DNS 서버들과는 달리 Latency Based Routing, Weighted Round  
Robin, DNS Failover, Geo Routing 기능을 제공한다. 이 기능들은 도메인 하나를   
쿼리하더라도 각 상황에 따라 다른 IP 주소를 알려준다.   
  
Latency Based Routing은 현재 위치에서 지연 시간(Latency)이 가장 낮은 리전의   
IP 주소를 알려준다. 지연 시간이 낮다는 것은 현재 위치에서 가장 가깝고 속도가    
빠르다는 것이다. 아래 그림처럼 Route 53은 클라이언트에서 가장 가까운 리전의   
IP 주소를 알려준다.  
<img src="https://user-images.githubusercontent.com/33191974/159161331-262e78e3-5a3a-49f0-befd-50732dd0e791.png" width="50%" height="50%"/>    
Latency Based Routing처럼 전 세계에 DNS 서버를 구축해야 하는 기능은 웬만한  
기업이나 개인이 독자적으로 구축하려면 비용도 많이 들고 상당히 어렵다. 특히   
DNS 서버만 달랑 구축해서는 안되고, 전 세계 곳곳에 실제 서비스를 하는 서버를   
배치해야 하기 때문에 더욱 어렵다. Latency Based Routing 기능은 AWS 글로벌   
인프라가 있기 때문에 가능한 것이다.    
만약 AWS 위에서 운영 중인 서비스가 동경 리전(Tokyo region)과 버지니아 리전(  
Virginia region)을 통해 서비스 되고 있을 경우, 이 서비스 도메인 이름을 Route  
53을 통해 등록하고 지연 속도 기반 라우팅을 설정할 수 있다. 이러한 경우 Route  
53은 해당 서비스의 DNS 질의를 했을 때, Tokyo와 Virginia region 중 고객    
입장에서 보다 낮은 지연 속도(Latency)를 가질 수 있는지 판단하여 지연이 낮은   
리전의 주소를 반환한다.   
  
Weighted Round Robin은 서버 IP 주소나 도메인(ELB)마다 가중치(Weight)를   
부여하고 트래픽을 조절하는 기능이다. 아래 그림처럼 가중치에 따라 한 쪽에는  
80%의 트래픽이 가고, 다른 한 쪽은 20%의 트래픽이 간다(가중치에 따라 클라이언트  
에 IP 주소를 알려주는 비율이 달라진다).  
<img src="https://user-images.githubusercontent.com/33191974/157826233-cc64334e-b525-4ee3-b00f-ad43b3be3f37.png" width="50%" height="50%"/>  
DNS Failover는 장애가 발생한 서버의 IP 주소, 또는 도메인(ELB)은 알려주지 않는  
기능이다. 따라서 장애가 발생하여 동작하지 않는 서버에는 트래픽이 가지 않는다.   
<img src="https://user-images.githubusercontent.com/33191974/157826631-775ba03e-a212-4dfa-94ce-0bb88e39f375.png" width="50%" height="50%"/>    

> #### ELB와 EC2  
> 위 그림에서 ELB(Elastic Load Balancing)에 EC2가 연결된 모습으로 표현했는데,  
> Route53은 ELB 없이 EC2만 연결할 수 있다(EC2에 연결할 때 ELB없이 EC2만  
> 연결할 수 있다는 의미인듯?).     

Geo Routing은 지역별로 다른 IP 주소를 알려준다. 아래 그림처럼 같은 example.com  
도메인이라도 영국에서는 79.125.8.27, 브라질에서는 177.71.128.60, 한국에서는   
54.92.43.31을 알려준다. 특히 미국은 주(State)별로 다른 IP를 알려주도록 설정  
할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157827452-c855cf31-94ed-4a86-9d4c-86c388db5122.png" width="50%" height="50%"/>    
  
Route 53은 CloudFront 또는 S3와 연동할 때 Zone Apex(예: www.example.com 대신  
example.com)를 지원한다. 일반적인 DNS에서는 CNAME(별칭 레코드)으로 연결할 때   
루트 도메인(예: example.com)을 사용할 수 없다.   

> #### Zone Apex  
> Zone Apex는 루트 도메인, 네이키드 도메인(Naked Domain)이라고도 한다. 이름  
> 그대로 서브 도메인이 붙지 않은 상태를 뜻한다. DNS RFC(RFC 1033)에 루트   
> 도메인은 A 레코드만 지정할 수 있다고 정의되어 있다.   












  




  


 





















