웹 서버 AMI도 준비되었으니 EC2 인스턴스 생성 옵션 설정과 Auto Scaling 그룹을  
생성한다.  

- My AMIs에서 방금 생성한 웹 서버 AMI(ExampleTicketAutoScalingAMI)를 사용한다.   
기본 Amazone Linux AMI를 선택하지 않도록 주의한다.   
- S3, Auto Scaling, SQS, DynamoDB 접근용 IAM 역할(ExampleTicketRole)을 사용  
하도록 설정한다.   
- CloudWatch 세부 모니터링 사용하도록 설정한다.  
- User data에 아래 코드를 입력한다.  
- 나머지는 기본값 그대로 사용한다.   

User data  
```
#!/bin/bash
cd /home/ec2-user
aws s3 sync --region=ap-northeast-2 \
  s3://exampleticket.src02/ExampleTicketWebServer ExampleTicketWebServer
cd ExampleTicketWebServer
npm install
sudo forever start w app.js
```
  
S3 버킷에서 웹 서버 소스를 최신 버전으로 업데이트한다. 그리고 package.json  
파일에 모듈이 추가되었을 때 추가된 모듈을 설치할 수 있도록 npm install 명  
령을 한 번 더 실행하고 forever 명령을 실행한다.   
  
시작 템플릿 생성    
<img src="https://user-images.githubusercontent.com/33191974/160122928-67e1b219-9262-4cc1-a85a-41e424a82864.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160122991-de5bdffa-eb2e-4f59-bc52-5014b516747b.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160123021-3a3bf4f4-70c9-4404-9abb-ab48b2fc2ca1.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160123048-b4c0ef24-b315-4341-9bc4-76e445f7c82f.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160123135-e539b56d-0d0e-4bd4-936a-9f97a6ccf6cb.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160123163-16edc7e6-716c-4d80-a7ed-4c38e25de293.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160123203-a13761e1-2a3f-4de0-be69-836c8d5c029c.png" width="50%" height="50%"/>  

생성될 EC2 인스턴스에서 사용할 Security Group을 설정한다. Create a new se   
curity group으로 Security Group을 새로 생성할 때는 Add Rule 버튼을 클릭  
하여 HTTP 프로토콜(80번 포트)을 꼭 추가한다.   
  
Select an existing security group으로 이미 있는 Security Group을 선택할  
때는 HTTP 프로토콜(80번 포트)이 열려있는 Security Group을 선택한다.  
  
Auto Scaling 그룹을 생성한다.  
- 그룹 이름은 ExampleTicketAutoScalingGroup으로 한다.  
- EC2 인스턴스 3개를 생성할 것이므로 Group size에 3을 입력한다.   
- Subnet 부분을 클릭하여 EC2 인스턴스를 생성할 서브넷을 선택한다.   
- Load Balancing을 사용하도록 설정하고, 앞에서 생성한 ELB 로드 밸런서를  
선택한다.   
- CloudWatch 세부 모니터링을 사용하도록 설정한다.   
- 나머지는 기본값 그대로 사용한다.  
- Auto Scaling 정책은 'EC2 생성 옵션 설정과 Auto Scaling 그룹 생성하기'를  
참조하여 설정하기 바란다.   


<img src="https://user-images.githubusercontent.com/33191974/160124195-58dc0b8a-09e3-4fa3-9081-8db9018478f6.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160124647-4c227497-c454-4316-822f-0c4c64a06519.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/160124869-eab65797-84fa-4005-9b0c-15df46d00e33.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160124933-17be5ea4-4f22-4b7a-be6f-d9d2d6736746.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160125040-20584ccb-3d4e-4175-b2bc-6f2b40095361.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160125071-dff1712c-aad1-424f-95ea-fdbc60934107.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160125110-16aed3e4-3863-4cbd-9a61-01dc1f1149f3.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160125187-8a29c6ed-5b3e-4c33-8e19-92b95396c19e.png" width="50%" height="50%"/>  

  
웹 서버 Auto Scaling 그룹을 생성한 모습이다.  
<img src="https://user-images.githubusercontent.com/33191974/160125753-1cbdd2da-6f23-407a-8147-0f97cd073125.png" width="50%" height="50%"/>   


























