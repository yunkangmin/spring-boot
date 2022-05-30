## DynamoDB 데이터 쿼리하기
앞서 설계했던 대로 테이블도 만들고 데이터도 추가했다.  
이제 원하는 데이터를 얻는 쿼리를 해보자. 먼저 주간 전체순위를 산출한다.  
DynamoDB 테이블 목록에서 UsersLeaderboard 테이블을 선택하고 위쪽 Explore Table 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138678275-eb3cb5e0-fe1e-4351-98af-d306a4c48900.png)  

UsersLeaderboard 테이블에서 주간 전체순위를 쿼리한다.  
- Scan, Query : Scan은 테이블의 모든 아이템을 출력하는 것이고, Query는 지정한 조건에 맞은 내용을  
  출력하는 것이다. Query를 선택한다. 
- Index Name : 검색할 인덱스이다. [Global] Week-TopScore-Index: Week, TopScore를 선택한다.
- Hash Key : 검색할 해시키이다. 인덱스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.
   - Week equal to : 주 단위로 검색할 것이므로 2014-05-09, 2014-05-15를 입력한다.
- Range Key : 검색할 범위키이다. 인덱스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.
   - TopScore : 최대점수를 999999라 보고, 99999를 입력한다.
   - less than : 범위키는 검색조건을 정할 수 있다. 지정한 값보다 작은 값을 구하도록 less than을 선택한다.
- Ascending, Desending : 높은 점수가 맨 위에 출력되도록 내림차순(Desending)으로 설정한다.

설정이 완료되었으면 Query 버튼을 클릭힌다. Week가 2014-05-09, 2014-05-15인 아이템들이 TopScore가 높은  
순으로 정렬되어 출력된다. 이렇게 주간 전체순위를 산출할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/138682646-7f48b537-4c4a-4e61-b00b-04217ce7017e.png)  
![image](https://user-images.githubusercontent.com/33191974/138682685-b906a3ce-35ec-4677-b8ee-cfafa859af2b.png)  

>#### 범위키(Range Key) 검색 조건
>- equal to : 속성의 값과 같을 때
>- less than : 속성의 값보다 작을 때
>- less than or equal to : 속성의 값보다 작거나 같을 때
>- greater than : 속성의 값보다 클 때
>- greater than or equal to : 속성의 값보다 크거나 같을 때
>- between : 속성의 값에 포함될 때
>- begins with : 속성의 값이 ~로 시작될 때

이번에는 로컬 보조 인덱스를 사용하여 개인 최고기록을 조회해보자.  
UsersLeaderboard 테이블에서 개인 최고기록을 쿼리한다. 
- Scan, Query : Scan은 테이블의 모든 아이템을 출력하는 것이고, Query는 지정한 조건에 맞은 내용을   
  출력하는 것이다. Query를 선택한다.
- Index Name : 검색할 인덱스이다. [Local] TopScore-Index: Id, TopScore를 선택한다.
- Hash Key : 검색할 해시키이다. 인덱스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.
   - Id equal to : John(1) 유저의 점수를 조회할 것이므로 1을 입력한다.
- Range Key : 검색할 범위키이다. 인덱스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.  
   - TopScore : 최대점수를 99999라보고, 99999를 입력한다.
   - less than : 범위키는 검색조건을 정할 수 있다. 지정한 값보다 작은 값을 구하도록 less than을 선택한다.
- Ascending, Desending : 높은 점수가 맨 위에 출력되도록 내림차순(Desending)으로 설정한다.  
- All, Projected : All을 선택하면 아이템의 모든 속성이 출력되고, Projected를 선택하면 보조 인덱스를  
  생성할 때 Projected Attributes에 설정한 속성들이 출력된다. 기본값 그대로 사용한다.     
![image](https://user-images.githubusercontent.com/33191974/138684775-53d719a9-2db2-49ec-8200-df5815b1d2b0.png)
![image](https://user-images.githubusercontent.com/33191974/138685073-cc0c9d58-07e3-4a74-a6ee-6026a2e30f57.png)  
AWS 콘솔에서는 쿼리 출력 개수를 설정할 수 없지만 프로그래밍 언어에서 API로 DynamoDB에 쿼리하는 경우  
출력 개수를 설정할 수 있다. 쿼리 출력 개수를 1개로 설정하면 개인 최고기록이 되는 것이다.  
  
FriendsLeaderboards에서 주간 친구 순위를 산출해보자.  
FriendLeaderboard 테이블에서 주간 친구 순위를 쿼리한다.  
- Scan, Query : Scan은 테이블의 모든 아이템을 출력하는 것이고, Query는 지정한 조건에 맞은 내용을 출력하는  
  것이다. Query를 선택한다.
- Index Name : 검색할 인덱스이다. [Global] FriendIdAndWeek-Score-Index : FriendIdAndWeek, Score를 선택한다.
- Hash Key : 검색할 해시키이다. 인덱스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.  
   - FriendIdAndWeek equal to : 3번 유저의 2014-05-09,2014-05-15 주간의 친구 순위를 산출할 것이므로  
     3,2014-05-09,2014-05-15를 입력한다.
- Range key : 검색할 범위키이다. 인덱스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.
   - TopScore : 최대점수를 99999라 보고, 99999를 입력한다.
   - less than : 범위키는 검색조건을 정할 수 있다. 지정한 값보다 작은 값을 구하도록 less than을 선택한다.
- Ascending, Desending : 높은 점수가 맨 위에 출력되도록 내림차순(Desending)으로 설정한다.          
![image](https://user-images.githubusercontent.com/33191974/138685884-a1f7e365-ccf8-48ad-88f4-ad6e915f6e7a.png)  
![image](https://user-images.githubusercontent.com/33191974/138686640-f422abf7-bfaf-43e5-8ac6-61614cd27f67.png)


  
  
 
























