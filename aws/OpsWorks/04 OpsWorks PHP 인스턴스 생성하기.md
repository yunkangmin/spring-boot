Apache와 PHP가 설치된 EC2 인스턴스를 생성해보자. 앞에서 생성한 PHP App Server  
레이어에서 Add instance를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158531273-52a36026-5aaf-4920-abc5-da26e6defd24.png" width="50%" height="50%"/>  
OpsWorks PHP 인스턴스를 생성한다.  
- Hostname: EC2 인스턴스 이름이다. 앞에서 설정한 Hostname theme 규칙대로 이름이   
생성된다. 기본값 그대로 사용한다.   
- Size: EC2 인스턴스 유형이다. t1.micro를 선택한다.    
- Subnet: EC2 인스턴스가 위치할 서브넷이다. 기본값 그대로 사용한다.   
- Scaling type: Auto Scaling 종류이다. 24시간 7일 가동하는 유형인 24/7을    
선택한다.  
- SSH key: EC2 인스턴스에 접속할 때 사용할 키 쌍이다. 기본값 그대로 사용한다.   
- Operating system: EC2 인스턴스에 설치될 운영체제이다. 기본값 그대로 사용한다. 
- Architecture: 운영체제 CPU 아키텍처이다. 기본값 그대로 사용한다.  
- Root device type: EC2 인스턴스의 Root 장치 유형이다. 기본값 그대로 사용한다(  
t2.micro는 인스턴스 스토리지를 사용할 수 없다).  
<img src="https://user-images.githubusercontent.com/33191974/158533025-bd61a97e-1711-4360-add0-5546a29fb570.png" width="50%" height="50%"/>    
OpsWorks PHP 인스턴스가 생성되었다. 아직은 정지 상태이므로 start를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158533243-54c58fc2-1217-417c-a0e2-84d312ce31f7.png" width="50%" height="50%"/>   
잠시 기다리면 OpsWorks가 알아서 Chef 레시피로 Apache와 PHP를 설치하고, 인스턴스를   
시작한다.   
<img src="https://user-images.githubusercontent.com/33191974/158533618-90d79ca4-d72f-42eb-b00c-911a6247f308.png" width="50%" height="50%"/>   


























