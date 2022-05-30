# 정리  
- Auto Scaling은 CloudWatch와 연동하여 EC2 인스턴스의 CPU 사용률, 네트워크   
사용량이 늘어났을 때 EC2 인스턴스를 생성하고, CPU 사용률, 네트워크 사용량이   
줄어들면 EC2 인스턴스를 삭제한다. CPU 사용률, 네트워크 사용량뿐만 아니라   
CloudWatch에서 지원하는 모든 측정치(Metric)와 연동할 수 있다.    
   
- CloudWatch는 EC2 인스턴스가 이상이 있을 경우 알림을 받고자 할 때, 사용량이  
급증했을 때 자동으로 횡적 확장(Auto Scaling)을 하고 부하 분산(Elastic Load  
Balancing)을 구축할 때 사용한다. 
CloudWatch가 Auto Scaling에게 알려준다. 

---

웹 서버 AMI도 준비되었으니 EC2 인스턴스 생성 옵션 설정과 Auto Scaling 그룹을  
생성한다.  
- My AMIs에서 방금 생성한 웹 서버 AMI(ExamplePhotoAutoScalingAMI)를 사용한다.  
기본 Amazon Linux AMI를 선택하지 않도록 주의한다.   
- S3, SQS 접근용 IAM 역할(ExamplePhotoRole)을 사용하도록 설정한다.  
- CloudWatch 세부 모니터링을 사용하도록 설정한다.  
- User Data에 아래 코드를 입력한다.   
- 나머지는 기본값 그대로 사용한다.  
  
User data  
```
#!/bin/bash
aws s3 sync --region=ap-northeast-2 \
s3://examplephoto.src02/ExamplePhotoWebServer ExamplePhotoWebServer
cd ExamplePhotoWebServer
npm install
sudo node app.js
```
S3 버킷에서 웹 서버 소스를 최신 버전으로 업데이트한다. 그리고 package.json  
파일에 모듈이 추가되었을 때 추가된 모듈을 설치할 수 있도록 npm install  
명령을 한 번 더 실행하고 node 명령을 실행한다.  
  
[여기](https://github.com/yunkangmin/spring-boot/blob/main/aws/AutoScaling/03%20EC2%20%EC%83%9D%EC%84%B1%20%EC%98%B5%EC%85%98%20%EC%84%A4%EC%A0%95%EA%B3%BC%20Auto%20Scaling%20%EA%B7%B8%EB%A3%B9%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.md)를 참고하여 Auto Scaling을 생성하자.   
    
생성될 EC2 인스턴스에서 사용할 Security Group을 설정한다. Create a new   
security group으로 Security Group을 새로 생성할 때는 Add Rule 버튼을 클릭  
하여 HTTP 프로토콜(80번 포트)을 꼭 추가한다.   
  
Select an existing security group으로 이미 있는 Security Group을 선택할  
때는 HTTP 프로토콜(80번 포트)이 열려 있는 Security Group을 선택한다.  
  
Auto Scaling 그룹을 생성한다.   
- 그룹 이름은 ExamplePhotoAutoScalingGroup으로 한다.  
- Subnet 부분을 클릭하여 EC2 인스턴스를 생성할 서브넷을 선택한다.  
- Load Balancing을 사용하도록 설정하고, 앞에서 생성한 ELB 로드 밸런서를   
선택한다.  
CloudWatch 세부 모니터링을 사용하도록 설정한다.   
- 나머지는 기본값 그대로 사용한다.   
- Auto Scaling 정책은 'EC2 생성 옵션 설정과 Auto Scaling 그룹 생성하기'  
를 참조하여 설정하자.   
  
웹 서버 Auto Scaling 그룹을 생성한 모습이다.   
<img src="https://user-images.githubusercontent.com/33191974/159628850-75367199-751b-4d56-aaa0-271f05a11704.png" width="50%" height="50%"/>  


  

























