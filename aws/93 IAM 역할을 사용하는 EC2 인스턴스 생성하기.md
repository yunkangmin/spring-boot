EC2 인스턴스 전용 IAM 역할은 EC2 인스턴스를 생성할 때 설정해줘야 한다. 이미   
만들어진 EC2 인스턴스에는 IAM 역할을 설정할 수 없다. EC2 인스턴스 페이지로   
이동한 뒤 EC2 인스턴스 목록(INSTANCES -> Instances)에서 위쪽 Launch Instance   
버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/157698130-3f5a769a-9877-40f7-9a0f-aba4e1cd5a35.png" width="50%" height="50%"/>  
EC2 가상 서버에 설치될 운영체제를 선택한다. AWS 명령행 인터페이스(AWS CLI)가  
미리 설치되어 있는 Amazon Linux를 사용한다. Free tier only에 체크하고,   
Amazon Linux AMI의 Select 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157698854-296781ef-db39-42e5-8ee9-41c5ef432254.png" width="50%" height="50%"/>   
EC2 인스턴스 유형을 선택한다. 기본적으로 Free tier용인 Micro Instance가 선택되어    
있다. Next: Configure Instance Details 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/157699315-b3facb7b-2253-46b2-bd71-1453ea890438.png" width="50%" height="50%"/>    
EC2 인스턴스 세부 설정에서 IAM 역할을 설정할 수 있다. IAM role 부분에서  
IAM 역할 목록이 표시된다. 앞에서 생성한 ExampleEC2Role을 선택한다. 나머지   
설정은 기본값 그대로 사용하고 Review and Launch 버튼을 클릭한다(나머지 세부   
설정은 생략한다. 자세한 내용은 'EC2 인스턴스 생성하기'를 참고하기 바란다).  
<img src="https://user-images.githubusercontent.com/33191974/157700919-fb6860d4-3341-4390-8fe3-108adb272b5d.png" width="50%" height="50%"/>     
지금까지 설정한 값들이 정상적으로 설정되었는지 확인한다. 문제가 없다면 Launch  
버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157701591-b725edec-670c-40df-8da6-d737fc1e1af2.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/157701847-080e8b4e-396c-4e90-9b9d-5d861459c3c2.png" width="50%" height="50%"/>    
앞에서 EC2 인스턴스를 생성해봤다면 EC2 인스턴스 접속을 위한 키가 이미 있을 것이다.  
awskeypair를 선택한 뒤 | acknowledge that I have...를 체크하고 Launch  
Instances 버튼을 클릭한다(EC2 인스턴스 접속을 위한 키가 없다면 'EC2 인스턴스 
생성하기' 또는 '7장 EC2 인스턴스 접속을 위한 키 쌍'을 참조하여 키를 생성하기   ㅂ
바란다).  
<img src="https://user-images.githubusercontent.com/33191974/157702711-b9ab84ff-d295-4a79-83e6-23fb06273d81.png" width="50%" height="50%"/>   
잠시 기다리면 EC2 인스턴스가 생성되었다는 화면이 나온다. View Instances 버튼을   
클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/157703356-266d5ea3-620e-449d-8496-514ff808a84b.png" width="50%" height="50%"/>     
EC2 인스턴스가 완전히 생성되었다. 방금 생성한 EC2 인스턴스를 선택하고 아래   
세부 내용에서 Public IP를 확인한다.   
<img src="https://user-images.githubusercontent.com/33191974/157703990-7981c9b0-3e6f-4455-8826-5428c8c5c53c.png" width="50%" height="50%"/>   
SSH로 위에서 확인한 Public IP에 접속한 뒤 다음 명령을 입력한다(SSH 접속 방법은  
'EC2 인스턴스에 접속하기'를 참조하기 바란다). aws s3 ls는 S3의 파일 목록을  
보는 명령이다. 앞에서 생성한 S3 버킷(examplebucket10)의 파일 목록을 출력해보자  
(S3 버킷을 생성하지 않았다면 'S3 버킷 생성하기`, `S3 버킷에 파일 올리기/받기`를  
참조하여 S3 버킷을 생성하고 파일을 올리기 바란다).  
```
[ec2-user@ip-172-31-5-32 ~]$ aws s3 ls s3://cloudfront-test-02
2022-03-04 03:35:43       9571 test.jpg
```
앞에서 IAM 역할에 S3에 접근할 수 있는 Amazon S3 Full Access를 설정했기 때문에   
S3 버킷의 파일 목록을 볼 수 있다. 또한, EC2 인스턴스를 생성할 때 IAM 역할을  
사용하도록 설정했기 때문에 액세스 키와 시크릿 키를 따로 설정하지 않아도  
aws s3 명령이 잘 동작한다.   
  
> #### IAM 역할 삭제  
> EC2 인스턴스에서 IAM 역할을 사용하고 있을 때 IAM 역할을 삭제하면 안된다.  
> IAM 역할을 삭제하는 즉시 EC2 인스턴스에서 AWS 리소스로 접근하는 권한이   
> 사라지니 주의하기 바란다.   

다음 명령을 입력하여 DynamoDB에 접근한다. IAM 역할(ExampleEC2Role)에는  
DynamoDB에 접근할 수 있는 권하이 없기 때문에 Access Denied 에러가 발생한다.   
```
[ec2-user@ip-172-31-5-32 ~]$ aws --region ap-northeast-2 
dynamodb scan --table-name UserLeaderboard
An error occurred (AccessDeniedException) when calling
the Scan operation: User: arn:aws:sts::916803271848:assumed-role
/ExampleEC2Role/i-05fdad24172fc37f3 is not authorized to perform:
dynamodb:Scan on resource: arn:aws:dynamodb:ap-northeast-2:916803271
848:table/UserLeaderboard
```









































