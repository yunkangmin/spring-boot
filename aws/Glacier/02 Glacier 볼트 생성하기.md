이제 Glacier 볼트를 생성해보자. AWS 콘솔로 접속한 뒤 메인 화면에서 Storage & Content Delivery의   
Glacier를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158052780-302a1e34-da88-46a5-85c5-b9e8777fc960.png" width="50%" height="50%"/>    
서울리전을 사용한다.   
생성한 Glacier 볼트가 하나도 없을 때 아래 그림과 같은 페이지가 표시된다. Create Vault 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158052820-daacc12c-a65b-4268-a2e7-812406f38900.png" width="50%" height="50%"/>    
Glacier 볼트를 생성한다.  
- Region: AWS 콘솔에서 선택한 리전이 표시된다.   
- Vault Name: 볼트 이름이다. 여기서는 ExampleVault를 입력한다.   

설정이 완료되었으면 Continue To Notifications 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158052880-2018f0c9-7222-4eac-abcc-5bb9d2a9cf9a.png" width="50%" height="50%"/>   
반출 작업을 완료했을 때 알림을 받도록 설정한다. Glacier는 아카이브를 다운로드하기 위한 반출 작업이  
3 ~ 5시간 걸리기 때문에 알림 기능이 매우 유용하다. Enable notifications and create ad new SNS topic을  
선택한다.  
<img src="https://user-images.githubusercontent.com/33191974/158053352-12da727b-d0b2-4705-9606-b7b34c96ccaa.png" width="50%" height="50%"/>   
- Topic Name: 여기에 입력한 대로 SNS 토픽을 생성한다. ExampleGlacier를 입력한다.   
- Display Name: SMS 문자 메시지에 사용할 이름을 설정한다. 우리나라에서는 아직 사용할 수 없으므로   
비워둔다.   
- Select the job type(s) you want to trigger your notifications: 반출 작업의 종류에 따라 알림을   
보내도록 설정한다.   
   - Archive Retrieval Job Complete: 아카이브 반출 작업이 완료되었을 때 알림을 보낸다.   
   - Vault Inventory Retrieval Job Complete: 볼트 인벤토리 갱신 작업(아카이브 목록을 가져오는 작업)이  
   완료되었을 때 알림을 보낸다.   
설정이 완료되었으면 Create Vault 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158053421-6e81b3ac-37f4-413e-8b63-1ed56ce383ba.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158053468-56c83814-2450-4d7d-ae7d-aa14b1ab6eca.png" width="50%" height="50%"/>    
이제 알림(Notification) 메일을 받기 위해 SNS(Simple Notification Service)에 가서 ExampleGlacier 토픽에   
이메일 구독(Subscription)을 생성하고 이메일을 인증한다. 이메일 구독 생성과 인증하는 방법은 'SNS 토픽과   
이메일 구독 생성하기'를 참조하기 바란다.   




























