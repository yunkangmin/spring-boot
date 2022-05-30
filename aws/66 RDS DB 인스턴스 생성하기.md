RDS 인스턴스를 생성하고 사용해보자. 데이터베이스 엔진은 가장 손쉽게 사용할 수 있는  
MySQL로 한다. AWS 콘솔로 접속한 뒤 메인화면에서 Database의 RDS를 클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/156877296-66f3dcca-7afc-4d88-bb78-67606f58db1d.png" width="50%" height="50%"/>    
오른쪽 위에서 RDS의 리전을 변경할 수 있다. 여기서는 서울을 선택한다.   
<img src="https://user-images.githubusercontent.com/33191974/156877342-7b4efcfb-4f63-433a-9bc1-4339d0effd61.png" width="50%" height="50%"/>  
데이터베이스 생성 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156877414-b8f4356f-14ef-43ca-909c-0f26248de0ad.png" width="50%" height="50%"/>   
RDS DB 인스턴스에 사용할 데이터베이스 엔진을 선택한다. MySQL Community Edition의  
Select 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156877470-b3238ee9-5c63-43a7-af24-6c51fdb845cf.png" width="50%" height="50%"/>    
장애에 대응할 수 있는 이중화를 위한 다중 가용 영역(Multi-AZ) 복제와 고성능  
I/O를 제공하는 Provisioned IOPS Storage를 사용할 것인지 선택한다. Multi-AZ와   
Provisioned IOPS Storage를 사용하면 추가요금이 발생한다. No를 선택하여 이 두  
가지 기능을 사용하지 않는 프리티어용 인스턴스를 생성한다.   
<img src="https://user-images.githubusercontent.com/33191974/156877573-c13bbfe0-74e4-4369-94a6-5a54abdc9127.png" width="50%" height="50%"/>   
RDS DB 인스턴스 세부 설정이다.   
- License Model: MySQL은 general-public-license만 선택할 수 있다.   
- DB Engine Version: 사용할 MySQL의 버전이다. 특정 버전을 사용해야 하는 환경이   
아니라면 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156877758-652d3a9d-fd5b-456b-9021-7b98f4b85dab.png" width="50%" height="50%"/>  

- DB Instance Class: 생성할 DB 인스턴스 클래스이다. 프리티어에서 무료로 사용할   
수 있는 마이크로 인스턴스(db.t1.micro)를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156877785-5a401676-2bf7-4050-842a-db0f52abc0b7.png" width="50%" height="50%"/>  
  
- Multi-AZ Deployment: 장애에 자동으로 대처하는 Failover 기능을 위한 다중 가용  
영역(Multi Availability Zone) 복제 옵션이다. No를 선택한다(자동으로 비활성화  
되어 있다).     
  <img src="https://user-images.githubusercontent.com/33191974/156877841-a028991c-d589-47d8-aa8a-0c699351ec20.png" width="50%" height="50%"/>   

- Allocated Storage: DB에서 데이터를 저장할 스토리지의 용량이다. 최소 5GB에서   
3072(3TB)까지 설정할 수 있다. 5GB로 설정한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156877950-5f54ee4a-f476-4727-bc72-6f3b8bc53d3b.png" width="50%" height="50%"/>  
  
- Use Provisioned IOPS: 고성능 I/O 옵션이다. 이 옵션을 사용하면 스토리지의 읽기  
/쓰기 성능을 원하는 대로 조절할 수 있다. 단, 추가비용을 지불해야 한다.   
기본값 그대로 사용한다(현재는 안보임).     
- DB Instance Identifier: DB 인스턴스의 이름이다(이름은 같은 리전 안에서 중복   
되면 안된다). exampledbinstance-01로 설정한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156878167-0fb45eb5-4945-4aee-80e5-ef60215a5e19.png" width="50%" height="50%"/>   

- Master Username: MySQL DB 관리자 계정이다. admin으로 설정한다.   
- Master Password, Confirm Password: MySQL DB 관리자 계정의 비밀번호이다.   
여러분이 사용하고 싶은 비밀번호를 설정한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156878287-95f1fe7a-0dfa-4038-9ed5-ed435352b8d3.png" width="50%" height="50%"/>  
  
