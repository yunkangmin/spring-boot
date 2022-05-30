EC2 인스턴스에서 액세스 키와 시크릿 키를 사용하지 않고 S3와 SQS에 접근할 수  
있도록 IAM 역할을 생성한다.  
1. IAM 역할 이름을 설정한다.  
2. Amazon EC2를 선택한다.  
3. S3 버킷에 파일을 올리고 받을 수 있어야 하기 때문에 Amazon S3 Full Access  
권한을 선택한다.   
4. IAM 역할을 생성한다.  
5. SQS에 메시지를 보내고 받을 수 있어야 하기 때문에 Attach Role Policy 버튼을   
클릭하여 Amazon SQS Full Access 권한도 추가한다.   
<img src="https://user-images.githubusercontent.com/33191974/159159686-30b09af5-71f3-4bce-85b3-1a67f61e4bd4.png" width="50%" height="50%"/>     






















