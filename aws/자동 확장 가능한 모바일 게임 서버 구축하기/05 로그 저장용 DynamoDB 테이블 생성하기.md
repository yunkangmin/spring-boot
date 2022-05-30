결제 로그 및 각종 게임 로그 등 비정형 데이터를 저장할 DynamoDB 테이블을 생성  
한다.   
먼저 ExampleGameLog 테이블의 데이터 구조이다.   
- action: 문자 형식이며 로그의 종류이다. 게임, 결제 등 게임 기획에 맞게 분류  
한다.  
- date: 문자열 형식이며 로그가 기록된 날짜 및 시간이다.   
- 나머지는 로그 종류에 따라 달라진다.  
  
ExampleGameLog 테이블 구조   
- 키 이름(값 형식)  
- action(String)
- date(string)
- 비정형 데이터
- 테이블 인덱스
- 해시 기본 키
- 범위 기본 키  

로그 데이터만 저장하기 때문에 테이블 구조가 매우 간단하다. 실무에서는 게임  
기획에 맞게 구조를 설계한다.  
  
DynamoDB에서 ExampleGameLog 테이블을 생성한다.   
- Table Name: ExampleGameLog을 입력한다.   
- Hash Attribute Name: String을 선택하고 action을 입력한다.   
- Range Attribute Name: String을 선택하고 date을 입력한다.  

<img src="https://user-images.githubusercontent.com/33191974/160237394-975039f6-ff47-4ecd-8351-10151c7e4ef3.png" width="50%" height="50%"/>  
보조 인덱스는 필요에 따라 각자 생성한다. 다음은 ExampleGameLog 테이블을  
생성한 모습이다.    
  
ExampleGameLog 테이블 생성 완료  
<img src="https://user-images.githubusercontent.com/33191974/160237504-1077143e-646e-4bb8-802c-4a666f314cb1.png" width="50%" height="50%"/>   
  
> #### DynamoDB 처리량 설정   
> 게임이 성공해서 사용량이 늘어났을 때는 DynamoDB 처리량도 함께 늘려준다.   
> DynamoDB 처리량이 낮으면 읽기/쓰기 동작이 실패한다. 그래서 로그 데이터가  
> 누락될 수 있으니 주의한다.  
























