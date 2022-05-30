# 정리  
1. 토픽 생성  
2. 토픽 구독하면서 프로토콜 및 엔드포인트 설정  
(엔드포인트는 알림을 받을 대상)    
  
- TTL(Time To Live)  
컴퓨터나 네트워크에서 데이터의 유효기간을 나타내기 위한 방법이다.   
  
---

가장 간단한 방식인 토픽과 이메일 구독을 생성해보자. AWS 콘솔로 접속한 뒤   
메인화면에서 Mobile Service의 SNS를 클릭한다.   
  
오른쪽 위에서 SNS의 리전을 변경할 수 있다. 서울리전을 사용한다.  
<img src="https://user-images.githubusercontent.com/33191974/158771837-77c91d63-ebc6-4dcd-bf51-0a73b0064905.png" width="50%" height="50%"/>    
  
SNS 메인 페이지에서 Create New Topic 버튼을 클릭한다.   
SNS 토픽을 생성한다.   
- Topic Name: 토픽 이름이다. ExampleTopic을 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158772522-9d27d3f6-0bb4-4910-86ab-6b86de7b8876.png" width="50%" height="50%"/>   
  
- Display Name: SMS 문자 메시지에 사용할 이름이다. 우리나라에서는 아직   
사용할 수 없으므로 비워둔다.   
  
설정이 완료되었으면 Create Topic 버튼을 클릭한다.   
  
SNS 토픽 목록에 방금 생성한 SNS 토픽(ExampleTopic)이 추가되었다.   
Create Subscription 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158773292-6520563f-4565-4775-9e6f-e4d8313a1787.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158784687-0623b438-6c16-47e6-9405-9cc4be14a230.png" width="50%" height="50%"/>  

> #### 다른 AWS 리소스에서 생성한 SNS 토픽   
> 사용자 편의상 다른 AWS 리소스에서도 SNS 토픽을 생성할 수 있다. 'Cloud  
> Watch 알람 생성하기'와 'Glacier 볼트 생성하기'를 해봤다면 SNS 토픽이   
> 생성되어 있을 것이다.  

SNS 구독을 생성한다.  
- Topic Name: 현재 SNS 토픽의 이름이 표시된다.  
- Protocol: 푸시 알림 메시지를 보내는 방식이다. Email을 선택한다.   
   - HTTP, HTTPS: 푸시 알림을 보내면 설정한 HTTP, HTTPS URL로 접속한다.   
   - Email, Email-JSON: 푸시 알림을 보내면 설정한 이메일 주소로 메일을   
   보낸다. Email은 사람이 알아보기 쉬운 형태이고, Email-JSON은 프로그램이  
   파싱할 수 있도록 JSON 형태로 보낸다.  
   - Amazon SQS: 푸시 알림을 보내면 설정한 SQS 큐에 메시지를 보낸다.   
   - Application: 푸시 알림을 보내면 설정한 애플리케이션(APNS, GCM, ADM)의   
   엔드포인트로 푸시 알림을 보낸다.  
- Endpoint: 각 프로토콜의 엔드포인트이다.  

설정이 완료되었으면 Subscribe 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158786846-52aa8567-f2e2-483c-8bd0-b1a1a00800b6.png" width="50%" height="50%"/>  
SNS 구독의 엔드포인트를 인증한다. 이메일은 스팸 메일을 방지하기 위해 자신의   
이메일이 맞는지 인증 메일을 보낸다. Close 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158787538-7a8dd2b1-1577-4bfe-9605-a0458634f4bb.png" width="50%" height="50%"/>   
  
이메일 확인이 완료되었다는 페이지가 표시된다. 이제 SNS가 알림을 받을 수 있게  
되었다.  
<img src="https://user-images.githubusercontent.com/33191974/158787717-bfb063b6-640e-4edb-b6a0-1a4ab062ce65.png" width="50%" height="50%"/>  
  
SNS 구독 목록에서 위쪽 Refresh 버튼을 클릭하면 목록이 갱신된다. 방금 이메일  
인증을 했으므로 구독 ID가 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/158788794-745d510e-29b9-477a-a4d2-b8b6b250a78e.png" width="50%" height="50%"/>   







  




  























