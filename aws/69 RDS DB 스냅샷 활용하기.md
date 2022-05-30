EBS(Elastic Block Storage)와 마찬가지로 RDS도 스냅샷을 생성할 수 있다. RDS DB  
스냅샷은 DB의 전체 내용 중 특정 시점을 파일로 저장한 형태이다.   
  
DB 자동 백업과 DB 스냅샷은 차이점이 있다.  
- DB 자동 백업: RDS DB 인스턴스를 삭제하면 DB 자동 백업도 함께 삭제된다.   
- DB 스냅샷: RDS DB 인스턴스를 삭제하더라도, DB 스냅샷은 계속 유지된다. 그리고   
다른 리전으로 복사할 수 있다.   

# RDS DB 스냅샷 생성하기   
이제 RDS DB 인스턴스(exampledbinstance)의 DB 스냅샷을 생성해보자. RDS DB   
인스턴스 목록(Instances)에서 RDS DB 인스턴스(exampledbinstance)를 선택하고   
스냅샷 생성을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156910036-510c6a42-ca39-48ed-8fa0-5533226238d9.png" width="50%" height="50%"/>  

> #### Snapshots  
> Snapshots 메뉴에서도 DB 스냅샷을 생성할 수 있다.  

Snapshot Name에는 생성할 DB 스냅샷의 이름을 설정한다. 여기서는 examplesnapshot을  
입력하고, Yes, Take Snapshot 버튼을 클릭한다.  
  
DB 스냅샷을 생성하며녀 자동으로 DB 스냅샷 목록(Snapshots)으로 이동한다. DB  
스냅샷 목록에는 DB 스냅샷(examplesnapshot)이 생성중이다. 완전히 생성되기까지   
약 2 ~ 3분정도 소요된다. Status가 available로 표시되면 생성이 완료된 것이며  
이 DB 스냅샷으로 RDS DB 인스턴스를 생성하거나 다른 리전으로 복사할 수 있다.     
<img src="https://user-images.githubusercontent.com/33191974/156910138-c7f672c8-cd4a-4463-9fc2-743eeb6bb43b.png" width="50%" height="50%"/>   

> #### Multi-AZ 복제와 DB 스냅샷 생성  
> Multi-AZ 복제를 사용했을 때에는 예비 인스턴스(Standby)에서 스냅샷을 생성하게   
> 되므로 메인 인스턴스의 I/O 성능에 영향을 주지 않는다.   































