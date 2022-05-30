RDS는 다양한 데이터베이스 엔진을 지원한다. 데이터베이스 엔진은 데이터베이스   
소프트웨어를 뜻하며 DB 인스턴스에 미리 설치되어 있다. 데이터베이스 엔진에 따라   
라이선스 모델이 다르며 사용 요금 또한, 차이가 있다.  
  
RDS에서 지원하는 데이터베이스 엔진은 아래와 같다.   
- MySQL
   - MySQL Community Edition
- PostgreSQL
- Oracle
   - Oracle Database Standard Edition One,
   - Oracle Database Standard Edition
   - Oracle Database Enterprise Edition
- Microsoft SQL Server
   - Microsoft SQL Server Express Edition
   - Microsoft SQL Server Web Edition
   - Microsoft SQL Server Standard Edition
   - Microsoft SQL Server Enterprise Edition   

RDS는 데이터베이스 엔진마다 라이선스 모델이 다르다. 그리고 같은 데이터베이스   
엔진이라도 모델의 종류에 따라 사용 요금이 달라질 수 있다.   
- MySQL: General Public License이며 추가 요금이 발생하지 않는다.  
- PostgreSQL: PostgreSQL License이며 추가 요금이 발생하지 않는다. 단 요금은  
MySQL보다 조금 높다.  
- Oracle: 두 가지 라이선스 모델이 있다.   
   - License Included: AWS에서 미리 구매한 라이선스를 사용하는 방식이다. 따라서   
   라이선스 요금이 추가로 발생한다. 사용할 수 있는 버전은 Oracle Standard   
   Edition One이다.  
   - Bring-Your-Own-License(BYOL): 이미 Oracle 라이선스를 보유하고 있을 때  
   사용 가능한 방식이다. 따라서 추가 요금이 발생하지 않는다. 사용할 수 있는  
   버전은 Oracle Standard Edition One, Oracle Standard Edition, Oracle  
   Enterprise Edition이다. 프리티어에서 매월 750시간 무료로 사용할 수 있지만  
   BYOL만 지원하기 때문에 Oracle 라이선스를 따로 구매해야 한다.   
- Micro SQL Server: 세 가지 라이선스 모델을 제공한다.   
   - 프리티어: 마이크로 인스턴스에 단일 가용 영역(Single-AZ)으로 SQL Server   
   Express Edition 버전을 실행하면 매일 750시간 무료로 사용할 수 있다.  
   - License Included: AWS에서 미리 구매한 라이선스를 사용하는 방식이다.   
   따라서 라이선스 요금이 추가로 발생한다. 사용할 수 있는 버전은 SQL Server   
   Express Edition, SQL Server Web Edition, SQL Server Standard Edition이다.   
   - Bring-Your-Own-License(BYOL): 이미 SQL Server 라이선스를 보유하고 있을 때  
   사용 가능한 방식이다. 따라서 추가 요금이 발생하지 않는다. 사용할 수 있는   
   버전은 SQL Server Standard Edition, SQL Server Enterprise Edition이다.  
   
   


























