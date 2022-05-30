SNS의 기본 개념에 대해 알아보자. 그리고 SNS 토픽과 이메일 구독을 생성하는  
방법, 구글 안드로이드용 예제 애플리케이션을 생성하고 푸시 알림을 받는 방법,  
애플 IOS용 예제 애플리케이션을 생성하고 푸시 알림을 받는 방법에 대해 설명한다.  
  
SNS(Simple Notification Service)는 iPhone, iPad, Android, Kindle Fire와 같은   
모바일 장치에 푸시 알림을 보낼 수 있는 서비스이다. 또한, 이메일과 SMS 문자   
메시지, SQS 큐 메시지도 보낼 수 있다.   
  
> #### 프리티어에서 사용 가능   
> SNS는 프리티어에서 무료로 사용할 수 있다. 2014년 8월 기준으로 매달 푸시   
> 알림 요청 1,000,000건, HTTP 알림 100,000건, 이메일 전송 1,000건, SMS 전송  
> 100건을 무료로 사용할 수 있다. 단, 데이터 전송 요금은 별도이다.   

모바일 장치에 푸시 알림을 보내려면 서버를 구축하고 APNS(Apple Push Notification  
Service), GCM(Google Cloud Messaging), ADM(Amazon Device Messaging)에 각각   
전송 요청을 해야 한다. SNS를 사용하면 푸시 알림 요청 한 번으로 동시에 Apple  
IOS, Google Android, Amazon Kindle Fire로 푸시 알림을 보낼 수 있다. 따라서   
APNS, GCM, ADM 전송 코드를 일일이 구현하지 않아도 된다. 또한, HTTP 접속,  
이메일 전송, SQS 큐 메시지 전송 기능도 있다.   
  
SNS를 사용하면 메세지 전송 구현에 드는 노력과 시간을 아낄 수 있고, 서버  
구축과 운영 비용을 절감할 수 있다. 그리고 SNS는 높은 가용성을 제공하므로   
서비스 중지에 대해 신경 쓸 필요가 없다.    
<img src="https://user-images.githubusercontent.com/33191974/158768012-144eeeb5-74a3-47ac-b718-3cc403536611.png" width="50%" height="50%"/>    
SNS는 사용자가 구현한 서버 애플리케이션(EC2 인스턴스)에서 모바일 장치로  
푸시 알림을 보낼 수 있다. 또한, AWS의 S3, Glacier DynamoDB, RDS, ElastiCa  
che, CloudWatch, CloudFormation의 다양한 이벤트도 SNS를 통해 전달할 수 있다.  
예를 들면 CloudWatch로 EC2 인스턴스를 감시하다가 장애가 발생하면 이메일 또는  
아이폰, 안드로이드폰의 푸시 알림으로도 받아볼 수 있다.   
  
다음은 SNS 기본 개념이다.   
- 애플리케이션(Application): APNS, APNS_SANDBOX, GCM, ADM 별로 생성하는   
설정이다. GCM은 API 키, APNS는 인증서, 개인 키, ADM은 클라이언트 ID,   
Secret이 필요하다.   
   - 엔드포인트(Endpoint): 애플리케이션에 속하며 푸시 알림을 받을 모바일  
   장치의 정보이다. Device Token, Registration ID 등을 설정해야 하며 각  
   장치마다 다른 값을 가지고 있다. 푸시 알림을 보내는 최소 단위이다.   
- 프로토콜(Protocol): 푸시 알림 메시지를 보내는 방식이다. HTTP, HTTPS, Email,   
Email-JSON, SQS, Application(APNS, GCM, ADM 등)이 있다.  
  - 엔드포인트: HTTP와 HTTPS는 URL, Email과 Email-JSON은 이메일 주소, SQS는   
  SQS 큐이름이다. 푸시 알림을 보내는 최소 단위이다.   
- 토픽(Topic): 여러 개의 엔드포인트를 그룹으로 만든 것이다. 토픽을 통해   
푸시 알림 요청을 하면 토픽에 속한 모든 엔드포인트로 알림을 보낸다.   
- 구독(Subscription): 토픽에 속한 엔드포인트이다.   
- 요금: 알림 전송 개수, 데이터 전송량에 따라 요금이 책정된다. 자세한 요금은   
AWS 사이트의 요금표(http://aws.amazon.com/ko/sns/pricing)를 참조하자.   
  
SNS의 애플리케이션, 토픽, 구독은 리전별로 생성해야 한다.   

SNS 토픽, 구독, 엔드포인트 간의 관계   
<img src="https://user-images.githubusercontent.com/33191974/158770377-a09c6d8e-d771-4cba-9e12-a7137e28bd41.png" width="50%" height="50%"/>    
  
> #### SMS 문자 메시지   
> 2014년 8월 기준으로 SMS 문자 메시지는 미국 버지니아 북부 리전(us-east-1)  
> 에서 생성한 토픽에서만 보낼 수 있고, 미국 핸드폰 번호만 지원한다. 다른  
> 리전에서 버지니아 북부 리전에 생성한 토픽에 SMS 메시지 전송 요청을 할 수 있다.  




















