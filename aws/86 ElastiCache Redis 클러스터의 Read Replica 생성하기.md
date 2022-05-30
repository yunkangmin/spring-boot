# 정리
Replication Group(examplereplication)의 마스터 redis 클러스터는    
exampleredis이고 read replica는 exampleredis-read-1이다. 각각의   
클러스터에는 캐시 노드가 있다.   

---

Redis 클러스터의 Read Replica를 생성하여 읽기 성능을 높이고, 장애에 자동으로  
대처하는 Failover 기능을 사용할 수 있다. 서비스에서 읽기 위주의 작업이 많을 경우  
Read Replica를 여러 개(최대 5개) 만들어 부하를 분산시킬 수 있다. 즉, 쓰기 작업은  
마스터 캐시 노드에 하고 읽기 작업은 Read Replica인 슬레이브 캐시 노드에서 실시한  
다면 마스터 캐시 노드의 부하를 줄일 수 있다.   
  
마스터 캐시 노드에 쓰기를 하면 자동으로 슬레이브 캐시 노드로 데이터가 복제된다.   
단, 쓰기 작업을 실시한 즉시 복제되는 것은 아니며 약간의 시간차가 있다.  
  
> #### `<Note>` Redis 클러스터와 캐시 노드   
> 2014년 8월 기준으로 Redis 클러스터는 캐시 노드를 1개만 가지고 있고, 더 추가할   
> 수 없다. 따라서 마스터 클러스터와 마스터 캐시 노드는 사실상 같은 의미이다.   

ElastiCache 클러스터(exampleredis)의 Read Replica를 생성해보자. 

--- 

현재는 생략해도 됨  
ElastiCache   
Replica Group 목록(Amazon ElastiCache -> Replication Group)으로 이동하여  
Redis Cluster를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/157270866-7df77911-619e-4b4c-8083-11dfa6965f7e.png" width="50%" height="50%"/>   
이미 redis 클러스터를 생성하면 read replica가 생성되어 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157271024-2d0dc75f-d508-4fd7-9b7d-e0a373cff162.png" width="50%" height="50%"/>  
추가적으로 더 생성하기 위해서는 노드 추가 버튼을 클릭한다.   
- Primary Cluster ID : 마스터 Redis 클러스터이다. 앞에서 생성한 exampleredis를  
선택한다.  
- Replication Group ID: Replication Group의 이름이다. examplereplication을  
입력한다.   
- Replication Group Description: Replication Group의 설명이다. examplerepli  
cation을 입력한다.  
<img src="https://user-images.githubusercontent.com/33191974/157271513-5f7c99b7-ab7f-41d9-affb-3fb041c069dc.png" width="50%" height="50%"/>  

---

Replication Group에 Read Replica를 추가한다.   
<img src="https://user-images.githubusercontent.com/33191974/157442040-d0ba55dc-957b-423b-a28a-0d590d02b635.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/157442124-5d0f3af2-6ab4-4d6d-9352-1240b7e567b7.png" width="50%" height="50%"/>    
  
- Replication Group: Read Replica를 추가할 Replication Group을 설정한다.   
exmplereplication이 선택되었는지 확인한다.   
- Read Replica ID: Read Replica의 이름이다. examplereids-read-1을 입력한다.   
- Availability Zone: Availability Zone: Read Replica가 생성될 가용 영역이다.  
추후 각자 상황에 맞게 리전에 속한 AZ를 선택하면 된다. ap-northeast-1a를   
선택한다.  
  
설정이 완료되었으면 Add 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157441769-79293fed-00f6-40e5-afec-63b965b7b977.png" width="50%" height="50%"/>   
  
Replication Group 목록에서 examplereplication을 선택하면 아래 세부 내용에   
Primary 엔드포인트 주소가 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/157442642-e2304c99-2b95-4889-938f-6da39dad1eaa.png" width="50%" height="50%"/>     
  
