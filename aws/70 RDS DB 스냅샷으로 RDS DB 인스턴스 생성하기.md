EBS 스냅샷으로 EBS 볼륨을 생성하는 것과 마찬가지로 RDS DB 스냅샷으로 RDS DB  
인스턴스를 생성할 수 있다. RDS DB 스냅샷 목록(Snapshots)에서 RDS DB 스냅샷(  
examplesnapshot)을 선택하고 위쪽 Restore Snapshot 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156910269-28bcb975-d623-47db-9707-375956b50480.png" width="50%" height="50%"/>     
RDS DB 스냅샷으로 RDS DB 인스턴스 생성하기 전에 설정이 필요하다.   
- DB Engine: MySQL DB 스냅샷을 생성했으므로 변경할 수 없다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910400-b458251f-f789-467d-9e2e-de5bdf013b3c.png" width="50%" height="50%"/>  

- License Model: MySQL은 General Public License만 선택할 수 있다(현재는 없는듯).    
- DB Instance Class: 생성할 DB 인스턴스의 클래스이다. DB 스냅샷으로 DB 인스턴스를  
생성할 때 성능이 더 좋은 인스턴스 클래스로 바꿀 수 있다. 여기서는 db.t1.micro를  
선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910392-cd5c5285-adbc-45cd-984c-973c6b0aff48.png" width="50%" height="50%"/>  
  
- Multi-AZ Deployment: 장애에 자동으로 대처하는 Failover 기능을 위한 다중 가용   
영역(Multi Available Zone) 복제 옵션이다. No를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910440-b624d6aa-12e8-4f5b-abcf-0b99f5a195a6.png" width="50%" height="50%"/>  
  
- Storage Type: 스토리지 타입이다. DB 스냅샷을 생성했던 DB 인스턴스가 Provisioned  
IOPS(현재는 없어진 메뉴인듯)를 사용했다면 여기서 Provisioned IOPS를 선택할 수 있다.    
  <img src="https://user-images.githubusercontent.com/33191974/156910480-ba396aa7-a5c9-4335-b6d3-329ecb650109.png" width="50%" height="50%"/>  
  
- DB Instance Identifier: DB 스냅샷을 이용하여 새로 생성될 DB 인스턴스의 이름이다.   
exampledbinstance2를 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910514-7b1f2fa4-1997-48aa-b103-4a7b0df74199.png" width="50%" height="50%"/>  
  
- VPC: DB 인스턴스가 위치할 네트워크(VPC)이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910560-56e00986-c8a1-4172-9301-dd836f6b7546.png" width="50%" height="50%"/>  
  
- DB Subnet Group: DB 인스턴스가 위치할 서브넷이다. 위에서 Default VPC이외의  
VPC를 선택했을 때 이 서브넷을 설정할 수 있다.  기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910597-fff78b6f-35b6-4eb3-9d48-6ca08aba6914.png" width="50%" height="50%"/>  
  
- Publicly Accessible: DB를 외부에서 접근할 수 있게 하는 옵션이다. No로 설정하면   
VPC 내부에서만 접근할 수 있다. Yes를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910673-13c78f21-d529-4c6f-8ddb-028da5e666dd.png" width="50%" height="50%"/>  
   
- Availability Zone: DB 인스턴스가 생성될 가용 영역(Availability Zone)이다.   
EC2 인스턴스에서 DB에 접속한다면 같은 AZ에 있는 것이 좋다. 기본값 그대로 사용한다.     
현재는 없는듯.  
- Database Port: MySQL 접속 포트 번호이다. 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/156910792-51e119ce-2e0b-4c58-b129-5f10254143e6.png" width="50%" height="50%"/>  
  
- Option Group: DB 옵션이다. MySQL은 특별히 지정하지 않아도 된다. 기본값 그대로   
사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156910829-76efd11f-588e-441b-87ee-0f2647295cfc.png" width="50%" height="50%"/>  
  
- Auto Minor Version Upgrade: 자동으로 마이너 버전을 업데이트하는 옵션이다.   
보안 패치나 버그가 수정된 버전을 자동으로 업데이트한다. 예를 들면 MySQL의 경우   
5.6.13사용하는데 5.6.14가 나오면 5.6.14 버전으로 업데이트하게 된다. 기본값  
그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/156910899-fd32756c-0122-49e3-b3b4-4e988cdef543.png" width="50%" height="50%"/>  
  
RDS DB 인스턴스 목록으로 돌아왔다. DB 인스턴스 목록에서 DB 스냅샷으로 DB 인스턴스  
가 생성되고 있다. 완전히 생성되까지 약 10분정도 소요된다.  
  
DB 인스턴스가 완전히 생성된 후 세부 내용에 엔드포인트 주소가 표시된다.  
생성한 DB 인스턴스에 접속하려면 Security Group을 설정해줘야 한다.   
'RDS DB 인스턴스 Security Group 생성 및 설정하기'를 참조하여 Security Group을  
설정하자.   
  
'RDS DB 인스턴스 사용하기'를 참조하여 DB에 접속하자.  
  
<img src="https://user-images.githubusercontent.com/33191974/156911642-85a838c4-7be0-4bc8-b733-d1911134cce2.png" width="50%" height="50%"/>  
이처럼 RDS DB 스냅샷에 저장된 내용을 RDS DB 인스턴스로 복구할 수 있다.  

 
  




  


  
  
  
    


  




- 


































