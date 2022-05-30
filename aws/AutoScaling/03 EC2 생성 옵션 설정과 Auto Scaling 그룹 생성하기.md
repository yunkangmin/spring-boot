EC2 인스턴스 생성 옵션(Launch Configuration)을 설정하고, Auto Scaling 그룹을 생성해보자.   
Auto Scaling 그룹 목록(AUTO SCALING -> Auto Scaling Groups)을 클릭한다. 생성한 EC2 인스턴스 생성  
옵션과 Auto Scaling 그룹이 하나도 없을 때 아래 그림과 같은 페이지가 표시된다. Create Auto Scaling  
Group 버튼을 클릭한다.   
    
<img src="https://user-images.githubusercontent.com/33191974/158012420-9f438983-6e61-4ea5-8256-babd9ae22d63.png" width="50%" height="50%"/>  
EC2 인스턴스 생성 옵션과 Auto Scaling 그룹을 연달아 생성한다(나중에 개별적으로 생성할 수 있다).  
이름은 ExampleAutoScalingConfig를 입력한다.   
<img src="https://user-images.githubusercontent.com/33191974/158013426-d08bd3d6-5448-4cd6-b732-1f1d49e18f39.png" width="50%" height="50%"/>   
시작 템플릿 생성을 클릭한다. Amazon EC2 Auto Scaling에서 생성하는 EC2 인스턴스 유형을 지정하는 시작   
템플릿을 생성한다. 사용할 Amazon Machine Image(AMI) ID, 인스턴스 유형, 키 페어 및 보안 그룹 같은  
정보를 포함시킨다.   
  
시작 템플릿을 생성한다(EC2 인스턴스 생성 옵션을 설정한다).
- Name: EC2 인스턴스 생성 옵션의 이름이다. ExampleAutoScalingConfig를 입력한다.  
  <img src="https://user-images.githubusercontent.com/33191974/158016320-c956fcf1-0bfb-426d-a819-77b8d845f90d.png" width="50%" height="50%"/>   
    
- Auto Scaling guidance(Auto Scaling 지침)에서 확인란을 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158013580-9c59796c-7b9e-4b97-b19b-6ebc4eb66341.png" width="50%" height="50%"/>  
  
- Amazon Machine Image(AMI)는 빠른 시작 목록에서 앞에서 생성한 이미지(AutoScalingAMI)를 클릭한다.  
  <img src="https://user-images.githubusercontent.com/33191974/158013623-c687323a-4847-42e7-8f7b-3b97daa29052.png" width="50%" height="50%"/>  
   
- 인스턴스 유형에서 지정한 AMI와 호환되는 하드웨어 구성을 선택한다.     
  <img src="https://user-images.githubusercontent.com/33191974/158013652-4b416022-d6d5-4b91-b0bc-22e9614c463c.png" width="50%" height="50%"/>  
  
- 키 페어(로그인)  
  <img src="https://user-images.githubusercontent.com/33191974/158016360-b34b683b-aeb4-4967-ab5b-594d3fbc8f9e.png" width="50%" height="50%"/>     
- 네트워크 설정  
  <img src="https://user-images.githubusercontent.com/33191974/158016595-6643876d-48bc-4550-8dd5-671506bec5bd.png" width="50%" height="50%"/>   
  <img src="https://user-images.githubusercontent.com/33191974/158016609-97a32f2f-7358-490a-bf6a-40c44f68c1ce.png" width="50%" height="50%"/>  
  
- 스토리지 구성은 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158016678-3fdd16c8-1974-46e9-bcfa-c6d1af50283c.png" width="50%" height="50%"/>   

- Purchaing option: 스팟 인스턴스의 사용 옵션이다. 기본값 그대로 사용한다.  
  <img src="https://user-images.githubusercontent.com/33191974/158016711-1577f0f7-f453-4540-a3a6-096ef6557d1e.png" width="50%" height="50%"/>   
 
- IAM role: EC2 인스턴스에서 사용할 IAM 역할이다. IAM 역할을 사용하면 액세스 키와 시크릿 키 없이   
AWS API를 사용할 수 있다. IAM 역할을 생성하는 방법은 'IAM 역할 생성하기'를 참조하기 바란다.  
  <img src="https://user-images.githubusercontent.com/33191974/158016749-bdda358e-d8b2-47c3-af4c-febb12e4776a.png" width="50%" height="50%"/>         
  
- Monitoring: Cloud Watch 세부 모니터링을 사용하는 옵션이다. 1분 단위로 모니터링할 수 있도록 이 부분에   
체크한다.    
  <img src="https://user-images.githubusercontent.com/33191974/158016790-76dcb67a-816a-4b2d-a766-c4d43086fc04.png" width="50%" height="50%"/>  
  
- Kernel ID: EC2 인스턴스를 생성할 때 사용할 Kernel ID이다. 앞에서 AMI에 Kernel ID를 설정했으므로   
기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158016803-5aba70df-ff55-4b54-b9f2-a5d3f9785fdd.png" width="50%" height="50%"/>  
  
