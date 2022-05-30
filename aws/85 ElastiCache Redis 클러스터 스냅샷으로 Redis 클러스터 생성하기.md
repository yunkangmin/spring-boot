EBS 스냅샷으로 EBS 볼륨을 생성하는 것과 마찬가지로 ElastiCache Redis 스냅샷으로  
Redis 클러스터를 생성할 수 있다. ElastiCache 스냅샷 목록(Amazon ElastiCache ->   
Snapshots)에서 Redis 스냅샷(examplesnapshot)을 선택하고 위쪽 Restore Snapshot  
버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157263967-02c37f7a-6ee6-4d83-b03e-4d5f0eab439e.png" width="50%" height="50%"/>  
  
Redis 스냅샷으로 Redis 클러스터를 생성하기 전에 설정이 필요하다.   
- Cache Cluster Id: Redis 스냅샷을 이용하여 새로 생성될 Redis 클러스터의 이름이다.   
exampleredis2를 입력한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157264425-8850c87a-66d5-4219-9271-3298837e326c.png" width="50%" height="50%"/>  
  
- Cache Engine: Redis 스냅샷을 생성했으므로 변경할 수 없다.   
  <img src="https://user-images.githubusercontent.com/33191974/157264600-972704f6-11c5-4e68-9330-c5e42a24a59b.png" width="50%" height="50%"/>  
  
- Cache Engine Version: Redis 스냅샷을 생성했을 때 사용한 Redis 버전으로 고정된다.  
현재는 변경가능함. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157264997-0982280b-938a-457a-8abc-f11a1e9b9983.png" width="50%" height="50%"/>    
  
- Instance Type: 생성할 Redis 클러스터의 캐시 노드 유형이다. Redis 스냅샷으로   
Redis 클러스터를 생성할 때 성능이 더 좋은 캐시 노드 유형으로 바꿀 수 있다.   
여기서는 cache.m1.small을 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157265609-a839847e-1646-462c-b98b-efc6852caabf.png" width="50%" height="50%"/>  
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.   
보안 패치나 버그가 수정된 버전을 자동으로 업데이트한다. 예를 들면 Redis의 경우  
2.8.6을 사용하고 있는데 2.8.7이 나오면 2.8.7 버전으로 업데이트하게 된다. 기본  
값 그대로 사용한다(현재는 안보임).
- Cache Port: Redis 접속 포트 번호이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/157266270-670ea0be-b7d8-4370-baa5-9b7e69690853.png" width="50%" height="50%"/>  
  
- Choose a VPC: Redis 클러스터가 위치할 네트워크(VPC)이다. default를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157266389-893f4b69-6172-4ab5-a5df-2fbb12f6d33b.png" width="50%" height="50%"/>  
  
- Availability Zone: Redis 클러스터가 생성될 가용영역이다. EC2 인스턴스에서   
Redis에 접속한다면 같은 AZ에 있는 것이 좋다. 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/157266774-2258acd3-9401-4521-98a3-9b15c6e3776f.png" width="50%" height="50%"/>  
  
- Parameter Group: Redis를 실행할 때 필요한 매개변수 집합이다. 기본값 그대로   
사용한다.   
  
설정이 완료되었으면 Launch Cache Cluster 버튼을 클릭한다.   
  
ElastiCache 캐시 클러스터 목록(Amazon ElastiCache -> Cache Clusters)으로   
이동한다. 캐시 클러스터 목록에 Redis 스냅샷으로 Redis 클러스터(exampleredis2)가  
생성되고 있다. 완전히 생성되기까지 약 10분정도 소요된다.  
  
1 node 링크를 클릭하면 Redis 클러스터의 캐시 노드를 볼 수 있고 캐시 노드의   
엔드포인트 주소를 확인할 수있다.  
<img src="https://user-images.githubusercontent.com/33191974/157267568-b5a54f90-5560-4d11-9596-c648d19a0d5d.png" width="50%" height="50%"/>    
  
이처럼 ElastiCache Redis 스냅샷에 저장된 내용을 Redis 클러스터로 복구할 수 있다.  





  

  

 

























