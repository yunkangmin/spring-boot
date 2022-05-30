지금까지 Linux EC2 인스턴스 위주로 사용 방법을 설명했다. 이번에는 Windows  
EC2 인스턴스에 접속하는 방법을 알아보자.    
Windows EC2 인스턴스를 생성하는 방법은 Linux EC2 인스턴스를 생성하는   
방법과 동일하다. 단 보안 그룹에 3389 포트 번호를 허용해줘야 한다(아래 접속  
도구인 mstsc가 3389 포트를 사용하여 접속을 한다).       
  
Windows는 Linux처럼 키 쌍(Key Pair)를 이용하여 SSH로 접속할 수 없다. 따라서   
Administrator 비밀번호를 얻어야 한다. Windows EC2 인스턴스를 선택하고 마우스   
오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다. Get Windows Password를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/160275324-44cbf878-2d23-4795-bd9c-7d89ccdb00de.png" width="50%" height="50%"/>    
  
Administrator 비밀번호 복호화 창이 표시된다. 필자는 Windows EC2 인스턴스를   
생성할 때 키 쌍으로 `2021-10-05 key`를 설정했다. Key Pair Path의 파일 선택   
버튼을 클릭한다.     
<img src="https://user-images.githubusercontent.com/33191974/160275455-678e61bf-ff3a-46c0-b5cf-7823c3249063.png" width="50%" height="50%"/>   
  
키 쌍을 생성할 때 다운로드한 pem 파일을 선택한다. 키 쌍 다운로드는 'EC2 인  
스턴스 생성하기, 키 쌍 생성하기'를 참조하기 바란다.   
<img src="https://user-images.githubusercontent.com/33191974/160275486-cffc76d7-ad95-4a78-bc80-26a7fba616a5.png" width="50%" height="50%"/>   

키 쌍 pem 파일이 열렸다. Decrypt Password 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/160275530-56b5dfd4-da64-49cf-abe8-b44fdd73d6d9.png" width="50%" height="50%"/>  
  
Administrator 비밀번호가 복호화되었다.   
<img src="https://user-images.githubusercontent.com/33191974/160275596-aa9dc2ac-2cf4-4f4d-acb8-d6afdce5c736.png" width="50%" height="50%"/>   
  
Windows에서 원격 데스크톱 연결(시작 -> 실행 -> mstsc)을 실행한다. 그리고   
IP 주소를 입력한 뒤 연결(N) 버튼을 클릭한다(퍼블릭 아이피를 넣어야 한다).     
<img src="https://user-images.githubusercontent.com/33191974/160275692-e88a0955-80c4-4be3-8d36-4dc35bef3572.png" width="50%" height="50%"/>   
  
사용자 자격 증명 입력에 Administrator와 방금 복호화한 비밀번호를 입력하고  
확인 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/160276231-f53e9b03-c16b-4b86-9c93-d280693f14f7.png" width="50%" height="50%"/>  
  
인증서 경고 창이 표시된다. 예(Y) 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/160276272-b7db1ce7-97fc-4967-a485-a5b5d5484151.png" width="50%" height="50%"/>   
  
Windows EC2 인스턴스에 원격 데스크톱으로 연결했다.   
<img src="https://user-images.githubusercontent.com/33191974/160276306-cb34579e-fcf7-4dad-b879-248b18670ad9.png" width="50%" height="50%"/>  

































