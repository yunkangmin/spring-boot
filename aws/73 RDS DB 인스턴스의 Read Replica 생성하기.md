Read Replica는 DB 인스턴스의 읽기 복제본을 만들어 성능을 향상하는 데 쓰인다(  
현재 Read Replica는 MySQL만 지원한다). 서비스에서 읽기 위주의 작업이 많을 경우  
Read Replica를 여러 개(최대 5개) 만들어 부하를 분산할 수 있다. 즉, 쓰기 작업은   
마스터 DB 인스턴스에 하고 읽기 작업은 Read Replica인 슬레이브 DB 인스턴스에서  
실시한다면 마스터 DB 인스턴스의 부하를 줄일 수 있다.  
  
마스터 DB 인스턴스에 쓰기를 하면 자동으로 슬레이브 DB 인스턴스로 데이터가   
복제된다. 단, 쓰기 작업을 실시한 즉시 복제되는 것은 아니며 약간의 시간차가   
있다.  
  
> #### Multi-AZ 복제와 Read Replica   
> - Multi-AZ 복제: 쓰기 작업을 실시한 즉시 예비 인스턴스에 복제된다. 따라서   
> 데이터가 항상 일치한다는 것을 보장할 수 있다. Multi-AZ 복제는 서비스가 항상  
> 가동해야 하는 가용성(Availability)을 위한 것이지 부하 분산을 통한 성능 향상을  
> 위한 것은 아니다. 또한, 예비 인스턴스에서는 읽기 작업을 할 수 없다.   
> - Read Replica: 쓰기 작업을 실시하면 약간의 시간차를 가지고 복제된다. 따라서   
> 데이터가 일치하지 않을 수 있다. Read Replica는 읽기 부하 분산을 통해 성능  
> 향상을 위한 기능이다. 

RDS DB 인스턴스(exampledbinstance)의 Read Replica를 생성해보자. RDS DB 인스턴스  
목록(Instances)에서 DB 인스턴스(exampledbinstance)를 선택하고 Create Read   
Replica를 클릭한다.   
  
<img src="https://user-images.githubusercontent.com/33191974/156975217-7bbe1978-554a-4719-b32c-857968faeb38.png" width="50%" height="50%"/>      
Read Replica를 생성한다.   
- Destination Region: Read Replica 인스턴스가 위치할 리전이다. Read Replica는   
여러 리전에 생성할 수 있어, 지역별로 읽기 성능을 향상시킬 수 있다. 기본값 그대로  
사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156975460-12bfb583-bfdd-4cb6-b010-5988c51dadf5.png" width="50%" height="50%"/>   
  
- Read Replica Source: Read Replica를 생성할 DB 인스턴스이다. exampledbinstance가  
선택되었는지 확인한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156975588-44f646f9-6355-4019-b2f8-7180ad491777.png" width="50%" height="50%"/>  
  
- DB Instance Identifier: 새로 생성될 Read Replica 인스턴스의 이름이다. 구분을   
쉽게 하기 위해 exampledbinstance-read-1로 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156975745-93a0ca61-d129-42e9-9d99-67be82ef2921.png" width="50%" height="50%"/>  
  
- DB Instance Class: 생성할 Read Replica 인스턴스의 클래스이다. Read Replica   
인스턴스를 생성할 때 성능이 더 좋은 인스턴스 클래스로 바꿀 수 있다. 여기서는   
db.t1.micro를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156975938-ed18c52a-87c7-4cb4-b5f2-90ea34772bb4.png" width="50%" height="50%"/>   
  
- Storage Type: 스토리지 타입이다. 마스터 DB 인스턴스가 Provisioned IOPS를  
사용했다면 여기서 Provisioned IOPS를 선택할 수 있다. Standard를 선택하여 일반   
스토리지를 사용할 수도 있다. 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156976323-b96d6e02-8551-428c-aebf-1515b9761eb9.png" width="50%" height="50%"/>   
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.   
보안 패치나 버그가 수정된 버전을 자동으로 업데이트한다. 예를 들며녀 MySQL의  
경우 5.6.13을 사용하고 있는데 5.6.14가 나오면 5.6.14버전으로 업데이트하게 된다.  
기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156976647-99bbe8db-8505-44d8-b02f-d4ce3266d572.png" width="50%" height="50%"/>  
  
- Database Port: MySQL 접속 포트번호이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156976867-db515d27-e386-4f8d-bfa4-e5520768c347.png" width="50%" height="50%"/>  
  
- Availability Zone: Read Replica가 생성될 가용 영역이다. 추후 각자 상황에   
맞게 리전에 속한 AZ를 선택하면 된다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156977075-668a156e-a3db-4559-89c2-c220ce35dbb5.png" width="50%" height="50%"/>  
  
- Publicly Accessible: DB를 외부에서 접근할 수 있게 하는 옵션이다. No로 설정하면   
VPC 내부에서만 접근할 수 있다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156977229-133ea6d7-6e3e-4b2f-aecf-f4f341350961.png" width="50%" height="50%"/>  
  
- Destination DB Subnet Group: Read Replica 인스턴스가 위치할 서브넷이다.   
기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156977384-9c63c107-ebd8-41b7-84c4-5592ec8ec7c0.png" width="50%" height="50%"/>    
  
설정이 완료되었으면 Yes, Create Read Replica 버튼을 클릭한다.   
RDS DB 인스턴스 목록에서 Read Replica 인스턴스(exampledbinstance-read-1)생성되고  
다. 완전히 생성되기까지 약 10 ~ 15분 정도 소요된다(DB 용량에 따라 시간은   
질 수 있다).   
  
마스터 DB 인스턴스(exampledbinstance)도 설정을 해야 하기 때문에 Status가   
modifying으로 표시된다(설정은 약 5분정도 소요된다).  
<img src="https://user-images.githubusercontent.com/33191974/156977757-2d0bd015-01f5-4609-b485-17124f2019b8.png" width="50%" height="50%"/>  
Read Replica 인스턴스(exampledbinstance-read-1)가 완전히 생성된 후 세부내용에   
엔드포인트 주소, Read Replica Source, Replication State가 표시된다.   
  
이 Read Replica 인스턴스의 엔드포인트 주소로 접속하면 마스터 DB와 동일한 데이터를  
사용할 수 있다. 단, 읽기 전용이므로 SQL 문의 INSERT, UPDATE, DELETE는 사용할 수   
없다.  

> #### Promote Read Replica   
> Promote Read Replica 기능은 Read Replica 인스턴스를 새로운 마스터 DB 인스턴스로  
> 승격(promote)시키는 기능이다. 이 기능을 사용하면 이전 마스터(소스) DB 인스턴스의  
> 복제 관계는 끊어지고, 별개의 DB 인스턴스가 된다. 이 기능을 사용하려면 먼저  
> 자동 백업(Enable Automatic Backups), 백업 유지 기간(Backup Retention Period),   
> 백업 시간(Backup Window)을 설정해야 한다.   







  







  

  
















































