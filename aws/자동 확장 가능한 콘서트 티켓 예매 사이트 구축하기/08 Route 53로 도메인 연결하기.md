앞에서 생성한 웹 서버용 CloudFront 배포를 도메인에 연결한다(도메인을 구입하지   
않았다면 이 부분은 넘어가도 된다).  
  
웹 서버용 CloudFront 배포에 최상위 도메인(예) slkjfd.cf)을 연결한다. A - IPv4    
address를 선택하고 Alias Target 부분을 클릭하면 연결할 수 있는 AWS 리소스가   
표시된다. 여기서 방금 생성한 CloudFront 배포를 선택한다.   
<img src="https://user-images.githubusercontent.com/33191974/159921203-e59e2697-fa88-4f4f-a616-db0593e671c4.png" width="50%" height="50%"/>    
  
최상위 도메인과 마찬가지로 www 서브 도메인(예) www.slkjfd.cf)도 연결한다.  
<img src="https://user-images.githubusercontent.com/33191974/159921450-33b970c8-4ad7-4463-adaa-b1e3992789d4.png" width="50%" height="50%"/>   
  
반드시 도메인을 구입한 사이트에서 Route 53 네임서버 주소를 설정해야 한다.   





























