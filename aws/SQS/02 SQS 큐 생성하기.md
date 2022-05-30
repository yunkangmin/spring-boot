이제 SQS 큐를 생성해보자. AWS 콘솔로 접속한 뒤 메인 화면에서 App Services의   
SQS를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158937166-0d0a7f31-dc94-454f-8d0b-ca28a9a33195.png" width="50%" height="50%"/>    
서울 리전을 선택한다.  
  
생성한 SQS 큐가 하나도 없을 때 아래 그림과 같은 페이지가 표시된다.   
Create New Queue 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158937297-a0f9bf46-3e69-4261-a7f6-9cc8c34588f3.png" width="50%" height="50%"/>   
  
SQS 큐를 생성한다.  
- Queue Name: SQS 큐 이름이다. ExampleQueue를 입력한다.   
- Default Visibility Timeout: 기본 보기 제한 시간이다. 기본값 그대로 사용한다.  
- Message Retention Period: 메시지 보관 기간이다. 기본값 그대로 사용한다. 
- Maximum Message Size: 메시지 최대 크기이다. 기본값 그대로 사용한다. 
- Delivery Delay: 기본 지연 전송 시간이다. 기본값 그대로 사용한다.  
- Receive Message Wait Time: 짧은 폴링 또는 긴 폴링 사용 설정이다. 0이면  
짧은 폴링, 1이상 설정하면 긴 폴링을 사용한다.  
- Use Redrive Policy: 처리 실패 큐를 사용하는 옵션이다. 여기서는 처리 실패  
큐를 설정하지 않을 것이므로 기본값 그대로 체크를 해제한다.   
- Dead Letter Queue: 메시지를 보낼 처리 실패 큐의 이름이다.  
- Maximum Receives: 설정한 숫자보다 초과해서 메시지를 받으면 처리 실패 큐로   
보낸다.  
  
설정이 완료되었으면 Create Queue 버튼을 클릭한다.   

<img src="https://user-images.githubusercontent.com/33191974/158938481-d047c67a-cb4b-40f1-b566-9d73506f0b01.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158938509-6d2faf8f-d8b1-4382-8215-69ba25a0ff4f.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158938537-d620d473-22d6-4455-8f79-5844dcd80331.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158938575-01f323dc-cf0c-4ef9-9c8c-ba97a57a71cf.png" width="50%" height="50%"/>  
(각 항목에 대한 설명은 [여기](https://github.com/yunkangmin/spring-boot/blob/main/aws/SQS/AWS%20SQS%20%EB%93%A4%EC%9D%B4%ED%8C%8C%EA%B8%B0/2%20SQS%20%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EA%B8%B0.md)를 참고하자)  
  
SQS 큐 목록에 SQS 큐(ExampleQueue)가 생성되었다. 아래 세부 내용을 보면  
SQS의 URL이 표시된다. 이 URL을 이용하여 다른 리전이나 AWS 외부에서도 SQS  
큐를 사용할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158938840-a8494255-daba-4563-ad65-08e845f21793.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158938909-1cea16b4-269a-4701-912a-1c28295538dc.png" width="50%" height="50%"/>  






