DB 인스턴스 추가 설정이다.  
- VPC: DB 인스턴스가 위치할 네트워크(VPC)이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156878405-cdd9cd21-7eaf-4c27-85d4-abadd2e85724.png" width="50%" height="50%"/>  

- DB Subnet Group: DB 인스턴스가 위치할 서브넷이다. 위에서 Default VPC이외의   
VPC를 선택했을 때 이 서브넷을 설정할 수 있다. 기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156878445-153512d4-fca4-4b37-b0ae-dea8636b1591.png" width="50%" height="50%"/>  
  
- Publicly Accessible: DB를 외부에서 접근할 수 있게 하는 옵션이다. No로 설정하면   
VPC 내부에서만 접근할 수 있다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156885951-008d24ae-8488-4431-bbd8-4767d5f384ea.png" width="50%" height="50%"/>   
  
- Availability Zone: DB 인스턴스가 생성될 가용 영역(Availability Zone)이다.   
EC2 인스턴스에서 DB에 접속한다면 같은 AZ에 있는 것이 좋다. 기본값 그대로  
사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156878651-58e4e197-9cfe-4503-af0f-979312d5cf1e.png" width="50%" height="50%"/>  
  
- VPC Security Group: 방화벽 설정인 Security Group이다. 기본값 그대로 사용한다.   
이 Security Group은 나중에 DB 인스턴스 전용으로 따로 생성해야 한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156878749-7397d1a0-863f-4f9b-820c-31050eacbbeb.png" width="50%" height="50%"/>  
  
- Database Name: 생성할 DB이름이다. ExampleDB로 설정한다. 아무것도 입력하지   
않으면 DB 인스턴스에서 MySQL 서버만 실행되고 DB는 생성되지 않는다.   
  <img src="https://user-images.githubusercontent.com/33191974/156878813-3119754e-a9d6-41cf-a0c6-f7be12ea708d.png" width="50%" height="50%"/>  
  
- Database Port: DB 접속 포트 번호이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156878847-fd3651ab-dae4-4e40-a8d5-9ba3d46c1c82.png" width="50%" height="50%"/>   
  
- Parameter Group: MySQL을 실행할 때 필요한 파라미터 집합이다. DB 인스턴스 생성  
후 Parameter Group을 추가할 수 있다(my.cnf 파일을 생성하는 것과 동일하다).   
기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156878899-83543d09-edf9-4c7e-9428-8c44077086b9.png" width="50%" height="50%"/>  
  
- Option Group: DB 옵션이다. MySQL은 특별히 지정하지 않아도 된다. 기본값 그대로  
사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156878932-682e216e-f8e0-4477-91ac-65b737fde066.png" width="50%" height="50%"/>  
  
이어지는 DB 인스턴스 추가 설정이다.   
- Backup: 자동 백업 옵션이다. 이 자동 백업을 사용하면 원하는 시점으로 데이터를   
복구할 수 있는 PIT(Point in Time) 복구를 사용할 수 있다. PIT 복구는 최근 5분   
전 상태로 되돌릴 수 있으며 1초 단위로 설정이 가능하다.    
  <img src="https://user-images.githubusercontent.com/33191974/156879109-eae61ee9-eece-45a3-8d28-64497f90859a.png" width="50%" height="50%"/>  
  
- Backup Retention Period: 백업 데이터 유지 기간이다. 최대 35일까지 설정할 수  
있다. 여기서 지정한 날짜 이전까지 되돌릴 수 있다.   
  <img src="https://user-images.githubusercontent.com/33191974/156879156-3ca38ca1-5b7c-4e2e-b6e4-66488f538968.png" width="50%" height="50%"/>  
  
- Backup Window: 백업 시간이다. 기본값은 No Preference이다. 여기서는 Select  
Window를 선택하고 Start Time을 `00:00`, Duration을 0.5로 설정한다. UTC기준으로   
00시 00분에 백업을 시작하며 백업 시간은 0.5시간(30분)이다. 데이터의 용량이   
크면 설정한 백업 시간을 넘길 수도 있다. 이 Duration을 설정하는 이유는 아래   
Maintenance Window의 시간과 겹치지 않게 하기 위해서이다.  
  <img src="https://user-images.githubusercontent.com/33191974/156879365-b798a923-4e50-4237-be2e-e4ca9e7bc23c.png" width="50%" height="50%"/>  
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.  
보안 패치나 버그가 수정된 버전을 자동으로 업데이트한다. 예를 들면 MySQL의 경우   
5.6.13을 사용하고 있는데(현재는 8점대 버전 사용중) 5.6.14가 나오면 5.6.14  
버전으로 업데이트한다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156879470-adcfb4e2-51f3-440f-88e7-32cfad2685a9.png" width="50%" height="50%"/>  
  
