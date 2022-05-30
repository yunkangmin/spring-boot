이제 Auto Scaling에 사용할 웹 서버 AMI를 생성한다. Auto Scaling에서 아무것도  
설치되지 않은 EC2 인스턴스를 생성한 뒤 User data 스크립트로 Node.js를 설치  
해도 된다. 하지만, 설치하는 데 시간도 걸릴 뿐더러 중간에 설치가 실패하게 되면  
웹 서버 EC2 인스턴스를 못 쓰게된다. 따라서 웹 서버와 필요한 패키지, 모듈등을  
미리 설치한 AMI를 활용하는 것을 권장한다.   
  
먼저 EC2 인스턴스를 생성한다.   
- Amazon Linux AMI를 사용한다.  
- 앞에서 생성한 S3, Auto Scaling, SQS, DynamoDB 접근용 IAM 역할(Example  
TicketRole)을 사용하도록 설정한다.   
  
생성한 EC2 인스턴스에 SSH로 접속하여 Node.js로 npm을 설치한다.   
[여기](https://github.com/yunkangmin/spring-boot/blob/main/aws/56%20CloudFront%20%EC%BB%A4%EC%8A%A4%ED%85%80%20%EC%98%A4%EB%A6%AC%EC%A7%84%20%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0.md)를 참고하여 설치한다.   

웹 서버의 소스 파일이 업데이트되면 자동으로 Node.js를 다시 시작해주는 for  
ever도 설치한다. Auto Scaling 그룹에서는 EC2 인스턴스가 생성된 후에도 웹  
서버를 업데이트할 때 유용하다. 업데이트 스크립트는 부록에서 설명한다.   
```
sudo npm install -g forever
```
  
/hime/ec2-user에 Node.js 웹 서버 디렉터리(ExampleTicketWebServer)를 생성  
한다. 그리고 aws s3 sync 명령으로 S3 버킷(`<프로젝트 이름>.src`)에서 파일  
을 받는다.   

```
mkdir ExampleTicketWebServer
aws s3 sync --region=ap-northeast-2 s3://exampleticket.src/ExampleTicketWebServer ExampleTicketWebServer
```
  
Node.js 웹 서버 디렉터리(ExampleTicketWebServer)로 이동한 뒤 npm install  
명령으로 Node.js 모듈을 설치한다. 앞에서 package.json 파일에 필요한 Node.  
js 모듈을 정의했으므로 npm install 명령만 입력하면 자동으로 필요한 모듈이  
설치된다.   

```
cd ExampleTicketWebServer
npm install
```

Node.js와 필요한 모듈 설치가 완료되었다. 'Auto Scaling에 사용할 AMI 생성  
하기'를 참조해 AMI를 생성한다. AMI의 이름은 ExampleTicketAutoScalingAMI로  
설정한다.  
<img src="https://user-images.githubusercontent.com/33191974/160119405-cb8e78da-5fc9-4624-a58e-5f5a8e3d12b1.png" width="50%" height="50%"/>    































