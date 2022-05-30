RDS DB 인스턴스를 생성할 때 자동 백업 설정(Enable Automatic Backups)을 했다면   
DB의 특정 시점을 RDS DB 인스턴스로 생성할 수 있다. 이미 있는 DB 인스턴스의   
내용을 되돌리는 것이 아니라 특정 시점의 내용을 DB 인스턴스로 새롭게 생성하는  
방식이다.   
  
먼저 MySQL Workbench로 DB의 내용을 수정한 뒤 수정하기 전의 내용으로 복구할 것이다.   
  
MySQL Workbench를 실행하고 RDS DB 인스턴스(exampledbinstance)에 접속한다. 그리고   
ExampleDB -> ExampleTable을 선택하고 오른쪽 마우스 버튼을 클릭하면 팝업 메뉴가   
나온다. Select Rows - Limit 1000을 클릭한다. ExampleTable의 내용이 표시되면   
3번째 줄의 Andrew, LA를 추가하고 Apply 버튼을 누른다.  
  
RDS DB 인스턴스(exampledbinstance)의 ExampleTable에 데이터 추가가 완료되었다.   
아래 그림에서는 데이터가 추가된 시간이 `15:59:29`로 표시되고 있다.   
<img src="https://user-images.githubusercontent.com/33191974/156912782-cfb47ad7-fa93-4445-8fdd-e13bc96abcf5.png" width="50%" height="50%"/>    
RDS DB 인스턴스 목록에서 방금 DB에 내용 추가했던 DB 인스턴스를 선택하고   
Restore to Point in Time을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156912855-673e4e94-173d-42c9-9d6a-77dff62a4df4.png" width="50%" height="50%"/>     
- Use Custom Restore Time: MySQL Workbench에서 필자가 데이터를 추가한 시간이   
`15:59:29`이므로 `2022 15:58:00`으로 설정했다. 이 항목은 데이터를 추가하기   
이전의 시간으로 설정해야 한다. 날짜 부분을 클릭하면 달력이 표시되어 날짜를 선택  
할 수 있다.   
  <img src="https://user-images.githubusercontent.com/33191974/156913043-eb291630-b832-4b75-8ae3-ff354ec2319b.png" width="50%" height="50%"/>  
  
- Source DB Instance: 자동 백업을 했던 DB 인스턴스의 이름이 표시된다. 복구 전에  
꼭 확인하기 바란다.   
  <img src="https://user-images.githubusercontent.com/33191974/156913085-ac7889a9-a3cd-4732-8e08-6fd26f7e02c7.png" width="50%" height="50%"/>  
  
- DB Instance Identifier: 자동 백업의 특정 시점으로 새로 생성될 DB 인스턴스의   
이름이다. exampledbinstance3를 입력한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156913268-47f68ff4-911c-4180-bb44-4c0666bd4a0d.png" width="50%" height="50%"/>  
  
- DB Engine: MySQL DB의 자동 백업이므로 변경할 수 없다.    
  <img src="https://user-images.githubusercontent.com/33191974/156913289-d239f6d1-f7a5-4f13-a6e3-825742ce9199.png" width="50%" height="50%"/>  
  
- License Model: MySQL은 General Public License만 선택할 수 있다(현재는 없음).  
- DB Instance Class: 생성할 DB 인스턴스의 클래스이다. 자동 백업의 특정 시점으로   
DB 인스턴스를 생성할 때 성능이 더 좋은 인스턴스 클래스로 바꿀 수 있다.   
여기서는 db.t1.micro를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156913380-79f41f0e-5c87-498d-8be6-bf459d0fc4e6.png" width="50%" height="50%"/>  
  
- Multi-AZ-Deployment: 장애에 자동으로 대처하는 Failover 기능을 위한 다중 가용  
영역(Multi Availability Zone) 복제 옵션이다. No를 선택한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156913441-914a65bf-5ab7-439f-adfa-d1eec7ba16f8.png" width="50%" height="50%"/>  
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.   
기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156913487-041383fd-337f-4f37-b12e-73690ed851bb.png" width="50%" height="50%"/>   
  
- Database Port: MySQL 접속 포트 번호이다. 기본값 그대로 사용한다.   
- Storage Type: 스토리지 타입이다. 자동 백업을 했던 DB 인스턴스가 Provisioned   
IOPS를 사용했다면 여기서 Provisioned IOPS를 선택할 수 있다. Standard를 선택하여   
일반 스토리지를 사용할 수도 있다. 기본값 그대로 사용한다.       
  <img src="https://user-images.githubusercontent.com/33191974/156913603-7e76b34e-74b5-4b82-83a1-7c213e0e380b.png" width="50%" height="50%"/>   
  
- VPC: DB 인스턴스가 위치할 네트워크(VPC)이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156913657-d269eaa4-a990-412c-a590-000afbe96fb5.png" width="50%" height="50%"/>  
  
- DB Subnet Group: DB 인스턴스가 위치할 서브넷이다. 위에서 Default VPC 이외의   
VPC를 선택했을 때 이 서브넷을 설정할 수 있다. 기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156913699-ca06c19f-f048-454e-84b0-58574a13ef53.png" width="50%" height="50%"/>  
  
- Publicly Accessible: DB를 외부에서 접근할 수 있게 하는 옵션이다. No로 설정하면   
VPC 내부에서만 접근할 수 있다. Yes를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156913741-d6c7d5ee-d4fd-48fe-867c-31b08502cca7.png" width="50%" height="50%"/>  
  
- Availability Zone : DB 인스턴스가 생성될 가용 영역이다. 자동 백업을 했던   
DB 인스턴스의 AZ가 기본적으로 선택된다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156913789-f9617aa7-990f-4083-8fbe-ef513ffeb987.png" width="50%" height="50%"/>  
  
- Option Group: DB 옵션이다. MySQL은 특별히 지정하지 않아도 된다. 기본값 그대로   
사용한다.  

설정이 완료되어쓰면 Lauch DB Instance 버튼을 클릭한다.   
  
RDS DB 인스턴스 목록에서 자동 백업의 특정 시점으로 DB 인스턴스가 생성되고 있다.   
완전히 생성되기까지 10분정도 소요된다(DB의 용량에 따라 시간은 달라질 수 있다).
  
![image](https://user-images.githubusercontent.com/33191974/156913879-a046ec1c-6a9a-482d-a303-76cab7a75b32.png" width="50%" height="50%"/>   
생성한 DB 인스턴스에 접속하기 위해 Security Group을 설정하자.   
  
MySQL Workbench에서 자동 백업의 특정 시점으로 생성한 RDS DB 인스턴스에 접속해보면     
아래 그림과 같이 ExampleTable에 Andrew, LA를 추가하기 전의 데이터를 확인할 수 있다.   
![image](https://user-images.githubusercontent.com/33191974/156914227-8f72d8d9-1a68-4e71-9b4c-3d194caae0a9.png" width="50%" height="50%"/>     
이처럼 자동 백업의 특정 시점을 RDS DB 인스턴스로 복구할 수 있다.  



  


































