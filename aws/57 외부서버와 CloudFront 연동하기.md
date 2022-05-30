AWS가 아닌 외부의 웹 서버를 CloudFront와 연동하는 방법을 알아보자.   
<img src="https://user-images.githubusercontent.com/33191974/156503226-dbc76415-533a-4f24-bd7b-5517799d4037.png" width="50%" height="50%"/>    
예제로 사용할 웹 서버는 따로 준비하지 않고, 인터넷 상에서 흔히 볼 수 있는 사이트를   
사용한다. 부트스트랩은 트위터에서 개발한 유명한 웹 프론트엔드 프레임워크이다.   
이 http://getbootstrap.com을 오리진으로 하여 CloudFront와 연동해보자.  
<img src="https://user-images.githubusercontent.com/33191974/156503445-4ee4e230-d78e-45e2-80f6-26207a8b0f64.png" width="50%" height="50%"/>   
이제 CloudFront 서비스로 이동하여 배포 생성 버튼을 클릭한다.   
Origin Domain Name에 getbootstrap.com을 입력한다. 배포생성버튼을 클릭한다.   
배포가 모든 에지 로케이션에 전파되기까지 약 5분미만으로 소요된다.   
모든 에지 로케이션에 전파되면 Status가 활성화됨으로 바뀐다.  
Domain Name을 브라우저 주소창에 입력해보면 https://getbootstrap.com의 내용이   
그대로 표시된다.     
<img src="https://user-images.githubusercontent.com/33191974/156506215-abb6deda-dff7-4f5a-b10a-b83de9f57add.png" width="50%" height="50%"/>    





  
  



  





























