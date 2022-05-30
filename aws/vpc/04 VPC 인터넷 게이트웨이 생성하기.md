VPC에 속한 서브넷에서 외부 인터넷에 접속하려면 인터넷 게이트웨이가 필요하다. VPC 인터넷 게이트웨이   
목록(Virtual Private Cloud -> Internet Gateways)에서 위쪽 Create Internet Gateway 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158050488-33ac45da-2ba9-4a25-ad46-060d719c9864.png" width="50%" height="50%"/>     
VPC 인터넷 게이트웨이를 생성한다.   
- Name tag: 인터넷 게이트웨이의 이름이다. ExampleInternetGateway를 입력한다(입력하지 않아도 상관없다).  

설정이 완료되었으면 Yes, Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158050574-53532827-cff2-4396-a203-01855b6cfa3b.png" width="50%" height="50%"/>   
  
VPC 인터넷 게이트웨이 목록에 인터넷 게이트웨이(ExampleInternetGateway)가 생성되었다. 인터넷 게이트웨이  
를 선택하고 위쪽 Attach to VPC 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158050629-c72603fe-2c0b-403f-a4d4-f0ae8df8b247.png" width="50%" height="50%"/>   
VPC 부분에서 생성한 VPC(ExampleVPC)를 선택하고, Yes, Attach 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158050666-4a704f02-90a7-4be1-905c-3d209a6f9041.png" width="50%" height="50%"/>   
VPC에 인터넷 게이트웨이(ExampleInternetGateway) 연결이 완료되었다.   
<img src="https://user-images.githubusercontent.com/33191974/158050698-83af4d3f-0b16-49bc-8033-abd5a77c61a5.png" width="50%" height="50%"/>    
이제 VPC(ExampleVPC)에서 생성한 EC2 인스턴스는 외부 인터넷에 접속할 수 있다.   































