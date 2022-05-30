RDS DB 인스턴스의 생성이 완료되었으면 실제로 생성이 되었는지 확인해보자.  
여기서는 GUI 도구인 MySQL Workbench를 사용한다.   
[http://dev.mysql.com/downloads/tools/workbench/](http://dev.mysql.com/downloads/tools/workbench/)에 접속한 뒤  
각자 운영체제에 맞는 버전을 다운로드한다. 다운로드에는 오라클 계정이 필요하다.  
Windows 버전의 경우 MSI Installer를 다운로드한다(ZIP Archive는 MySQL 테이블을   
생성하려면 관련 라이브러리를 따로 설치해야 하기 때문에 번거롭다).  
  
MySQL Workbench Windows 버전을 기준으로 설명한다. 다운로드한 파일을 설치하고   
MySQLWorkbench.exe를 실행한다. MySQL Workbench가 실행되면 MySQL Connections  
옆의 `+` 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156883298-44d54216-99af-4410-b871-84eab3987ab0.png" width="50%" height="50%"/>    
새 MySQL 연결을 생성한다.   
- Connection Name: 연결의 이름이다. RDS를 입력한다.   
- Connection Method: 접속 방식이다. 기본값 그대로 Standard(TCP/IP)를 사용한다.  
- Hostname: RDS DB 인스턴스의 엔드포인트 주소를 입력한다. 단, 포트 번호는   
제외하고 도메인만 입력한다. 엔드포인트 주소는 RDS DB 목록에서 DB 인스턴스를   
선택한 뒤 세부 내용에서 확인할 수 있다.  
- Port: MySQL 접속 포트 번호이다. 기본값 그대로 사용한다.   
- Username: RDS DB 인스턴스를 생성할 때 설정했던 Master Username을 입력한다.   
예제에서는 admin으로 설정했다.  
Password의 Store in Vault ... 버튼을 클릭한다.  
RDS DB 인스턴스를 생성할 때 설정했던 Master Password를 입력하고 OK 버튼을  
클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156883525-057e0940-9a6a-438e-863e-24f822b7f102.png" width="50%" height="50%"/>   
설정이 완료되었으면 Setup New Connection 창의 OK 버튼을 클릭한다. 이제 RDS  
연결 생성이 완료되었다. 접속이 되지 않으면 'RDS DB 인스턴스 Security Group   
생성 및 설정하기'에서 Security Group을 생성한 뒤 사용하도록 설정하였는지 확인한다.  
<img src="https://user-images.githubusercontent.com/33191974/156886238-12d47596-41eb-4d77-b19e-fae61ef63a06.png" width="50%" height="50%"/>      
MySQL Workbench에서 RDS DB 인스턴스에 접속했다. 왼쪽을 보면 RDS DB 인스턴스를   
생성할 때 함께 생성한 ExampleDB를 확인할 수 있다. 이 ExampleDB를 클릭하고  
Tables에서 마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다. Creaet Table..를  
클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156909617-84d39257-7f6a-41a8-9325-24eb287cfc84.png" width="50%" height="50%"/>  
새 테이블을 생성한다.  
- Table Name: 테이블 이름이다. ExampleTable을 입력한다.   
- Collation: 문자 데이터타입이다. 기본값 그대로 사용한다.   
- Engine: MySQL의 스토리지 엔진이다. 기본값 그대로 InnoDB를 사용한다.  
새 칼럼을 추가한다. Column Name의 빈 칸을 클릭하면 새 컬럼을 추가할 수 있다.   
아래 형식과 같이 컬럼을 생성한다.
- Column Name: id, DateType: INT, PK 체크, NN 체크, AI체크
- Column Name: name, DateType: VARCHAR(45)
- Column Name: address, DataType: VARCHAR(45)  
컬럼 추가가 완료되었으면 Apply 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156909712-57f890f9-a1d6-445d-8c7e-eee67c701715.png" width="50%" height="50%"/>  
테이블을 생성하는 SQL 문이 표시된다. Apply 버튼을 클릭하고, 아무 에러없이   
적용이 완료되면 Finish 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/156909766-3097b8f5-4cd0-4670-a588-6672d68c6630.png" width="50%" height="50%"/>  
MySQL Workbench의 왼쪽을 보면 방금 생성한 ExampleTable을 확인할 수 있다.   
이 ExampleTable에서 마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다.   
Select Rows - Limit 1000을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156909826-50b73275-4bde-4608-bd99-882e8d67b99e.png" width="50%" height="50%"/>    
ExampleTable의 데이터가 출력된다. 테이블만 생성했으므로 아무 데이터가 없다.   
이 곳에서 엑셀처럼 데이터를 입력할 수 있다. 그림처럼 John, New York과 Maria,  
Seattle을 추가한다. 입력이 완료되었으면 Apply 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/156909860-92af6241-28be-484a-b5d1-dd574070f347.png" width="50%" height="50%"/>   
ExampleTable에 데이터 추가가 완료되었다. id 컬럼은 AI(Auto Increment)로 설정  
했기 때문에 자동으로 값이 지정된다.   
이렇게 MySQL Workbench를 이용하여 RDS DB 인스턴스의 내용을 확인하거나 데이터를   
추가할 수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/156909878-55631545-f513-4a7c-994d-5f081823cceb.png" width="50%" height="50%"/>   










































