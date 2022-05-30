이제 ELB 로드 밸런서를 생성해보자. AWS 콘솔로 접속한 뒤 메인 화면에서 Compute &   
Networking의 EC2를 클릭한다. ELB는 항상 EC2와 함께 사용해야 하므로 EC2와 같은   
페이지에 있다. 리전은 서울로 한다.   
  
먼저 ELB 로드 밸런서에 연결할 EC2 인스턴스를 생성해야 한다. 'EC2 인스턴스 생성  
하기'를 참조하여 EC2 인스턴스 2개를 각각 다른 가용 영역에 생성한다(OS는 Amazon  
Linux를 설치한다, 보안그룹은 인바운드에 80, 22 포트를 열어줘야 한다).  

<img src="https://user-images.githubusercontent.com/33191974/157871634-4762ee8a-8b2c-4659-9a32-821516f91fe9.png" width="50%" height="50%"/>    
EC2 페이지에서 ELB 로드 밸런서 목록(NETWORK & SECURITY -> Load Balancers)을 클릭하고   
위쪽 Create Load Balancer 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157873289-9b6d6c44-968b-4b64-a2ef-c3acfa3e81f0.png" width="50%" height="50%"/>  
  
ELB 로드 밸런서를 생성한다.   
- Load Balancer name: 로드 밸런서 이름이다. exampleelb를 입력한다.   
- Create LB Inside : 로드 밸런서가 생성될 VPC이다. 기본값 그대로 사용한다.    
- Create an internal load balancer: 인터넷에 연결되지 않은 내부 로드 밸런서로   
생성하는 옵션이다. 기본값 그대로 체크를 해제한다.   
- Enable advanced VPC configuration: VPC에 속한 서브넷을 선택하는 옵션이다.   
이 부분을 체크하면 뒤에서 서브넷을 선택할 수 있다. 기본값 그대로 체크를 해제한다.   
- Listener Configuration: 로드 밸런서가 처리할 프로토콜과 포트 번호이다. 기본값  
그대로 사용한다.   
   - HTTP, HTTPS(Secure HTTP): 일반적으로 사용하는 HTTP 프로토콜이다.   
   - TCP, SSL(Secure TCP): 소켓 통신 등 TCP 프로토콜을 사용할 때 선택한다.  
<img src="https://user-images.githubusercontent.com/33191974/157874155-49989c9c-ddad-4cd7-903f-44f9da1c353c.png" width="50%" height="50%"/>   
  
ELB 로드 밸런서의 Security Group을 생성한다.   
- Assign a security group: 로드밸런서의 Security Group이다. Create a new security  
group을 선택한다.   
- Security group name: 새로 생성될 Security Group 이름이다. 기본값 그대로 사용한다.   
- Description: 새로 생성될 Security Group의 설명이다. 기본값 그대로 사용한다.  
- 앞에서 Load Balancer Protocol을 HTTP에 80번 포트로 설정했으므로 동일하게 Type을   
HTTP로 설정한다.   
<img src="https://user-images.githubusercontent.com/33191974/157876361-380011a7-7084-4c68-b989-d9afffd52590.png" width="50%" height="50%"/>   
  
헬스 체크(Health Check) 기능을 설정한다.  
- Ping Protocol: 헬스 체크를 할 때 사용할 프로토콜이다. HTTP, HTTPS, TCP, SSL을  
선택할 수 있다. 기본값 그대로 사용한다.   
- Ping Port: 헬스 체크를 할 때 사용할 포트 번호이다. 기본값 그대로 사용한다.   
- Ping Path: 헬스 체크를 할 때 접속할 경로이다. HTTP, HTTPS에서만 설정할 수  
있다. 기본값 그대로 사용한다. 
- Response Timeout: 헬스 체크 응답 시간이다. 이 시간이 지나도 응답이 없으면  
EC2 인스턴스 가동 확인에 실패한 것으로 판단한다. 기본값 그대로 사용한다.   
- Health Check Interval: 헬스 체크 주기이다. 기본값 그대로 사용한다.   
- Unhealthy Threshold: 연속으로 설정한 값만큼 가동 확인에 실패했을 때 가동이   
중단된 것으로 판단한다. 기본값 그대로 사용한다.    
- Healthy Treshold: 가동이 중단되어 트래픽 분산에서 제외되었을 때 연속으로 설정된   
값만큼 가동 확인에 성공하면 다시 포함된다. 기본값 그대로 사용한다.    
<img src="https://user-images.githubusercontent.com/33191974/157878407-a2dab8c3-fdf5-4651-b5ef-99ead34418ec.png" width="50%" height="50%"/>    
  
앞에서 생성한 EC2 인스턴스 2개를 선택하여 ELB 로드 밸런서에 연결한다.   
- Enable Cross-Zone Load Balancing: 여러 가용 영역에 생성된 EC2 인스턴스에    
부하를 분산하는 옵션이다. 기본값 그대로 사용한다.  
- Enable Connection Draining: Connection Draining 사용 옵션이다. 1초부터 3600초  
(1시간)까지 설정할 수 있다. 기본값 그대로 사용한다.  
<img src="https://user-images.githubusercontent.com/33191974/157880683-935c7367-3fdc-4e7a-bdec-a99ee3171c39.png" width="50%" height="50%"/>     
지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Create 버튼을   
클릭한다. 
<img src="https://user-images.githubusercontent.com/33191974/157880843-919a0fb6-6c9c-4395-b8cf-387a66d10f8d.png" width="50%" height="50%"/>  
ELB 로드밸런서(exampleelb)가 생성되었다. 아래 ELB 세부 내용에 이 ELB 로드 밸런서의   
DNS Name이 표시된다. 앞으로 서비스에 접속할 때는 EC2 인스턴스에 바로 접속하지   
않고 ELB 로드 밸런서의 URL로 접속한다.   
<img src="https://user-images.githubusercontent.com/33191974/157881289-53b57bc7-e422-4cd1-9088-9c8d4ecafb4e.png" width="50%" height="50%"/>   

> #### Route 53와 ELB   
> AWS에서 제공하는 ELB 로드 밸런서의 DNS Name 대신 자신이 구입한 도메인을 사용   
> 하려면 Route 53에서 A 레코드를 생성할 때 Alias를 Yes로 선택하고, Alias Target에서   
> ELB 로드 밸런서를 선택하면 된다.   
> Route 53 A 레코드 생성 방법은 'Route 53 A 레코드 생성하기'를 참조하기 바란다.   







  

  
 























