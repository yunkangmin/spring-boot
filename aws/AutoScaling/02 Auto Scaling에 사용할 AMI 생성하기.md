아무것도 설치되지 않은 EC2 인스턴스를 바로 생성해 확장할 수도 있다. 하지만, 현재 프로젝트에서   
사용하는 웹 서버와 각종 플랫폼을 미리 설치한 AMI를 생성해 사용하면 편리하다. 보통 Auto Scaling은   
ELB 로드 밸런서와 함께 사용한다. '부하 분산과 고가용성을 제공하는 ELB'를 먼저 보기 바란다.   
  
이제 EC2 인스턴스에 웹 서버와 애플리케이션 소스가 설치된 AMI를 생성해 Auto Scaling에 사용해보자.   
AWS 콘솔로 접속한 뒤 메인 화면에서 Compute & Networking의 EC2를 클릭한다. Auto Scaling은 항상  
EC2와 함께 사용해야 하므로 EC2와 같은 페이지에 있다.  
  
서울리전을 선택한다.   
아직 ELB 로드 밸런서와 EC2 인스턴스(웹 서버 실행)를 생성하지 않았다면 'ELB 로드 밸런서 생성하기,   
EC2 인스턴스에 웹 서버 실행하기'를 참조하여 ELB 로드 밸런서와 EC2 인스턴스를 생성하고 웹 서버를  
실행한다.   
  
EC2 인스턴스 목록에서 AMI를 생성할 EC2 인스턴스(ELB 로드 밸런서와 연결된)를 선택하고, Create   
Image를 클릭한다.     
<img src="https://user-images.githubusercontent.com/33191974/158009550-49d54a5c-551e-4304-a7ef-d7a070cd3ceb.png" width="50%" height="50%"/>       
EC2 인스턴스로 AMI를 생성한다.   
- Image name: AMI 이름이다. AutoScalingAMI로 설정한다.   
- Image Description: AMI 설명이다(입력하지 않아도 상관없다).
- No reboot: AMI를 생성할 때 EC2 인스턴스를 재부팅하지 않고 생성한다. EC2 인스턴스를 재부팅하지 않고   
AMI를 생성하면 파일 시스템의 무결성을 보장하지 않는다.   
- Instance Volumes: 기본 장착될 EBS 볼륨이다. 기본값 그대로 사용한다.   

설정이 완료되었으면 Create Image 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158009806-90fbc7b8-fdb7-4c63-8e64-5bc8b9bfd260.png" width="50%" height="50%"/>     
Auto Scaling에서 사용할 AMI을 생성중이다.   
<img src="https://user-images.githubusercontent.com/33191974/158009878-0f011f5c-9716-4c33-8a6b-1860dca56ae5.png" width="50%" height="50%"/>   
































