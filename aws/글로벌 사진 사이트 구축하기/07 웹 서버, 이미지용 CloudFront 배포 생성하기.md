웹 서버에서 정적인 HTML, JavaScript 등의 파일과 이미지 파일을 전 세계에 빠른   
속도로 전송할 수 있도록 CloudFront 배포(Distribution)를 생성한다.   
  
웹 서버용 ELB 로드 밸런서의 CloudFront 배포를 생성한다.   
- Origin Domain Name: 이 부분을 클릭하면 현재 생성된 ELB 로드 밸런서 목록이  
표시된다. 방금 생성한 웹 서버용 ELB 로드 밸런서(examplephoto)를 선택한다.   
- Allowed Http Methods: 이미지 파일을 올릴 때 POST 메소드를 사용할 것이므로   
GET, HEAD, PUT, POST, PATCH, DELETE, OPTIONS를 선택한다.   
- 나머지는 기본값 그대로 사용한다.   
<img src="https://user-images.githubusercontent.com/33191974/159160020-e175b139-4af5-4d49-980e-cb33b245443a.png" width="50%" height="50%"/>    
  
도메인을 구입했다면 Alternate Domain Names(CNAMEs) 부분에 도메인을 입력한다.   
사진 사이트의 웹 서버 도메인이므로 최상위 도메인(예: examplephoto.com)과   
www 서브 도메인(예: www.examplephoto.com)을 입력한다.  
<img src="https://user-images.githubusercontent.com/33191974/159160041-51589c6b-f160-4c6c-8be7-5f1d9bcb051c.png" width="50%" height="50%"/>  
  
이미지 저장용 S3 버킷의 CloudFront 배포를 생성한다.  
- Origin Domain Name: 이 부분을 클릭하면 현재 생성된 S3 버킷 목록이 표시된다.   
방금 생성한 이미지 저장용 S3 버킷(`<프로젝트 이름>.image`)을 선택한다.   
- Restric Bucket Access: CloudFront에서만 S3 버킷에 접근할 수 있도록 yes를   
선택한다([참고](https://github.com/yunkangmin/spring-boot/blob/main/aws/59%20Signed%20URL%20%EC%82%AC%EC%9A%A9%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0.md#s3-%EB%B2%84%ED%82%B7-%EC%95%A1%EC%84%B8%EC%8A%A4)).    
- Origin Access Identity: 오리진 접근 식별자를 새로 생성하도록 Create a New  
Identity를 선택한다.   
- Grant Read Permissions on Bucket: CloudFront가 S3에서 파일을 읽을 수 있는   
권한을 버킷의 Bucket Policy에 설정하도록 Yes, Update Bucket Policy를 선택한다.  
- 나머지는 기본값 그대로 사용한다.   
<img src="https://user-images.githubusercontent.com/33191974/159160713-afdd67b5-5fcb-4e36-9f43-6d5426c56623.png" width="50%" height="50%"/>  
  
도메인을 구입했다면 Alternate Domain Names(CNAMEs)에 도메인을 입력한다.  
이미지 전용이므로 image 서브 도메인(예: image.examplephoto.com)을 입력한다.   
<img src="https://user-images.githubusercontent.com/33191974/159160867-9ddb749d-bea4-40b0-bd95-65b8c0ecc67e.png" width="50%" height="50%"/>  
  
웹 서버와 이미지용 CloudFront 배포를 생성한 모습이다.  
<img src="https://user-images.githubusercontent.com/33191974/159160906-f3223808-c568-49cc-9a7d-e1c4f783c2b7.png" width="50%" height="50%"/>  




























