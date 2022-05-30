생성된 Memcached 클러스터에 캐시 노드를 추가해보자. Memcached 클러스터에   
캐시 노드를 추가하면 데이터를 저장할 수 있는 공간이 늘어난다. ElastiCache 캐시   
클러스터 목록(Amazon ElasiCache -> Cache Clusters)에서 Memcached 클러스터(
examplememcached)의 1 nodes 링크를 클릭하여 캐시 노드 목록으로 이동한다.   
  
ElastiCache 클러스터 목록에서 캐시 노드 목록으로 이동   
<img src="https://user-images.githubusercontent.com/33191974/157165242-9e06a4f3-0409-4417-95f1-3d993175d13b.png" width="50%" height="50%"/>  

Memcached 클러스터(examplememcached)의 캐시 노드 목록이 표시된다. Add Node  
버튼을 클릭한다.     
ElastiCache 캐시 노드 목록  
<img src="https://user-images.githubusercontent.com/33191974/157165331-6f9bae46-4ada-4f8a-a749-ba3297e458bb.png" width="50%" height="50%"/>  
  
Memcached 클러스터에 캐시 노드를 추가한다.   
- Number of nodes to add: 추가할 캐시 노드의 개수이다. 1개를 추가할 것이므로   
1을 입력한다.   
- Apply immediately: 캐시 노드를 즉시 추가하는 옵션이다. 이 것을 체크하지 않으 
면 Maintenance Window에 설정한 시간에 캐시 노드가 추가된다. 즉시 생성할 것이므로   
체크한다.   
  
설정이 완료되었으면 Yes, Add 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157165507-efb24abe-e3c8-4d25-b617-b668191dbb68.png" width="50%" height="50%"/>   
  
ElastiCache 캐시 노드 목록에 추가한 캐시 노드가 생성되고 있다(추가한 캐시 노드가  
보이지 않으면 위쪽 리프레시 버튼을 클릭한다). 완전히 생성되기까지 약 5분 정도   
소요된다.   
<img src="https://user-images.githubusercontent.com/33191974/157165648-8d2ebb5a-f5c4-46f5-8feb-5d0207f67b8d.png" width="50%" height="50%"/>    
  
Memcached 클러스터에 캐시 노드가 완전히 추가되었다. ElastiCache 캐시 노드 목록에서   
추가한 캐시 노드의 포트와 엔드포인트 주소를 확인할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157166039-ace11a60-0868-4af3-873d-dcacb4de2d89.png" width="50%" height="50%"/>  
  
새로 추가한 캐시 노드에 텔넷으로 접속하여 stats 명령을 입력한다. examplememcached.  
jyqn3e.0002.apn2.cache.amazonaws.com는 필자가 생성한 캐시 노드 엔드포인트 주소  
이다. 여러분들이 생성한 캐시 노드의 엔드포인트 주소를 입력하기 바란다.   
```
telnet examplememcached.jyqn3e.0002.apn2.cache.amazonaws.com 11211
Trying 172.31.4.70...
Connected to examplememcached.jyqn3e.0002.apn2.cache.amazonaws.com.
Escape character is '^]'.
stats
STAT pid 1
STAT uptime 391
STAT time 1646713789
STAT version 1.6.6
STAT libevent 2.0.21-stable
STAT pointer_size 64
STAT rusage_user 0.018923
STAT rusage_system 0.000000
STAT max_connections 65000
...
```
접속이 되지 않는다면 Security Group에 포트 번호를 정상적으로 입력했는지, 'ElastiC  
ache Memcached 클러스터 Security Group 생성 및 설정하기'에서 Security Group을 
생성하고 사용하도록 설정했는지, 텔넷 접속에서 엔드포인트 주소와 포트 번호를  
정확하게 입력했는지, EC2 인스턴스가 같은 VPC에 속해 있는지 확인한다. 앞에서   
설명한 것처럼 ElastiCache의 캐시 노드는 AWS 외부에서 접속할 수 없다.   









































