RDS DB 인스턴스가 완전히 생성되었더라도 엔드포인트 주소로 접속이 안된다.  
Security Group에서 접속이 차단되어 있기 때문이다.  
  
우리는 DB 인스턴스를 생성할 때 Security Gruop을 기본값인 default(VPC)로 설정  
했다. default(VPC)는 모든 트래픽에 대해 inbound가 열려있지만 접속 가능한 IP  
대역(Source)은 default 자기 자신으로 설정되어 있다. 즉, 같은 default(VPC)   
Security Group 설정 안에서만 접속이 허용되므로 외부에서는 접속할 수 없다.  
외부에서 엔드포인트 주소에 접속하려면 RDS DB 인스턴스(MySQL) 전용 Security   
Group을 생성한다. 그리고 MySQL 포트(3306)를 열어줘야 한다.   
  
RDS DB 인스턴스용 Security Group을 생성해보자. AWS 콘솔의 메인 화면에서   
Compute & Networking의 EC2를 클릭한다.   
NETWORK & SECURITY -> Security Groups를 클릭한 뒤 위쪽의 Create Security   
Group 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156882055-4865ec91-3c4d-4834-9627-0b97a5320c26.png" width="50%" height="50%"/>    
RDS DB 인스턴스용 Security Group을 생성한다.  
- Security group name: Security Group의 이름이다. MySQL DB Instance를 입력한다.  
- Description: Security Group 설명이다. MySQL DB Instance를 입력한다.  
- VPC: Security Group이 적용될 VPC이다. 기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156882301-f7584ed9-363a-42b1-9445-a1361ee12872.png" width="50%" height="50%"/>    
  
외부에서 들어오는 트래픽인 Inbound 탭을 선택한다(Inbound가 기본으로 선택되어  
있을 것이다). 아래쪽 Add Rule 버튼을 클릭한다.   
- Type: 트래픽 종류이다. 우리는 MySQL 포트(3306)를 열어야 하므로 MySQL를   
선택한다.   
- Protocol: 프로토콜이다. MySQL을 선택하면 자동으로 TCP가 설정된다.   
- Port Range: 포트 번호이다. MySQL을 선택하면 자동으로 3306이 설정된다.   
- Source: 접속 가능한 IP 또는 IP 대역이다. Anywhere를 선택한다(실무에서는  
My IP를 선택하여 자신의 IP만 접속할 수 있도록 설정하거나, Custom IP를 선택하여  
특정 IP 대역을 설정하도록 한다).  
설정이 완료되었으면 Create 버튼을 클릭한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156886120-32e980ed-88b7-4f8b-8bfa-5214d02a36ed.png" width="50%" height="50%"/>    

> #### Oracle과 PostgreSQL의 포트 번호   
> MySQL과 Microsoft SQL Server는 Security Group을 생성할 때 Type에서 미리  
> 정의된 트래픽 종류를 선택할 수 있다. Oracle과 PostgreSQL은 미리 정의된 것이   
> 없으므로 Custom TCP Rule을 선택하고 포트 번호를 입력해야 한다.  
> - Oracle: 1521
> - PostgreSQL: 5432

Security Group 목록(NETWORK & SECURITY -> Security Groups)에 MySQL DB Instance  
Security Group이 생성되었다. 
  
<img src="https://user-images.githubusercontent.com/33191974/156884137-189f07d7-1e21-4498-b9cc-95348541f5a2.png" width="50%" height="50%"/>   
이제 다시 RDS DB 인스턴스 목록(Instances)으로 이동한다. DB 인스턴스 목록(  
Instances)에서 DB 인스턴스(exampledbinstance)를 선택한 뒤 마우스 오른쪽 버튼을   
클릭하면 팝업 메뉴가 나온다. Modify를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156882739-8553811a-eb8c-4bea-9bae-fe019df93264.png" width="50%" height="50%"/>  
RDS DB 인스턴스의 설정을 변경한다. Security Group에서 방금 생성한 MySQL DB  
Instance를 선택하고 아래쪽 Continue 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156882823-5b198b0d-cb02-4ce2-9f2b-7e8c4a66296b.png" width="50%" height="50%"/>   
설정한 내용이 이상이 없는지 확인한다. 이상이 없으면 Modify DB Instance 버튼을   
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156882931-c2a230d2-66f0-49f1-a2fe-9e203d24ce0e.png" width="50%" height="50%"/>  
RDS DB 인스턴스 목록을 보면 exampledbinstance의 Security Group에 default가   
삭제되고, MySQL DB Instance가 추가되고 있다. 잠시 기다리면 active 상태로   
바뀐다.   
<img src="https://user-images.githubusercontent.com/33191974/156882995-7ccfa742-36f9-44af-9a2e-d64e32544b4a.png" width="50%" height="50%"/>  
이제 외부에서 MySQL 데이터베이스에 접속할 수 있게 되었다.   












































