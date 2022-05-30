## AWS 리소스의 상태를 모니터링하는 CloudWatch
CloudWatch는 AWS 리소스의 상태를 모니터링 하는 서비스이다.  
모니터링뿐만 아니라 측정치(Metric)와 연계하여 다양한 액션(Action)을 사용할 수 있다.

> #### 프리티어에서 사용가능
> CloudWatch는 프리티어에서 무료로 사용가능하다.  
> 2014년 8월 기준으로 기본 모니터링(5분 주기), 측정치(Metric) 10개, 알람(Alarm) 10개,  
> 매달 API 요청 1,000,000건을 무료로 사용할 수 있다.  

CloudWatch는 EC2 인스턴스가 이상이 있을 경우 알림을 받고자 할 때, 사용량이 급증했을 때  
자동으로 횡적 확장(Auto Scaling)을 하고 부하 분산(Elastic Load Balancing)을 구축할 때  
사용한다. ELB와 Auto Scaling 사용방법은 뒤에서 자세히 설명한다.  
![image](https://user-images.githubusercontent.com/33191974/138230299-6397b67b-a09b-4120-b496-4326d70551ab.png)  
CloudWatch에서는 각 AWS 리소스의 특징에 따라 다양한 값들을 모니터링할 수 있다.  
CloudWatch가 제공하는 측정목록 이외에도 사용자가 직접 생성한 커스텀 측정치(Custom Metric)도 사용할 수 있다.  
기본 모니터링 간격은 5분이며, 세부 모니터링 간격은 1분이다. 기본 모니터링은 프리 티어에서 무료로   
사용할 수 있으며, 세부 모니터링은 추가 요금을 지불해야 한다.  

- EC2 인스턴스 : CPU 사용률, 데이터 전송량, 디스크 사용량 등을 모니터링할 수 있다.  
  측정치와 연계하여 알림(Notification), 자동 횡적 확장(Auto Scaling), EC2 인스턴스 제어 액션을  
  사용할 수 있다. 
- EBS 볼륨 : 읽기/쓰기 사용량, 지연 시간등을 모니터링할 수 있다. 측정치와 연계하여 알림(Notification),    
  자동 횡적 확장(Auto Scaling) 액션을 사용할 수 있다.  
- ELB(Elastic Load Balancing) : 요청 수 및 지연 시간 등을 모니터링할 수 있다.    
  측정치와 연계하여 알림, 자동 횡적 확장 액션을 사용할 수 있다.  
- RDS(Relational Database Service) : CPU 사용률, DB 연결 수, 사용 가능한 메모리 및 스토리지 공간,  
  읽기/쓰기 지연 시간 등을 모니터링할 수 있다. 측정치와 연계하여 알림, 자동 횡적 확장 액션을 사용할 수  
  있다.
- DynamoDB : 테이블 인덱스와 글로벌, 로컬 보조 인덱스에서 소모한 읽기/쓰기 용량 유닛, 스캔, 쿼리,  
  아이템 추가 및 수정(Putitem), 아이템 삭제(Delete)를 모니터링할 수 있다.   
  측정치와 연계하여 알림, 자동 횡적 확장 액션을 사용할 수 있다.  
- SNS(Simple Notification Service): 게시(Published) 및 전송(Delivered) 메시지 수 등을 모니터링할 수 있다.  
  측정치와 연계하여 알림, 자동 횡적 확장 액션을 사용할 수 있다.  
- SQS(Simple Queue Service) : 전송(Send) 및 수신(Received)된 메시지 수 등을 모니터링할 수 있다.  
  측정치와 연계하여 알림, 자동 횡적 확장 액션을 사용할 수 있다.  

이번에는 EC2 인스턴스를 예로들어 모니터링 하는 방법과 커스텀 측정치를 사용하는 방법을 알아보자.
   
  
