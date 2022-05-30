EC2 인스턴스에서 액세스 키와 시크릿 키를 사용하지 않고 S3 DynamoDB에 접근할  
수 있도록 IAM 역할을 생성한다.  

1. IAM 역할 이름을 설정한다.   
2. Amazon EC2를 선택한다.  
3. S3 버킷에서 파일을 받을 수 있어야 하기 때문에 Amazon S3 Full Access 권한을   
선택한다.   
4. IAM 역할을 생성한다.   
5. DynamoDB에 데이터를 읽고 쓸 수 있어야 하기 때문에 Attach Role Policy 버튼  
을 클릭하여 DynamoDB Full Access 권한을 추가한다.   
  
<img src="https://user-images.githubusercontent.com/33191974/160237734-697dadf4-e568-42e7-bf08-99fa04c25f31.png" width="50%" height="50%"/>  



















