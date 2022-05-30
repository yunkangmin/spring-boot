ElastiCache에서 Memcached 클러스터를 생성하고 사용해보자. AWS 콘솔로 접속한 뒤   
메인 화면에서 Database의 ElastiCache를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156989226-68bc7067-0bbf-402a-9397-c55c1bfb8a4b.png" width="50%" height="50%"/>      
서울 리전을 선택한다.  
ElastiCache에서 클러스터를 생성하지 않았다면 아래와 같이 페이지가 표시된다.  
Get Started Now를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156989435-48c8607b-e01b-4cb6-9f9e-58f73b782311.png" width="50%" height="50%"/>   
Memcached 클러스터를 생성한다.  
- Name: 클러스터 이름이다. examplememcached를 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156989712-ddd0604c-62c8-40cd-aeb0-15820a8b590b.png" width="50%" height="50%"/>  
  
- Cache Port: Memcached 포트이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156989859-d076d5f2-6bd0-47f8-a616-53e3d2e54f5c.png" width="50%" height="50%"/>  
  
- Number of Nodes: 생성할 캐시 노드이다. 이 부분에 입력한 수 대로 캐시 노드가   
생성된다. 1을 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156990063-21f9bcad-60b2-4b39-a0e5-35591dfb9d78.png" width="50%" height="50%"/>  
  
- Node Type: 캐시 노드 유형이다. 프리티어에서 무료로 사용할 수 있는 마이크로 캐시  
노드(cache.t2.micro)를 선택한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156990608-a706b2c8-6f9e-4b1d-9de8-34e3816c3a68.png" width="50%" height="50%"/>  
  
- Topic for SNS Notification: 알람을 받을 SNS 토픽이다. 여기서는 알람은 받지   
않을 것이므로 Disable Notifications를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156990998-7bdff91d-c797-4c1d-a935-c1dc8f55723b.png" width="50%" height="50%"/>  
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.   
보안 패치나 버그가 수정된 버전을 자동으로 업데이트한다. 예를 들면 Memcached의  
경우 1.4.5를 사용하고 있는데 1.4.6이 나오면 1.4.6 버전으로 업데이트하게 된다.  
기본값 그대로 사용한다(현재는 없는듯).     
- Engine: 사용할 캐시 엔진이다. memcached를 선택한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156991729-34fdd2f5-1c56-48ba-af62-8da9b67cb113.png" width="50%" height="50%"/>  
  
- Engine Version: Memcached 버전이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156991906-f716aada-2d48-46a9-b59d-9d4d60ba08c4.png" width="50%" height="50%"/>   
     
- Preferred Zone: 클러스터가 위치할 가용 영역(AZ)이다. 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156992139-7babec46-e290-4f38-8ada-0f475d8f9d5b.png" width="50%" height="50%"/>  
  
- Cache Subnet Group: 캐시 노드가 위치할 서브넷이다. 생성한 Subnet Group이  
있어야 선택할 수 있다.  
  <img src="https://user-images.githubusercontent.com/33191974/156992571-b9d3d4fd-c0cb-4bfc-8ecb-1315b462132e.png" width="50%" height="50%"/>       
  
- Maintenace Window: 점검 시간이다. 기본값은 No Preference이다. 여기서는 Start  
Time을 Moday, `00:00`, Duration을 1로 설정한다. UTC 기준으로 00시 00분에 점검이   
시작되며 시간은 1시간이다. 점검은 Duration에 설정한 시간보다 일찍 끝날 수 있다.  
이 시간에 Auto Minor Version Upgrade를 설정했다면 Memcached 버전 업데이트 또는  
패치가 적용된다. Memcached 버전 업데이트 또는 패치는 필수적인 것(보안 패치)만  
적용되며 자주 발생하지 않고 몇 달에 한 번 발생한다. Memcached 업데이트 또는  
패치가 적용되는 시간 동안에는 캐시 노드의 실행이 중지된다.  
  <img src="https://user-images.githubusercontent.com/33191974/156994313-4598f65c-22d0-4e65-9b73-b14b015952e9.png" width="50%" height="50%"/>  
      
설정이 완료되었으면 Launch Cache Cluster 버튼을 클릭한다.    
Memcached 클러스터 추가 설정이다(현재는 추가 설정이 없고 디폴트로 설정됨  
).  
- VPC Security Group: 방화벽 설정인 Security Group이다. 기본값 그대로 사용한다.     
이 Security Group은 나중에 Memcached 클러스터 전용으로 따로 생성해야 한다.  
- Cache Parameter Group: Memcached를 실행할 때 필요한 매개변수 집합이다. 캐시  
노드 생성 후 Cache Parameter Group을 추가할 수 있다(/etc/sysconfig/memcached/  
파일을 생성하는 것과 동일하다). 기본값 그대로 사용한다.    

ElasiCache 캐시 클러스터 목록(Amazon ElastiCache -> Cache Clusters)에서 방금  
설정한 Memcached 클러스터가 생성되고 완전히 생성되기까지 약 5분 정도 소요된다.   
Memcached 클러스터 생성이 완료되었으면 1 nodes 링크를 클릭한다.   
  
<img src="https://user-images.githubusercontent.com/33191974/156995498-05a7f392-263e-4202-9f3b-276707edff22.png" width="50%" height="50%"/>    
캐시 클러스터에 속한 캐시 노드의 목록이 표시된다. Endpoint 부분에 examplemem  
cached.jyqn3e.0001.apn2.cache.amazonaws.com처럼 캐시 노드에 접속할 수 있는   
엔드포인트 주소가 표시된다. 이후 이 엔드포인트 주소를 통해 캐시 노드에 접속하면  
된다.  



 


  

  



  

  
  





























