## DynamoDB 테이블에 데이터 추가하기
앞서 UsersLeaderboard 테이블과 FriendsLeaderboard 테이블을 생성해보았다.  
이제 쿼리 테스트를 위해 두 테이블에 데이터를 추가해보자.  
DynamoDB 테이블 목록에서 UsersLeaderboard 테이블을 선택하고 위쪽 Explore Table 버튼을 클릭한다.   
UsersLeaderboard 테이블 안에 들어있는 아이템을 볼 수 있다.   
현재는 빈 테이블이므로 데이터가 표시되지 않는다. New Item 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138663564-650cf1b3-3362-40ac-84bb-38c5d5eee21d.png)  
UsersLeaderboard 테이블에 아이템을 추가한다.  
추가할 데이터의 내용은 아래에 표로 나와 있다. 
- Id : 숫자 형식이며 유저를 구별하는 고유값이다.
- Week : 문자열 형식이며 한 주를 표현한다.
- TopScore : 숫자 형식이며 해당 주 동안 유저가 도달한 최고 점수이다.
- Name : 문자열 형식이며 유저의 이름이다.

데이터를 입력하고 아래 Put Item 버튼을 클릭하면 아이템이 추가된다. 
![image](https://user-images.githubusercontent.com/33191974/138664315-5f3e5158-be08-47b4-884c-4a46a2090040.png)  
아이템 추가가 성공하면 아래 그림처럼 창이 나온다. OK 버튼을 클릭하면 계속 아이템을 추가할 수 있다.
아래 표에 나와있는 대로 유저 3명의 데이터를 추가한다. 
![image](https://user-images.githubusercontent.com/33191974/138664516-47ed1abb-6b04-4cb9-89b3-fe9e8b841136.png)    
방금 입력한 데이터가 출력된다.  
![image](https://user-images.githubusercontent.com/33191974/138664952-eee58832-50c8-42e7-a4a6-cbb0d300b7c0.png)  
FriendsLeaderboard 테이블에 아이템을 추가한다. 
![image](https://user-images.githubusercontent.com/33191974/138665416-ed8c7c6f-64ef-49d0-8232-0de2149d1bfa.png)  
아래 표에 나와있는 데로 데이터를 추가한다.  
서로 친구인 상황은 다음과 같다.  
- John(1)은 Maria(2)와 친구
- Maria(2)는 John(1)과 Andrew(3)가 친구
- Andrew(3)은 Maria(2)와 친구  

![image](https://user-images.githubusercontent.com/33191974/138665855-6e207eec-5ed7-49fc-822c-bbedec5b66e1.png)    
아래와 같이 아이템 생성이 완료되었다.   
![image](https://user-images.githubusercontent.com/33191974/138666621-3b7957f0-f57b-4829-8907-05d3a1abe7a2.png)













