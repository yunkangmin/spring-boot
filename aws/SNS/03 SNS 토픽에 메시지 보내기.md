앞에서 SNS 토픽을 생성하고 이메일 구독을 생성했다. 이제 SNS 토픽에 메시지를  
보내보자. SNS 토픽(ExampleTopic)을 선택하고 위쪽 Publish 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158790253-1b2d0e5b-9f2a-4421-9fdd-ade9cec71d58.png" width="50%" height="50%"/>      
  
SNS 토픽에 메시지를 보낸다. 이메일 구독을 여러 개 등록했다면 모든 이메일 주소에   
메일을 보내고, APNS, GCM, ADM 애플리케이션을 등록했다면 각 장치에 모두 푸시  
알림을 보낸다. 즉 프로토콜에 관계없이 등록된 모든 구독에 메세지를 보낸다.          
- Topic Name: 선택한 SNS 토픽 이름이 표시된다.   
- Subject: 메시지 제목이다. 여기서는 Hello를 입력한다.  
- Message: 메시지 내용이다. 여기서는 Hello SNS Topic을 입력한다.  
- Use name message body for all protocols: 모든 프로토콜에 동일한 메시지를   
보낸다. 기본값 그대로 사용한다.   
- Use different message body for different protocols: 프로토콜마다 다른  
메시지를 보낸다. 선택하면 자동으로 양식이 입력된다.  
  
설정이 완료되었으면 Publish Message 버튼을 입력한다.   
<img src="https://user-images.githubusercontent.com/33191974/158793414-c0700160-ee69-476b-9ab1-2709d8b5d2ba.png" width="50%" height="50%"/>  

웹 브라우저를 실행하고 SNS 구독을 생성한 이메일 주소의 메일함으로 이동한다.  
메일함을 보면 방금 보낸 Hello라는 메일이 도착했을 것이다. 이처럼 SNS 토픽에 
메시지를 보낼 수 있다(토픽에 메시지를 전달하면 구독한 엔드포인트에 메세지를  
보낸다).  
<img src="https://user-images.githubusercontent.com/33191974/158793960-d402d04f-6830-450b-b18c-9ffe77048b6c.png" width="50%" height="50%"/>  





























