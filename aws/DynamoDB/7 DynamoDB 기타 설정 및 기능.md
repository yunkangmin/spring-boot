## DynamoDB 기타 설정 및 기능
DynamoDB는 테이블 생성 때 보았던 설정들을 따로 설정할 수 있다.  
이 설정들은 간략한 설명으로 대신한다.  
- Modify Throughput : 처리량(Throughput)을 변경한다. 테이블과 보조 인덱스의 읽기/쓰기 용량  
  유닛을 설정할 수 있다.
   - Use Basic Alarms : 기본 알람 사용 옵션이다. 60분 동안 전체 처리량에서 어느 정도 비율을  
     넘어서면 알람을 발생시킬 수 있다.
- Delete Table : 테이블을 삭제할 수 있다. 그리고 연재된 CloudWatch 알람도 함께 삭제할 수 있다.
- Export/Import : Data Pipline과 EMR(Elastic Map Reduce)을 이용하여 DynamoDB의 내용을 S3에 Export/  
  Import할 수 있다.
- Access Control : 테이블의 아이템 또는 속성에 높은 수준의 접근제어를 설정할 수 있다.
- Purchase Reserved Capacity : EC2와 RDS의 Reserved Instance처럼 일정한 예약금을 선불로 내면  
  읽기/쓰기 용량 유닛을 1년 또는 3년 동안 예약할 수 있으며 시간당 요금이 대폭 할인된다.
  
  
    
