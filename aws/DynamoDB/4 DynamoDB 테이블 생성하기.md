## DynamoDB 테이블 생성하기
앞에서 설계한 DB 구조대로 테이블을 생성하고, 인덱스를 설정해보자.  
AWS 콘솔로 접속한 뒤 메인 화면에서 Database의 DynamoDB를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138648491-f438a06f-c329-43ff-81a3-8d4aeead9abf.png)  
DynamoDB 메인 페이지에서 Create Table 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138648568-7edc3640-6a21-410c-8b81-b962e730cb3e.png)  
전체 유저의 점수를 저장할 테이블 이름과 기본키를 설정한다.  
- Table Name : DynamoDB의 테이블 이름이다. 리전 안에서 중복될 수 없다. 앞서 설계한 대로 UserLeaderboard를  
  입력한다.  
- Primary Key Type : 해시 기본키와 범위 기본키 또는 해시 기본키만 사용하는 옵션이다. Hash and Range를 선택한다.    
- Hash Attribute Name : 해시 기본 키의 속성(Attribute) 이름과 데이터 형식이다. Number를 선택하고 Id를 입력한다.  
- Range Attribute Name : 범위 기본 키의 속성 이름과 데이터 형식이다. String을 선택하고 Week를 입력한다.  

설정이 완료되었으면 Continue 버튼을 누른다.  
![image](https://user-images.githubusercontent.com/33191974/138649766-8295e068-f8b9-4fd2-b7e2-627bf6c14dbc.png)  
주간 전체순위를 산출할 글로벌 보조 인덱스를 추가한다.  
- Index Type : 보조 인덱스 타입이다. Global Secondary Index를 선택한다.  
- Index Hash Key : 인덱스 해시 키의 속성 이름이다. String을 선택하고 Week를 입력한다.  
- Index Range Key : 인덱스 범위 키의 속성 이름이다. Number를 선택하고 TopScore를 입력한다.  
- Index Name :  생성될 인덱스의 이름이다. 해시키와 범위키의 속성 이름을 입력하면 자동으로 생성된다.  
  기본값 그대로 사용한다.  
- Projected Attributes : 쿼리를 했을 때 가져올 속성이다. 기본값 그대로 사용한다.  
   - All Attributes : 아이템의 모든 속성을 가져온다.  
   - Table and Index Keys : 테이블 인덱스와 보조 인덱스에 키로 설정한 속성만 가져온다.  
   - Specify Attributes : 특정 속성만 가져온다. 가져올 속성 이름을 추가할 수 있고, 보조 인덱스에  
     키로 설정한 속성은 기본적으로 가져온다.  

설정이 완료되었으면 Add Index to Table 버튼을 클릭하여 인덱스르 추가한다.   
![image](https://user-images.githubusercontent.com/33191974/138650901-93a3dc97-c6a6-4c1d-ade6-ce3ba7e04465.png)  
![image](https://user-images.githubusercontent.com/33191974/138651021-206fc687-9aa4-4160-8500-ac3ea4913498.png)  
![image](https://user-images.githubusercontent.com/33191974/138650851-850698b9-55b4-4ceb-bfc5-2d6212723f06.png)    
Table Indexes에 글로벌 보조 인덱스가 추가되었다.  
![image](https://user-images.githubusercontent.com/33191974/138651151-124be7d1-09ee-403c-a681-52168da0b831.png)  
개인 최고기록을 조회할 로컬 보조 인덱스를 추가한다.  
- Index Type : 보조 인덱스 타입이다. Local Secondary Index를 선택한다. 
- Index Hash Key : 로컬 보조 인덱스는 테이블 인덱스의 해시 기본키와 같기 때문에 변경할 수 없다.  
- Index Range Key : 인덱스 범위 키의 속성 이름이다. Number를 선택하고 TopScore를 입력한다.  
- Index Name : 생성될 인덱스의 이름이다. 범위키의 속성 이름을 입력하면 자동으로 생성된다.   
  기본값 그대로 사용한다.  
- Projected Attributed : 쿼리를 했을 때 가져올 속성이다. 기본값 그대로 사용한다.  
   - All Attributes : 아이템의 모든 속성을 가져온다. 
   - Table and Index Keys : 테이블 인덱스와 보조 인덱스에 키로 설정한 속성만 가져온다.  
   - Specify Attributes : 특정 속성만 가져온다. 가져올 속성 이름을 추가할 수 있고, 보조 인덱스에  
     키로 설정한 속성은 기본적으로 가져온다. 
     
설정이 완료되었으면 Add Index to Table 버튼을 클릭하여 인덱스를 추가한다.  

![image](https://user-images.githubusercontent.com/33191974/138657083-f4fafaf3-eb61-4eb3-83e4-8279b3da4213.png)
![image](https://user-images.githubusercontent.com/33191974/138657296-4902f8e6-170f-40d2-880d-7927c5f5dea0.png)
![image](https://user-images.githubusercontent.com/33191974/138657381-950ba063-30d7-4e68-8686-9a5f3a3be1fd.png)  
  
DynamoDB 처리량 단위인 읽기/쓰기 용량 유닛을 설정한다.  
- Help me calculate how much throughtput capacity I need to provision : 이 부분을 체크하면  
  평균 데이터 크기와 초당 읽고 쓴 아이템 수를 입력하여 읽기/쓰기 용량 유닛을 계산할 수 있다.  
- Read Capacity Units : 읽기 용량 유닛이다. 기본값 그대로 사용한다.  
- Write Capacity Units : 쓰기 용량 유닛이다. 기본값 그대로 사용한다.  
- Throughtput Capacity Allocation : 생성된 보조 인덱스에도 읽기/쓰기 용량 유닛을 설정할 수 있다.  
  (각 숫자를 클릭하면 값을 수정할 수 있다). 기본값 그대로 사용한다.  
  
Continue 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138659129-212e3b2f-5558-4eb0-b9bc-8b2e25ce7302.png)

DynamoDB를 사용하다가 설정한 전체 처리량에서 일정 비율 이상을 넘어서면 알람을 발생시킬 수 있다.  
- Use Basic Alarms : 기본 알람 사용 옵션이다. 이번 실습에서는 체크를 해제하여 사용하지 않는다.  
- 60분 동안 전체 처리량에서 어느 정도 비율을 넘어서면 알람을 발생시킬지 설정하는 옵션이다.  
  95%, 90%, 85%, 80%, 75%를 선택할 수 있으며 80%가 기본값이다.  
- Send notification to : 이메일 주소를 입력하여 내용을 받을 수 있다. 이 부분은 SNS(Simple  
  Notification Service)와 연계된다.  

상세한 조건의 알람은 CloudWatch에서 설정할 수 있다. 설정이 완료되면 Continue 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138659180-bd41c0ca-5362-4a5f-8d6f-b7b967ac79a4.png)  
DynamoDB 테이블 목록에 UserLeaderboard 테이블이 생성되었다. 처음에는 Status가 CREATING으로 나오며  
완전히 생성되기까지 약 10초정도 소요된다.  
  
![image](https://user-images.githubusercontent.com/33191974/138659277-f22d3d58-e926-4e4a-8b97-49b7eb20d8fc.png)  

이제 주간 친구 순위 산출을 위해 FriendsLeaderboard 테이블을 생성한다.  
![image](https://user-images.githubusercontent.com/33191974/138659496-923092aa-9683-4f74-876d-4d34d4f9756f.png)  
유저의 친구의 점수를 저장할 테이블 이름과 기본키를 설정한다.  
- Table Name : DynamoDB의 테이블 이름이다. 리전 안에서 중복될 수 없다. 앞서 설계한 FriendsLeaderboard를   
  입력한다.  
- Primary Key Type : 해시 기본키와 범위 기본키 또는 해시 기본키만 사용하는 옵션이다. Hash and Range를 입력한다.   
- Hash Attribute Name : 해시 기본키의 속성(Attribute) 이름과 데이터 형식이다. Number를 선택하고  
  Id를 입력한다.  
- Range Attribute Name : 범위 기본키의 속성 이름과 데이터 형식이다. String을 선택하고 FriendIdAndWeek를  
  입력한다.  
![image](https://user-images.githubusercontent.com/33191974/138660248-6dee8639-2f82-4e65-b286-dbed8076229a.png)  

주간 친구 순위를 산출할 글로벌 보조 인덱스를 추가한다.  
- Index Type : 보조 인덱스 타입이다. Global Secondary Index를 선택한다.  
- Index Hash Key : 인덱스 해시키의 속성 이름이다. String을 선택하고 FriendIdAndWeek를 입력한다.  
- Index Range Key : 인덱스 범위키의 속성 이름이다. Number를 선택하고 Score를 입력한다.  
- Index Name : 생성될 인덱스의 이름이다. 해시키와 범위키의 속성 이름을 입력하면 자동으로 생성된다.  
  기본값 그대로 사용한다.  
- Projected Attributes : 쿼리를 했을 때 가져올 속성이다. 기본값 그대로 사용한다. 
   - All Attributes : 아이템의 모든 속성을 가져온다.
   - Table and Index Keys : 테이블 인덱스와 보조 인덱스에 키로 설정한 속성만 가져온다.
   - Specify Attibutes : 특정 속성만 가져온다. 가져올 속성 이름을 추가할 수 있고, 보조 인덱스에  
     키로 설정한 속성은 기본적으로 가져온다.

설정이 완료되었으면 Add Index to Table 버튼을 클릭하여 인덱스를 추가한다.
![image](https://user-images.githubusercontent.com/33191974/138661234-a5108681-d727-4b6d-94ec-f0ba4f8487de.png)
![image](https://user-images.githubusercontent.com/33191974/138661294-863878a4-ee40-41ca-8c5f-be6c1734cd44.png)  
  
DynamoDB 처리량 단위인 읽기/쓰기 용량 유닛을 설정한다.
![image](https://user-images.githubusercontent.com/33191974/138661454-2b7367ad-d3bf-4d09-a69c-5447097e67d1.png)  
테이블을 생성한다.  
![image](https://user-images.githubusercontent.com/33191974/138661533-992ae0d7-f75e-4491-8a8a-73df127afc4f.png)
![image](https://user-images.githubusercontent.com/33191974/138661645-c4e26129-e47f-4b04-8675-c5199dbf22a5.png)




  
  
  











































