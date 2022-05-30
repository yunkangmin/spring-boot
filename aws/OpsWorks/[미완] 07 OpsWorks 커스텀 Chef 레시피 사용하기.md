# 정리  
- aws에서  Chef 쿡북, 레시피
- Chef 쿡북: 레시피, 속성, 템플릿, 라이브러리등의 묶음이다. 쿡북은 애플리케이   
션 단위로 구성한다. 쿡북 저장소로 S3 버킷, HTTP, Subversion, Git을 사용할  
수 있다.  
  
- Chef 레시피: 애플리케이션 설치 및 업데이트, 소스 배포 방법이 정의된 파일이다.   
 
- 프로세스  
1. 스택 생성(커스텀 레시피)  
2. 커스텀 레이어 생성  
3. 레이어에서 레시피 편집  
  
- Hostname: 인스턴스 이름  
- getting-started::default  
- getting-started(레이어 이름)  


---


[참고](https://docs.aws.amazon.com/ko_kr/opsworks/latest/userguide/workingcookbook-installingcustom-enable.html)  
  
OpsWorks에서 제공하는 Chef 레시피가 아닌 인터넷에 공개된 Chef 레시피를 사용해보자.   
OpsWorks에서 커스텀 Chef 레시피를 사용할 때 작업 흐름은 다음과 같다.   
<img src="https://user-images.githubusercontent.com/33191974/158545098-97514850-c8e5-4202-ba60-ccb13f7541cb.png" width="50%" height="50%"/>    
Chef 웹사이트는 전 세계 많은 사람이 개발한 Chef 쿡북을 제공한다. 여기에는  
웬만한 소프트웨어 쿡북은 다 있다.   
https://community.opscode.com/cookbooks(현재는 안됨)  

---
  
<img src="https://user-images.githubusercontent.com/33191974/158546809-97d3322b-2ef8-462a-aba8-8a1ea073f7c6.png" width="50%" height="50%"/>    
  
OpsWorks 커스텀 스택을 생성한다.     
- Region: EC2 인스턴스가 생성될 리전이다. 서울리전을 선택한다.    
- Name: 리전을 선택하면 스택 이름을 설정할 수 있다. CustomStack을 입력한다.  
- VPC: EC2 인스턴스가 위치할 VPC이다. 기본 값 그대로 사용한다.   
- Default subnet: EC2 인스턴스가 위치할 서브넷이다. 기본값 그대로 사용한다.  
- Default operating system: EC2 인스턴스에 설치될 운영체제이다. 기본값  
그대로 사용한다.  
- Default root device type: EC2 인스턴스의 Root 스토리지 유형이다. 기본값   
그대로 사용한다.  
- IAM role: OpsWorks의 IAM 역할이다. 기본값 그대로 사용한다.    
- Default SSH key: EC2 인스턴스에 접속할 때 사용할 키 쌍이다. 앞에서 생성한     
awskeypair를 선택한다.    
- Default IAM instance profile: EC2 인스턴스에 사용할 IAM 역할이다. 기본값   
그대로 사용한다.   
- Hostname theme: EC2 인스턴스에 이름을 붙이는 방식이다. 과일 이름, 태양계  
행성 이름 등을 사용할 수 있다. 기본값 그대로 사용한다.  
- Stack color: 스택 상징 색이다. 사용하고 싶은 색을 선택한다.   
- Chef version: Chef 버전이다. 기본값 그대로 사용한다.   
- Use custom Chef cookbooks: OpsWorks에서 제공하는 Chef 쿡북 이외에 인터넷에   
공개된 Chef 쿡북이나 사용자가 작성한 Chef 쿡북을 사용하는 옵션이다. Yes를  
선택한다.  
   - Repository type: Chef 쿡북 저장소 종류이다. Git을 사용한다.   
   - Repository URL: Chef 쿡북 저장소 URL이다. https://github.com/amazon  
   webservices/opsworks-example-cookbooks.git을 입력한다.   
   - Branch/Revision: Git 저장소에서 특정 브랜치나 리비전을 가져오는 옵션   
   이다. 기본값 그대로 비워둔다.   
   - Manage Berkshelf: Berkshelf로 Chef 쿡북 의존성을 관리해주는 옵션이다.
- Custom JSON: Chef 레시피에 넘겨줄 속성(Attibute) 값이다. 기본값 그대로  
비워둔다.  
- Use OpsWorks security groups: OpsWorks용 Security Group을 사용하는 옵션  
이다. 기본값 그대로 사용한다.  
    
설정이 완료되었으면 Add Stack 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158551266-d465195c-5331-4e84-911e-2b754a0d32ad.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/158551421-8ecf1e3a-0328-45a1-a8d7-b27ce1b30cc6.png" width="50%" height="50%"/>    
OpsWorks 커스텀 스택이 생성되었다. Add a layer를 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158552052-627fbec3-5a35-4c63-acda-0d4ae817e4c7.png" width="50%" height="50%"/>   
OpsWorks 커스텀 레이어를 생성한다.   
- Layer type: 레이어 종류이다. Custom을 선택한다.  
- Name: OpsWorks에서 표시될 이름이다. getting-started를 입력한다.   
- Short name: Chef에서 사용할 이름이다. getting-started를 입력한다.   

설정이 완료되었으면 Add Layer 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158552882-9af4ec99-5ce7-456e-b6e2-8479eaca708a.png" width="50%" height="50%"/>   
OpsWorks 커스텀 레이어가 생성되었다. getting-started 레이어의 Recipes를   
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158570277-20ef33ea-420e-4b95-98cd-06bdf0db8b32.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158571140-2615919a-9d0a-447d-856e-d61f8f817fe4.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158571233-dfb708c3-d0bb-4e5c-849f-be75a94d41e9.png" width="50%" height="50%"/>  
커스텀 Chef 레시피 편집 화면이 표시된다. Setup에 getting-started::default를   
입력하고 +를 클릭한다. 그리고 Save 버튼을 클릭한다.   
- Setup: 인스턴스가 새로 생성되어 부팅될 때 실행된다.   
- Configure: 인스턴스 상태가 온라인이 되거나 다른 상태로 바뀔 때 실행된다.   
- Deploy: 배포 명령 때 실행된다.   
- Undeploy: 인스턴스 삭제, Undeploy 명령일 때 실행된다.   
- Shutdown: 인스턴스가 정지될 때 실행된다.    
   
<img src="https://user-images.githubusercontent.com/33191974/158571777-662d35fc-ec24-4991-aacb-ea378359c0b7.png" width="50%" height="50%"/>   
커스텀 Chef 레시피 설정이 저장되었다. 위쪽 Instances 버튼을 클릭한다. 
<img src="https://user-images.githubusercontent.com/33191974/158572412-8059a88e-dfff-417e-ad62-f5187d826f1b.png" width="50%" height="50%"/>     
Add an Instance를 클릭한다. 그리고 Size를 t2.micro로 선택한 뒤 Add Instance  
버튼을 클릭한다.     
<img src="https://user-images.githubusercontent.com/33191974/158573381-af4be6ea-9e04-4234-bf5a-14ae07675e23.png" width="50%" height="50%"/>  
OpsWorks 인스턴스가 생성되었다. 아직은 정지 상태이므로 start를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158573893-09a91bcb-94f9-4564-83eb-f96826f939a7.png" width="50%" height="50%"/>  
잠시 기다리면 인스턴스가 완전히 시작된다.  
  
인스턴스의 Public IP에 SSH로 접속한다. 그리고 다음 명령을 입력한다.   
```
$ sudo vim /root/chef-getting-started.txt
```
getting-started::default 레시피는 `/root/chef-getting-started.txt`라는 파일을    
생성한다. 아래 그림처럼 표시되면 레시피가 실행된 것이다.  











 












 


 






















