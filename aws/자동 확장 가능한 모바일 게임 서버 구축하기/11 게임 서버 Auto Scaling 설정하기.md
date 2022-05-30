# 정리
ELB 가용영역와 Auto Scaling 가용영역은 같아야 할 것 같다.   
- ELB 가용영역은 EC2 인스턴스에 사용한 가용영역.     
- Auto Scaling 가용영역은 EC2 인스턴스가 생성될 가용영역.    

---

게임 서버 AMI도 준비되었으니 EC2 인스턴스 생성 옵션 설정과 Auto Scaling  
그룹을 생성한다.  
- My AMIs에서 방금 생성한 웹 서버 AMI(ExampleGameAutoScalingAMI)를 사용   
한다. 기본값 Amazon Linux AMI를 선택하지 않도록 주의한다.   
- S3, DynamoDB 접근용 IAM 역할(ExampleGameRole) 사용하도록 설정한다.   
- CloudWatch 세부 모니터링을 사용하도록 설정한다.  
- User data에 다음 코드를 입력한다.  
- 나머지는 기본값 그대로 사용한다.   

User data  
```
#!/bin/bash
cd /home/ec2-user
aws s3 sync --region=ap-northeast-2 \
   s3://examplegame.src02/ExampleGameServer ExampleGameServer
cd ExampleGameServer
npm install
sudo ln -s /home/ec2-user/.nvm/versions/node/v17.8.0/bin/node /usr/bin/node
sudo node app.js
```

S3 버킷에서 웹 서버 소스를 최신 버전으로 업데이트한다. 그리고 package.  
json 파일에 모듈이 추가되었을 때 추가된 모듈을 설치할 수 있도록 npm  
install 명령을 한 번 더 실행하고 서버를 실행한다.  
<img src="https://user-images.githubusercontent.com/33191974/160268387-2cf91a39-2a0f-4753-bba3-bfbe51699327.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/160268400-25faa19f-cadc-4b25-803b-fb6b83f76c02.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268405-231bce39-4fab-44ab-afe2-996352c5df16.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268410-2e47902b-7418-42ec-a566-73fdda42c35c.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268419-83fe5df4-1f0a-4512-ae60-53404d222ddc.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268423-d3efc6cb-036a-4d40-99f2-c9120d49dc97.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268430-85c5fa13-bb82-44a4-8a6a-fc58e595b65b.png" width="50%" height="50%"/>  

생성될 EC2 인스턴스에서 사용할 Security Group을 설정한다. Create a new secu  
rity group으로 Security Group을 새로 생성할 때는 Add Rule 버튼을 클릭하여  
HTTP 프로토콜(80번 포트)을 꼭 추가한다.  
  
Select an existing security group으로 이미 있는 Security Group을 선택할  
때는 HTTP 프로토콜(80번 포트)이 열려 있는 Security Group을 선택한다.   
  
Auto Scaling 그룹을 생성한다.  
- 그룹 이름은 ExampleGameAutoScalingGroup으로 한다.  
- Subnet 부분을 클릭하여 EC2 인스턴스를 생성할 서브넷을 선택한다.  
- Load Balancing을 사용하도록 설정하고, 앞에서 생성한 ELB 로드 밸런서를  
선택한다.   
- CloudWatch 세부 모니터링을 사용하도록 설정한다.   
- 나머지는 기본값 그대로 사용한다.   
- Auto Scaling 정책은 'EC2 생성 옵션 설정과 Auto Scaling 그룹 생성하기'를  
참조하여 설정하기 바란다.  

<img src="https://user-images.githubusercontent.com/33191974/160268543-cd0de113-857e-4683-9803-2cc3546e53d4.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268572-6bd62caa-288c-4ba5-b5be-3d8d994c4aaa.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268580-f16527dc-826a-443a-b813-6d4936284c0c.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268583-8248a205-1343-44d2-b404-cde0238c2baa.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268590-baa23252-d593-47ec-b391-a6bbb4a5d2c4.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268600-4c7d7fcc-cdc2-4733-971b-7120e89626e0.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268603-ae6de423-e670-4e12-a475-05f3758c1571.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/160268611-82ee0d5b-bde3-44dc-8a1e-6a64a6bd3d14.png" width="50%" height="50%"/>  
  
웹 서버 Auto Scaling 그룹을 생성한 모습이다.  
<img src="https://user-images.githubusercontent.com/33191974/160268669-9e91a499-697a-4cee-81cf-a03137137a64.png" width="50%" height="50%"/>  





























