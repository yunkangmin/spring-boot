# 정리
<img src="https://user-images.githubusercontent.com/33191974/158942743-97428cae-1ad3-43f2-80ff-cc3720234ece.png" width="50%" height="50%"/>  
  
---

SQS 큐에 메시지를 보내고 받는 방법을 알아보자. SQS 큐 목록에서 메시지를   
보낼 SQS 큐(ExampleQueue)를 선택한 뒤 위쪽 Queue Actions 버튼을 클릭하면   
팝업 메뉴가 나온다.   
<img src="https://user-images.githubusercontent.com/33191974/158940569-5e1de1db-3922-4e71-b358-21c29ffb4831.png" width="50%" height="50%"/>  
  
SQS 큐에 메시지를 보낸다.  
- MessageBody: 메시지의 내용이다. 여기서는 Hello SQS를 입력한다.   
- Message Attributes: 메시지의 문자열, 숫자, 바이너리 형식의 추가 속성이다.   
여기서는 속성을 따로 추가하지 않는다.  
- Delay delivery of this message by 0 seconds: 지연 전송 시간 사용 옵션이다.  
지연 전송은 사용하지 않을 것이므로 체크하지 않는다.  
  
설정이 완료되었으면 Send Message 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158940866-0fd5f5d7-0810-4024-b597-ddd650d5abea.png" width="50%" height="50%"/>  
  
SQS 큐에 메시지를 보낸다.  
<img src="https://user-images.githubusercontent.com/33191974/158941062-93596921-7588-4a80-b1bc-f3de3865fb44.png" width="50%" height="50%"/>   
  
방금 보낸 메시지를 꺼내서 내용을 보자. SQS 목록에서 메시지를 받을 SQS 큐  
(ExampleQueue)를 선택하고 메시지 보내기/받기 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158941289-51e758db-1805-44e5-aadd-5d0d52522ee5.png" width="50%" height="50%"/>   
  
SQS 메시지를 받기 위해 폴링을 시작한다는 창이 표시된다. Start Polling for  
Messages 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158941398-fc090a3d-fe19-4565-9eed-600aa1637d20.png" width="50%" height="50%"/>  
  
SQS 메시지 목록에서 방금 보낸 메시지가 표시된다. 이 목록에서는 기본적으로   
메시지 10개, 30초 동안 폴링하여 메시지를 받는다. 지금처럼 메시지가 목록에   
표시되거나 API 등으로 받으면 Receive Count가 증가한다.  
<img src="https://user-images.githubusercontent.com/33191974/158941812-3276bd0e-a032-4337-af73-8c7ed2fb74ab.png" width="50%" height="50%"/>    
  
SQS 메시지 목록 창을 닫고, 같은 방법으로 한 번 더 메시지를 받아본다.   
Hello SQS 메시지의 Receive Count가 3으로 증가했을 것이다.   
  
SQS 목록에서 위쪽 Refresh 버튼을 클릭하면 메시지 개수가 갱신된다.  
앞에서 SQS 큐(ExampleQueue)의 Maximum Recevies을 2로 설정했는데  
메시지를 3번 받았으므로 메시지는 처리 실패 큐(ExampleDLQ)로 이동된다.  
따라서 처리 실패 큐(ExampleDLQ)의 메시지가 1개로 표시된다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158942143-cac902db-806e-47d6-a9e9-03ce898b892a.png" width="50%" height="50%"/>   
  
AWS API를 이용하여 SQS 메시지를 보내고 받으려면 'CLI SQS'를 참조하자.  

 
 
