- Maintenance Window: 점검 시간이다. 기본값은 No Preference이다. 여기서는   
Select Window를 선택하고 Start Day를 Monday, Start Time을 `03:00`, Duration을  
0.5로 설정한다. UTC 기준으로 03시 00분에 점검이 시작되며 시간은 0.5시간(30분)  
이다. Duration을 설정하는 이유는 위 Backup Window의 시간과 겹치지 않게 하기   
위해서이다. 점검은 Duration에 설정한 시간보다 일찍 끝날 수 있다.   
  <img src="https://user-images.githubusercontent.com/33191974/156879636-d155828a-90ed-4032-9b82-c2bfb8a2dd5d.png" width="50%" height="50%"/>  
  
   - 이 시간에 Auto Minor Version Upgrade를 설정했다면 DB버전 업데이트 또는  
   패치가 적용된다. DB 버전 업데이트 또는 패치는 필수적인 것(보안패치)만    
   적용되며 자주 발생하지 않고 몇 달에 한 번 발생한다. DB 업데이트 또는 패치가   
   적용되는 시간 동안에는 DB 인스턴스의 실행이 중지된다.  
   - DB 인스턴스 클래스를 변경했다면 이 시간에 적용된다. DB 인스턴스 클래스를  
   변경하는 동안에는 DB 인스턴스의 실행이 중지된다.   
  
설정이 완료되었으면 Launch DB Instance 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156879743-5b47243b-d589-4b8a-922d-d751f7f2a280.png" width="50%" height="50%"/>    

> #### Multi-AZ 복제와 백업  
> Multi-AZ 복제를 사용했을 때에는 예비 인스턴스(Standby)에서 백업을 진행하게   
> 되므로 메인 인스턴스의 I/O 성능에 영향을 주지 않는다.   

> #### MySQL에서 자동백업   
> 설정화면에 나와 있는 것처럼 MySQL에서 자동 백업을 사용하려면 스토리지 엔진으로  
> InnoDB를 사용해야 한다. MyISAM은 지원하지 않는다.   

완전히 생성되기까지 약 10분 정도 소요된다.   
  
RDS DB 인스턴스가 완전히 생성되었으면 exampledbinstance-01를 선택한다. 그리고   

> #### Multi-AZ 복제와 Failover   
> Multi-AZ 복제를 사용하도록 설정한 뒤, 메인 DB 인스턴스에 장애(AZ 장애,   
> 인스턴스 중단, 네트워크 장애, 스토리지 장애)가 발생하면 자동으로 예비   
> 인스턴스가 메인 인스턴스로 승격된다. 이 기능을 Failover라고 하며 Failover가  
> 완료되는 데 걸리는 시간은 약 3 ~ 6분이다.    
> Failover 기능이 동작하면 Endpoint 주소가 가리키는 DB 인스턴스가 메인   
> 인스턴스에서 예비 인스턴스로 바뀌므로 Endpoint를 사용하는 측에서는 Failover   
> 를 위해 따로 설정을 해줄 필요가 없다. 이 Multi-AZ 복제는 MySQL과 Oracle  
> 데이터베이스 엔진에서만 지원한다.  

> #### RDS DB 인스턴스 정지(stop)  
> EC2 인스턴스와는 달리 RDS DB 인스턴스는 정지 개념이 없다. 따라서 DB 인스턴스를  
> 정지하고 싶으면 삭제(Delete)해야 한다. 단 DB 인스턴스를 삭제(Delete)할 때,   
> 최종(Final) DB 스냅샷을 생성할 수 있다. 이후 이 최종 DB 스냅샷으로 DB   
> 인스턴스를 다시 생성하여 이전 데이터 그대로 시작할 수 있다.   







  


  




  






  

  




  



  

  




  










































