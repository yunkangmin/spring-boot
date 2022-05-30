# 정리
1. 보안그룹은 방화벽이고 VPC 성  
2. EC2 생성시 같은 자동으로 default VPC가 설정되므로 ElastiCache와    
같은 VPC이다.   
3. VPC 보다 작은 더 작은 개념이 서브넷이다. VPC안에 있다.  
  
--- 
  
ElastiCache Memcached 클러스터와 캐시 노드가 완전히 생성되었더라도 엔드포인트   
주소로 접속이 되지 않는다. Memcached 클러스터를 생성할 때 Security Group을   
기본값인 default(VPC)로 설정했다. 이 default(VPC)는 모든 트래픽에 대해 Inbound가  
열려 있지만 접속 가능한 IP 대역(Source)은 default 자기 자신으로 설정되어 있다.  
즉 같은 default(VPC) Security Group 설정 안에서만 접속이 허용되므로 외부에서는  
접속할 수 없다. 따라서 Memcached 클러스터 전용 Security Group을 생성하고 포트  
(11211)를 열어줘야 한다.   
  
RDS와 ElastiCache는 큰 차이점이 있다. RDS의 데이터베이스 엔진은 AWS 외부(인터넷)  
에 접속이 허용되어 있지만 ElastiCache의 캐시 엔진은 AWS 외부에서 접속할 수 없다.  
Security Group을 생성하여 모든 IP 대역에 대해 접속을 허용하더라도 동일한 VPC에   
속한 EC2 인스턴스에서만 접속할 수 있다.   
  
ElastiCache Memcached 클러스터용 Security Group을 생성해보자. AWS 콘솔의 메인   
화면에서 Compute & Networking의 EC2를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157038179-d36d8380-461a-4db0-9394-1ae3cc37b05d.png" width="50%" height="50%"/>    
NETWORK & SECURITY -> Security Groups를 클릭한 뒤 위쪽의 Create Security Group  
버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157038467-e5a296a6-04f0-4010-a22d-0b7562ed14d1.png" width="50%" height="50%"/>   
  
ElastiCache Memcached 클러스터용 Security Group을 생성한다.   
- Security group name: Security Group의 이름이다. Memcached Cluster를 입력한다.  
- Description: Security Group 설명이다. Memcached Cluster를 입력한다.   
- VPC: Security Group이 적용될 VPC이다. 기본값 그대로 사용한다.  

외부에서 들어오는 트래픽인 Inbound 탭을 선택한다(Inbound가 기본으로 선택되어   
있을 것이다). 아래쪽 Add Rule 버튼을 클릭한다.   
- Type: 트래픽 종류이다. Memcached는 미리 정의된 것이 없으므로 Custom TCP Rule을  
선택한다.     
- Protocol: 프로토콜이다. Custom TCP Rule을 선택하면 자동으로 TCP가 선택된다.  
- Port Range: 포트 번호이다. 우리는 Memcached 포트를 열어야 하므로 11211을  
입력한다.   
- Source: 접속 가능한 IP 또는 IP 대역이다. Anywhere를 선택한다(실무에서는 My  
IP를 선택하여 자신의 IP만 접속할 수 있도록 설정하거나, Custom IP를 선택하여   
특정 IP 대역을 설정하도록 한다).    
<img src="https://user-images.githubusercontent.com/33191974/157039787-7bab01bd-9772-4ca0-be75-21d9d42db5bd.png" width="50%" height="50%"/>   
  
설정이 완료되었으면 Create 버튼을 클릭한다.   
  
Security Group 목록(NETWORK & SECURITY -> Security Groups)에 Memcached Cluster  
Security Group이 생성되었다.   
<img src="https://user-images.githubusercontent.com/33191974/157040120-459c7980-4d63-4789-b665-155e6faeff5e.png" width="50%" height="50%"/>    
이제 다시 ElastiCache 캐시 클러스터 목록(Amazon ElastiCache -> Cache Clusters)  
으로 이동한다. ElastiCache 캐시 클러스터 목록에서 Memcached 클러스터(example  
memcached)에 있는 Modify 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157041472-e468ac95-bfa5-4808-a758-8f948774821f.png" width="50%" height="50%"/>    
ElastiCache Memcached 클러스터의 설정을 변경한다. VPC Security Group(s)에서  
방금 생성한 Memcached Cluster를 선택하고 아래쪽 Yes, Modify를 클릭한다.     
<img src="https://user-images.githubusercontent.com/33191974/157041705-5100aa6f-076b-4bc8-97c4-62f0509491b1.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/157041931-a47bc985-4eef-4039-9a1a-0ecc0dac5b26.png" width="50%" height="50%"/>    
Memcached 클러스터(examplememcached)의 설정이 변경 중이다. 약 10초 정도 기다리면   
설정이 완전히 적용된다. 1 nodes 링크를 클릭하여 캐시 노드 목록으로 이동한다.   
Memcached 클러스터의 캐시 노드 목록에서 포트 번호와 엔드포인트 주소를 확인할 수  
있다.   
<img src="https://user-images.githubusercontent.com/33191974/157042249-b7e23033-e378-4730-af14-a972cb9186bf.png" width="50%" height="50%"/>  
Memcached는 텔넷(telnet)으로 접속할 수 있다. 앞에서 생성한 EC2 인스턴스(Example  
Server)에서 텔넷을 이용하여 Memcached 캐시 노드로 접속할 것이다(아직 EC2 인스턴스  
를 생성하지 않았다면 "EC2 인스턴스 생성하기"를 참조하여 EC2 인스턴스를 생성하기  
바란다).    
  
telnet <엔드포인트 주소> 11211 순으로 명령을 입력하면 Memcached 캐시 노드로  
접속할 수 있다. 접속한 후 stats를 입력하면 현재 캐시 노드의 정보가 표시된다.   
```
telnet examplememcached.jyqn3e.0001.apn2.cache.amazonaws.com 11211
Trying 172.31.14.19...
Connected to examplememcached.jyqn3e.0001.apn2.cache.amazonaws.com.
Escape character is '^]'.
STAT pid 1
ERROR
stats
STAT pid 1
STAT uptime 2887
STAT time 1646661580
STAT version 1.6.6
STAT libevent 2.0.21-stable
STAT pointer_size 64
STAT rusage_user 0.101921
STAT rusage_system 0.045790
STAT max_connections 65000
STAT curr_connections 5
...
```
접속이 되지 않는다면 Security Group에 포트 번호를 정상적으로 입력했는지, MemCa  
ched 클러스터 설정에서 방금 생성한 Security Group을 선택했는지, 텔넷 접속에서  
Endpoint와 포트 번호를 정확하게 입력했는지 EC2 인스턴스가 같은 VPC에 속해   
있는지 확인한다. 앞에서 설명한 것처럼 ElastiCache의 캐시노드는 AWS 외부에서  
접속할 수 없다.   
  
> #### 텔넷 클라이언트 설치  
> - Windows 7 이상: 제어판 -> 프로그램 및 기능 -> Windows 기능 사용/사용 안함  
> -> 텔넷 클라이언트 선택 후 설치  
> - Amazon Linux, CentOS: sudo yum install telnet   
> - Ubuntu: sudo apt-get install telnet
















 