위에서 보면 exampleredis가 마스터(Primary)이고, exampleredis-read-1이 슬레이브  
(read replica)이다. 이 후 마스터에 장애가 발생하면 Failover 기능이 작동하여   
슬레이브인 exampleredis-read-1이 마스터로 승격된다.   
  
평소에는 Primary 엔드포인트 주소가 마스터인 exampleredis를 가리키고 있다.   
그래서 쓰기 작업은 Primary 엔드포인트 주소에 접속하여 실시하면 된다. Failover  
기능이 동작하면 자동으로 Primary 엔드포인트 주소는 슬레이브인 exampleredis-  
read-1을 가리키게 되고 쓰기 작업을 수행한다(여기서는 exampleredis-001인듯).  
따라서 Failover를 위한 작업을 따로 하지 않아도 된다.  
  
> #### `<Note>` Demote, Promote   
> Demote 기능은 마스터 Redis 클러스터를 다른 마스터 Redis 클러스터(현재는 아닌듯)의 Read   
> Replica로 만드는 기능이다. Apply Immediately를 Yes로 설정하면 곧 바로 Read   
> Replica로 만들며, No로 설정하면 Maintenance Window에 설정된 시간에 실행된다.  
> <img src="https://user-images.githubusercontent.com/33191974/157445544-6fe86195-8257-43bc-ac98-3762bcac9ed4.png" width="50%" height="50%"/>    
> Prmote 기능은 Read Replica Redis 클러스터를 새로운 마스터 Redis 클러스터로  
> 승격(promote)시키는 기능이다. 이 기능을 사용하면 이전 마스터 Redis 클러스터의  
> 복제 관계는 끊어지고, 별개의 Redis 클러스터가 된다. Apply Immediately를 Yes로  
> 설정하면 곧 바로 마스터 Redis 클러스터로 만들며 No로 설정하면 Maintenance   
> Window에 설정된 시간에 실행된다.   
> <img src="https://user-images.githubusercontent.com/33191974/157445628-78e83937-0a3d-421b-bf6b-e28311f9e8a4.png" width="50%" height="50%"/>    
> Promote 기능은 서비스되고 있는 캐시 클러스터와 동일한 데이터를 가진 개발 및   
> 테스트용 캐시 클러스터를 생성하고 싶을 때, 지역이나 언어별로 서비스를 분리할  
> 때 활용할 수 있다.   

ElastiCache 캐시 클러스터 목록(Amazon ElastiCache -> Cache Clusters)으로   
이동한다. 캐시 클러스터 목록에서 방금 추가한 Redis 클러스터 Read Replica(  
exampleredis-read-1)가 생성되고 있다. 완전히 생성되기까지 10분 정도 소요된다.   
<img src="https://user-images.githubusercontent.com/33191974/157451518-2a0723c4-8e83-4a8d-8e31-b3b1ee8e8687.png" width="50%" height="50%"/>  
  
Redis 클러스터 Read Replica(exampleredis-read-1)가 완전히 생성된 뒤 ▶를   
클릭하여 세부 정보를 확인한다. Replication Group이 examplereplication으로   
설정되어 있다. 그리고 캐시 클러스터 목록에서 exampleredis와 exampleredis-read-  
1이 같은 Replication Group(examplereplication)으로 설정되어 있다**(현재는    
Replication Group을 따로 생성하지 않고 redis 클러스터를 생성하면 read repli  
ca를 생성할 수 있다. 따라서 자동으로 redis 클러스터 안에 속한다)**.   
  
Read Replica(exampleredis-read-1)의 Endpoint에 접속하면 마스터 Redis 클러스  
터와 동일한 데이터를 사용할 수 있다. 단, 읽기 전용이므로 Redis의 쓰기 관련  
명령어는 사용할 수 없다.   
<img src="https://user-images.githubusercontent.com/33191974/157458170-00a34875-efda-4bd0-a453-fe9b109d4091.png" width="50%" height="50%"/>   















  

































