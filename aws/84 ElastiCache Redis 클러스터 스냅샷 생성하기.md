ElastiCache의 Redis는 스냅샷을 생성할 수 있다. Redis 스냅샷은 현재 Redis에  
저장된 모든 내용을 파일로 저장한 형태이다. Redis는 인 메모리 캐시이므로 스냅샷을  
생성하면 Redis 메모리에 저장된 모든 데이터를 스냅샷으로 생성한다.  
    
이제 ElastiCache Redis 클러스터의(exampleredis)의 스냅샷을 생성해보자.   
  
> #### <Warning> cache.t1.micro  
> cache.t1.micro 캐시 노드의 유형은 Redis 스냅샷을 생성할 수 없다. 스냅샷을  
> 생성하려면 캐시 노드 유형을 cache.m1.small 이상 사용해야 한다.   
  
ElastiCache 캐시 클러스터 목록(Amazon ElastiCache -> Cache Clusters)에서   
Redis 클러스터(exampleredis)를 선택하고 위쪽 Backup 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/157261852-bcc5f0eb-57bf-4aff-b746-fef8dbc1f365.png" width="50%" height="50%"/>  
  
> #### <Note> Snapshots  
> Amazon ElastiCache -> Snapshots 메뉴에서도 스냅샷을 생성할 수 있다.   
  
Snapshot Name에는 생성할 스냅샷의 이름을 설정한다. 여기서는 examplesnapshot을   
입력하고, Yes, Creaet Snapshot 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157262490-b490570d-71d2-4c2f-ad70-41fc5cad1d7b.png" width="50%" height="50%"/>  
  
ElastiCache 스냅샷 목록(Amazon ElastiCache -> Snapshots)으로 이동한다.  
스냅샷 목록에 Redis 스냅샷(examplesnapshot)이 생성 중이다. 완전히 생성되기까지   
약 2 ~ 3분 정도 소요된다.  
    
Status가 available로 표시되면 생성이 완료된 것이며 이 스냅샷으로 Redis 캐시  
노드를 생성할 수 있다(2014년 8월 기준으로 ElastiCache Redis 스냅샷은 다른   
리전으로 복사할 수 없고, 같은 리전에서만 복사할 수 있다).
<img src="https://user-images.githubusercontent.com/33191974/157263209-8087b09b-46dd-4a53-b794-747d7c5bcb81.png" width="50%" height="50%"/>   
  
  

  
  
  
  

  




