- RAM Disk ID: EC2 인스턴스를 생성할 때 사용할 RAM Disk ID이다. 기본값 그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158016823-85c35f3e-f1fb-4cf7-b71c-9375121ba0e3.png" width="50%" height="50%"/>  
  
- User data: root 권한으로 실행할 스크립트이다. 이 부분이 가장 중요하다. EC2 인스턴스가 생성되었을 때  
여기에 설정한 스크립트가 실행된다. As Text를 선택하고 다음 User Data 코드를 입력한다. 이제 EC2 인스턴스   
가 생성되면 Node.js 서버를 실행하게 된다.  
   - As text: 텍스트 상태 스크립트를 입력한다.   
   - Input is already base64 encoded: 스크립트 파일이 BASE64로 인코딩되었을 때 체크한다.     
  <img src="https://user-images.githubusercontent.com/33191974/158016845-8f670466-0b34-4f73-9dea-0945995c223c.png" width="50%" height="50%"/>  
  
- IP Address Type: IP 주소 형태이다. 기본값 그대로 사용한다.   
   - Only assign a public IP address to instance launched in the default VPC and subnet. (default):   
   기본 VPC와 서브넷에 생성된 EC2 인스턴스에만 공인 IP를 부여한다.   
   - Assign a public IP address to every instance: 모든 EC2 인스턴스에 공인 IP를 부여한다.   
   - Do not assign a public IP address to any instance: EC2 인스턴스에 공인 IP를 부여하지 않는다.     
  
User data  
```
#!/bin/bash
node /home/ec2-user/example/app.js 
```

---

#### User data와 최신 애플리케이션 소스 업데이트   
이번 실습에서는 User data에서 Node.js로 웹 서버만 실행된다. 실무에서 사용할 때는 최신 버전의   
애플리케이션 소스를 S3 버킷이나 Git에서 받아온 뒤 웹 서버를 실행하면 된다.  
S3 버킷에서 소스를 받을 때는 다음처럼 한다.   
- S3에 접근할 수 있는 IAM 역할을 생성한다.  
- Auto Scaling으로 EC2 인스턴스를 생성할 때 IAM 역할을 사용하도록 설정한다.  
  
User data
```
#!/bin/bash
cd /home/ec2-user
mkdir ExampleServer
chown -R ec2-user.ec2-user ExampleServer
aws s3 sync s3://examples3origin/ExampleServer ExampleServer
node /home/www/ExampleServer/app.js 
```
  
Git에서 소스를 받을 때는 다음처럼 한다.   
User data  
```
#!/bin/bash   
cd /home/ec2-user  
git clone 깃주소 ExampleServer
chown -R ec2-user.ec2-user ExampleServer
node /home/www/ExampleServer/app.js
```
--- 

나머지 설정은 그대로 두고 시작 템플릿 생성 버튼을 클릭한다.      

시작 템플릿에서 위에서 생성한 `ExampleAutoScalingConfig`을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158013716-117c66c7-c817-46ab-b8f1-22848fc6c048.png" width="50%" height="50%"/>       
- Network: Auto Scaling 그룹이 생성될 VPC이다. 기본값 그대로 사용한다.  
- Subnet: EC2 인스턴스가 생성될 서브넷이다. 빈 칸을 클릭하면 서브넷의 목록이 표시된다. Default in  
ap-northeast-2a, Default in ap-northeast-2c를 클릭한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158013824-33e93354-92cd-4920-98d0-52b47683e0df.png" width="50%" height="50%"/>     
  
- Load Balancing: ELB 로드 밸런서를 사용하는 옵션이다. 이 부분을 체크하고 빈 칸을 클릭하면 현재 생성된   
ELB 로드 밸런서의 목록이 표시된다. 앞에서 생성한 ELB 로드 밸런서(exampleelb)를 선택한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158014126-0176ea21-e310-423c-8fd4-4e3a68cfa0ff.png" width="50%" height="50%"/>    
  

- Health Check Type: Auto Scaling 그룹도 EC2 인스턴스의 헬스 체크를 한다. 기본값 그대로 사용한다.   
   - ELB: ELB 로드 밸런서에서 확인한 헬스 체크 값을 사용한다.   
   - EC2: Auto Scaling 그룹이 개별적으로 EC2 인스턴스의 헬스 체크를 한다.   
- Health Check Grace Period: EC2 인스턴스가 부팅되었을 때(InService) 설정한 시간만큼 헬스 체크를 미룬다.   
- Monitoring: CloudWatch 세부 모니터링을 사용하는 옵션이다. 이 부분에 체크한다.   
다음 버튼을 클릭한다.  
  <img src="https://user-images.githubusercontent.com/33191974/158014140-ffa4c325-cb6c-4954-865e-d38f8514bdcd.png" width="50%" height="50%"/>    
  
