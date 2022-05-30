RDS DB 인스턴스는 다른 리전으로 이전할 수 없다. 따라서 RDS DB 인스턴스를  
RDS DB 스냅샷으로 생성한 뒤 다른 리전으로 복사해야 한다. RDS DB 스냅샷 목록  
(Snapshots)에서 DB 스냅샷(examplesnapshot)을 선택하고 스냅샷 복사를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156972681-ce10cc5b-4d46-475c-b306-26fb11fbb02b.png" width="50%" height="50%"/>   
RDS DB 스냅샷을 복사한다.   
- Source DB Snapshot: 복사할 DB 스냅샷의 이름이 표시된다.  
  <img src="https://user-images.githubusercontent.com/33191974/156972845-f6dda736-3457-42bf-b90e-1d86582253df.png" width="50%" height="50%"/>   
  
- Destination Region: 복사할 리전이다. US West(N. California)를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156972968-82865974-0c42-4f23-81d5-7d838cc781d6.png" width="50%" height="50%"/>  
  
- New DB Snapshot Identifier: 새로 만들어질 DB 스냅샷의 이름이다. examplesnap  
shot을 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156973131-0ed4507a-e650-4d4a-8753-c862309548f7.png" width="50%" height="50%"/>  
  
설정이 완료되었으면 Yes, Copy Snapshot 버튼을 클릭한다.  
RDS DB 스냅샷 복사가 시작되었다. here 링크를 클릭하여 US West(N. California)의  
RDS DB 스냅샷 목록으로 이동한다.  
<img src="https://user-images.githubusercontent.com/33191974/156973471-569ce466-a372-4eb4-b9e6-b0a24bf76e8b.png" width="50%" height="50%"/>    
RDS DB 스냅샷 목록(Snapshots)으로 이동했다(화면 맨 위의 리전이 N. California로   
선택되어 있는지 확인한다). 방금 복사한 RDS DB 스냅샷이 표시된다. 복사되는 데   
시간이 약간 걸린다.   
<img src="https://user-images.githubusercontent.com/33191974/156973716-7a91b7be-5764-42e2-9b5b-57143d3a71fc.png" width="50%" height="50%"/>    
이제 이 RDS DB 스냅샷으로 US West (N. California) 리전에서 RDS DB 인스턴스를  
생성하여 DB를 운영하면 된다.   





  
  
  







































