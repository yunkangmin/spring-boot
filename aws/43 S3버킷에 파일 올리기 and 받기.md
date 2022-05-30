## S3 버킷에 파일 올리기 and 받기
S3의 가장 기본적인 기능인 파일 올리기 및 받기를 먼저 알아보자.  
S3 버킷 목록에서 방금 생성한 버킷(examplebucket12323)을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138443188-715a621c-179e-49bd-8b30-60d2cde63c9a.png)  
버킷 안의 객체 목록으로 들어왔다. 방금 버킷을 새로 만들었으므로 아무것도 들어있지 않다.  
위쪽의 Upload 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138443431-27375507-8bdf-4d95-92f1-f43c17c09739.png)   
Add Files 버튼을 클릭한다.(Drag and drop files and folders upload here 부분에 파일을 드래그 & 드롭으로  
옮겨도 된다.)  
![image](https://user-images.githubusercontent.com/33191974/138443649-1ca0500a-78bd-43c1-9971-836016850a9c.png)  
컴퓨터 안에 있는 아무 그림 파일이나 선택하고, 열기 버튼을 클릭한다.    
그림 파일이 업로드 대기 목록에 추가된 것을 확인할 수 있다.   
아래 Set Details 버튼을 클릭한다.   
파일 저장에 관한 설정이다.  
- Use Reduced Redundancy Storage : 낮은 중복 스토리지 사용 옵션이다. 이 부분은 파일을 올리고 나서도 설정이  
  가능하다. 기본값 그대로 사용한다.  
- Use Server Side Encryption : 서버에 파일을 암호화해서 저장하는 옵션이다. 이 부분은 파일을 올리고 나서도  
  설정이 가능하다. 기본값 그대로 사용한다.   
![image](https://user-images.githubusercontent.com/33191974/138444763-9aa8c420-a7dd-4694-a8be-56d70ddcb571.png)  
권한 설정이다.  
- Grant me full control : 자신의 계정에 모든 제어 권한을 부여하는 옵션이다. 기본값 그대로 사용한다.  
- Make everything control : 올리는 모든 파일을 인터넷에 공개하는 옵션이다. 기본값 그대로 사용한다. 
![image](https://user-images.githubusercontent.com/33191974/138445276-32dd53be-1b18-489f-a7c8-6297bca1bb75.png)  
메타데이터 설정이다. 
- Figure out content types automatically : 파일 확장자에 따라 HTTP Content-Type을 자동으로 설정하는 옵션이다.  
  기본값 그대로 사용한다.  
이제 Start Upload 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138445553-3a940af5-14ef-4993-bf71-3775988c6eec.png)  
아래 그림처럼 S3 객체 목록에서 그림 파일이 올라간 것을 확인할 수 있다.  
파일의 용량이 크다면 오른쪽 프로그레스바에서 진행율이 표시된다.  
![image](https://user-images.githubusercontent.com/33191974/138445700-0caff3e1-ebef-44ac-9532-bf926fe30f2c.png)   
S3 객체 목록에서 파일을 선택하고 마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다. Download를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138445913-c48cabc3-426b-4237-86e2-acc73aa543fc.png)
![image](https://user-images.githubusercontent.com/33191974/138445966-ef362b0e-1b46-4012-b2a9-511ab1013ed8.png)    
다운로드 버튼을 누르면 아래와 같이 파일을 다운받는다.
![image](https://user-images.githubusercontent.com/33191974/138446145-883c6b6a-325d-4c06-932b-1810f871d198.png)  

> #### S3 GUI 클라이언트
> S3는 다양한 GUI 클라이언트들이 있다. AWS 콘솔로 S3 버킷에 접근하는 것보다 좀 더 편리하다.  
> CloudBerry Explorer : http://www.cloudberrylab.com/free-amazon-s3-explorer-cloudfront-IAM.aspx  
> Bucket Explorer : http://www.bucketexplorer.com





  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  