EC2 인스턴스를 추가하거나 삭제하는 기준을 설정한다.   
- Keep this group at its initial size: 앞에서 설정한 EC2 인스턴스 개수를 유지한다.   
- Use scaling policies to adjust the capacity of this group: 설정한 정책에 따라서 EC2 인스턴스를   
조절한다. 이 부분을 선택한다.   
- Scale between N and N instance: EC2 인스턴스를 최대 몇 개까지 추가하고, 삭제하더라도 최소 몇 개까지   
남겨둘지 설정한다. 최소 1개에서 최대 3개까지 늘릴 것이다. 1과 3을 입력한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158014617-b7c372ed-9fc7-4993-a193-96861c46b3b2.png" width="50%" height="50%"/>  
  <img src="https://user-images.githubusercontent.com/33191974/158014632-8acd657f-8945-46c7-ab67-a1c841dea02b.png" width="50%" height="50%"/>  
  
다음을 클릭한다.  
Auto Scaling이 동작할 때 알림을 받도록 설정한다. Add notification 버튼을 클릭하고, create topic을   
클릭한다.  
- Send a notification to : 알림 이름이다. AutoScalingTopic을 입력한다.   
- With these recipients: 알림을 받을 이메일 주소이다. SNS 토픽으로 처음 사용하는 이메일 주소라면  
인증 메일이 전송될 것이다. 이 부분은 'SNS 토픽과 이메일 구독 생성하기'를 참조하기 바란다.   
- Whenever instance: 알림을 보내는 조건이다. 기본값 그대로 사용한다.   
   - launch: EC2 인스턴스가 추가되었을 때 알림을 보낸다.  
   - terminate: EC2 인스턴스가 삭제되었을 때 알림을 보낸다. 
   - fail to launch: EC2 인스턴스 추가에 실패했을 때 알림을 보낸다.   
   - faili to terminate: EC2 인스턴스 삭제에 실패했을 때 알림을 보낸다.   


<img src="https://user-images.githubusercontent.com/33191974/158014886-e2c8af73-c668-4691-90ce-03f07eb0b6cf.png" width="50%" height="50%"/>  

다음 버튼을 클릭하여 Auto Scaling Group을 생성한다.  
  
Auto Scaling 그룹 목록에서 방금 생성한 Auto Scaling 그룹(ExampleAutoScalingGroup)을 선택하고,   
아래 세부 내용에서 Instance 탭을 클릭하면 현재 생성된 EC2 인스턴스 목록을 볼 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158017282-d0e7bf3b-bcdb-44b5-b0da-cfc8e4eac661.png" width="50%" height="50%"/>  
EC2 인스턴스 목록으로 이동한다. EC2 인스턴스 목록을 보면 ELB 로드 밸런서를 생성할 때 생성했던 EC2   
인스턴스 2개와 Auto Scaling이 생성한 EC2 인스턴스 1개가 있다(EC2 인스턴스의 세부 내용에서 AMI ID 부분을   
보면 Auto Scaling이 생성한 EC2인지, 아닌지 알 수 있다).   
  
ELB 로드 밸런서를 생성할 때 함께 생성했던 EC2 인스턴스 2개는 이제 삭제하거나 정지해도 된다.   
필자는 이 EC2 인스턴스 2개를 정지시켰다.  
<img src="https://user-images.githubusercontent.com/33191974/158017500-a61de181-530d-4868-a5eb-96be5072eb0a.png" width="50%" height="50%"/>    
Auto Scaling이 생성한 EC2 인스턴스에 SSH로 접속한다. 서비스에 사용자가 늘어나서 트래픽이 증가했다고   
가정하고, CPU 사용률을 강제로 올려볼 것이다. 터미널에서 yes > /dev/null 명령을 입력한다.   
```
[ec2-user@ip-172-31-6-102 ~]$ yes > /dev/null
```
이 명령을 입력한 뒤 가만히 두고 잠시 기다린다. 약 1분 정도 지나면 Auto Scaling 그룹에 새로운 EC2  
인스턴스가 추가된다.   
  
Auto Scaling 그룹목록으로 이동한다. Auto Scaling 그룹(ExampleAutoScalingGroup)의 인스턴스 목록에   
새로운 EC2 인스턴스가 추가되었다.  
<img src="https://user-images.githubusercontent.com/33191974/158017721-328b4020-c020-4978-8548-0887411a51cc.png" width="50%" height="50%"/>   
이제 서비스에 사용자가 줄어서 트래픽이 감소했다고 가정하고 CPU 사용률을 내려보자. yes > /dev/null  
명령을 실행한 터미널에서 ctrl + c를 입력하여 명령을 중지시킨다. 1 ~ 2분 정도 지나면 Auto Scaling 그룹에서  
EC2 인스턴스가 삭제된다(기본적으로 가장 먼저 추가된 EC2 인스턴스부터 삭제된다).  
  
Auto Scaling 그룹(ExampleAutoScalingGroup)의 인스턴스 목록에서 EC2 인스턴스가 삭제되고 있다.   
잠시 기다리면 완전히 삭제되고 목록에서 사라진다.   
<img src="https://user-images.githubusercontent.com/33191974/158017948-122a62bc-4a38-4010-ab93-62c6f20130b4.png" width="50%" height="50%"/>  










  









   
















































