웹 서버에서 정적인 HTML, JavaScript 등의 파일을 배포하기 위해 CloudFront  
배포를 생성한다. EC2 인스턴스의 부하를 줄이려면 CloudFront는 필수이다.  
  
웹 서버용 ELB 로드 밸런서의 CloudFront 배포를 생성한다.   
- Origin Domain Name: 이 부분을 클릭하면 현재 생성된 ELB 로드 밸런서 목록이  
표시된다.  
- 나머지는 기본값 그대로 사용한다.  
<img src="https://user-images.githubusercontent.com/33191974/159918704-80597fdc-278e-4fde-8ab9-eedd5063b112.png" width="50%" height="50%"/>  
  
도메인을 구입했다면 Alternate Domain Names(CNAMEs) 부분에 도메인을 입력한다.  
티켓 예매 사이트의 웹 서버 도메인이므로 최상위 도메인(예) slkjfd.cf)와 www  
서브 도메인(예) www.slkjfd.cf)를 입력한다.  
<img src="https://user-images.githubusercontent.com/33191974/159919308-a7d0278e-c69f-4703-bb92-0d0b7300b91d.png" width="50%" height="50%"/>  
  
웹 서버용 CloudFront 배포를 생성한 모습이다.   
<img src="https://user-images.githubusercontent.com/33191974/159919622-c5313043-e6ac-46a9-a866-1761b7e95851.png" width="50%" height="50%"/>   






















