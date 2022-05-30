ELB 로드 밸런서가 라운드 로빈 알고리즘으로 트래픽을 분해하는 것이 아닌 사용자의 쿠키 세션을   
기준으로 트래픽을 분배하는 Sticky Session을 사용해보자.   
  
ELB 로드 밸런서 목록(NETWORK & SECURITY -> Load Balancers)에서 ELB 로드 밸런서(exampleelb)를 선택하고,   
아래 Description 탭을 클릭한다. Port Configuration 부분에서 Stickiness: Disabled의 Edit를 클릭한다.  
  
<img src="https://user-images.githubusercontent.com/33191974/158007500-eae3a3c0-33f9-4c68-a33b-9f2bd3e2f5a3.png" width="50%" height="50%"/>    
  
Sticky Session을 설정한다. Enable Load Balancer Generated Cookie Stickiness를 선택한다.   

- Enable Load Balancer Generated Cookie Stickiness: ELB 로드 밸런서가 생성한 쿠키를 사용한다.   
   - Expiration Period: 쿠키가 유지되는 시간을 설정할 수 있다. 값을 설정하지 않으면 쿠키를 삭제하지   
   않고 계속 유지시킨다. 여기서는 300을 입력한다.   
- Enable Application Generated Cookie Stickiness: 서버의 애플리케이션이 생성한 쿠키를 사용한다.   
   - Cookie Name: 서버의 애플리케이션이 생성한 쿠키 이름을 설정한다.   
설정이 완료되었으면 Save 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158007550-668deaeb-fcb3-47eb-b7df-f115683d6ac8.png" width="50%" height="50%"/>  
ELB 로드 밸런서 Sticky Session 설정이 완료되었다.   
<img src="https://user-images.githubusercontent.com/33191974/158007624-f7cc32a2-c10e-443f-a581-be56dfb3258d.png" width="50%" height="50%"/>    
웹 브라우저를 실행하고 ELB 로드 밸런서의 URL에 접속한다. 필자는 두 번째 EC2 인스턴스에 연결되었다.   
이 URL을 계속 새로고침해도 계속 같은 EC2 인스턴스에 연결된다.    
  
Expiration Period를 300초(5분)로 설정했기 때문에 5분 뒤에 새로고침해보면 다른 EC2 인스턴스에 연결될  
수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158007680-da09b9ca-74ca-4fcd-9d91-667cae000590.png" width="50%" height="50%"/>   
구글 크롬을 사용하고 있다면 f12를 눌러 개발자 도구를 실행한다. 그리고 Network 탭을 클릭한 뒤 새로고침  
해보면 ELB 로드 밸런서가 생성한 쿠키 값을 볼 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158007730-784b93e0-e39b-4c85-ba0a-08e05ad16b24.png" width="50%" height="50%"/>   


























