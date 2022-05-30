SQS 처리 실패 큐를 생성한 뒤 앞에서 생성한 SQS 큐(ExampleQueue)와 연결해보자.  
  
SQS 처리 실패 큐를 생성한다.  
- Queue Name: 처리 실패 큐의 이름이다. ExampleDLQ를 입력한다.  
나머지 설정은 기본값 그대로 사용한다.  
  
설정이 완료되었으면 Create Queue 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158939313-8f70e759-b108-44b2-9c19-258aa9e129ee.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/158939348-9a03f6a7-0980-4202-b74b-e1370f02aa0a.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158939380-8fe91d29-9bdf-4ef2-a73f-ef974a353943.png" width="50%" height="50%"/>  
SQS 큐 목록에 처리 실패 큐로 사용할 SQS 큐(ExampleDLQ)가 생성되었다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158939517-b3e35426-32d1-4cd2-b1eb-b1319f9c397f.png" width="50%" height="50%"/>  
SQS 큐 목록에서 처리 실패 큐와 연결할 SQS 큐(ExampleQueue)를 선택하고,   
위쪽 Queue Actions 버튼을 클릭하면 팝업 메뉴가 나온다. Configure Queue를  
클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/158939766-e0ef4e91-222b-4033-88ad-8eb54e613aad.png" width="50%" height="50%"/>  
SQS 처리 실패 큐를 설정한다.  
- Use Redrive Policy: 처리 실패 큐 사용 옵션이다. 처리 실패 큐를 사용할   
것이므로 이 항목을 체크한다.  
- Dead Letter Queue: 메시지를 보낼 처리 실패 큐의 이름이다. 여기서는 방금   
처리 실패 큐로 사용하기로 한 ExampleDLQ를 설정한다.   
- Maximum Receives: 설정한 숫자보다 초과해서 메시지를 받으면 처리 실패 큐로   
보낸다. 여기서는 메시지를 3번 받으면 처리 실패 큐로 보내도록 2를 입력한다.  
  
설정이 완료되었으면 Save Changes를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158939990-cd2c0982-46b0-4253-80ac-caf33b11ee9f.png" width="50%" height="50%"/>   
   
SQS 큐 목록에서 SQS 큐(ExampleQueue)를 선택한 뒤 아래 Redrive Policy 탭을   
클릭한다. 방금 생성한 SQS 큐(ExampleDLQ)가 처리 실패 큐로 설정되었다. 
<img src="https://user-images.githubusercontent.com/33191974/158940316-d5f07989-6088-41e4-800f-386df7f50721.png" width="50%" height="50%"/>   






  























