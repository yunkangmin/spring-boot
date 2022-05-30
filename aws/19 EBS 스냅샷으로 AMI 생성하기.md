## EBS 스냅샷으로 AMI 생성하기
EBS 스냅샷으로 EBS 볼륨뿐만 아니라 EC2 인스턴스를 생성할 수 있는 AMI  
(Amazon Machine Image)도 생성할 수 있다.  
EBS 스냅샷으로 AMI를 생성할 때 주의할 점은 Linux의 경우 Kernel ID를 알아야 한다는 것이다.  
AMI를 만들 때 이 Kernel ID를 설정하게 된다.  
나중에 AMI로 EC2 인스턴스를 생성했을 때 Kernel ID가 맞지 않으면 부팅이 되지 않는다.  
(커널 패닉 발생). 단, HVM(t2 유형등)은 Kernel ID를 설정하지 않아도 된다.  
  
먼저 Kernel ID를 알아보자.  
EC2 인스턴스 목록에서 EC2 인스턴스를 선택한다.  
앞에서 EBS 스냅샷을 만들 때 이 EC2 인스턴스(Example Server)에 장착된  
8GiB EBS 볼륨으로 만들었다.  
그리고 이 EC2 인스턴스는 amzn2-ami-hvm-2.0.20211001.1-x86_64-gp2으로 생성했다.  
  
EBS 스냅샷 목록에서 앞에서 생성한 8GiB EBS 스냅샷을 선택하고, 마우스 오른쪽 버튼을  
클릭하면 팝업 메뉴가 나온다.  
![image](https://user-images.githubusercontent.com/33191974/137573591-3e76ec4e-03ae-4af6-9959-7f2f8117b4ab.png)
EBS 스냅샷으로 AMI를 생성한다. 
- Name : AMI 이름이다. Exmple AMI로 설정한다. 
- Description : AMI 설명이다(입력하지 않아도 상관없다.)
- Architecture : CPU 아키텍처 설정이다. 32비트인 i386과 64비트인 x86_64를 선택할 수 있다.  
                 기본값 그대로 사용한다. 
- Virtualization : 가상화 유형이다. 기본값 그대로 사용한다. 
- Root device name : Linux Root device 이름이다. 기본값 그대로 사용한다.  
- Kernel ID : 부팅할 때 사용할 커널이다. 앞에서 EC2 인스턴스 세부 내용에 표시된    
              Kernel ID는 없었다.(HVM이므로) 각자 상황에 맞게 설정한다.  
              (만약에 반가상화(PV)를 선택했다면 커널을 설정해줘야 한다.)
- RAM disk ID : 램 디스크 ID이다. 기본값 그대로 사용한다. 
- Block Device Mappings : 기본 장착될 EBS 볼륨 설정이다. 기본값 그대로 사용한다.  

설정이 완료되었으면 생성버튼을 클릭하여 생성한다. 
![image](https://user-images.githubusercontent.com/33191974/137575897-a93a5894-2a6a-4bd8-b59b-09aad094e55e.png)  
![image](https://user-images.githubusercontent.com/33191974/137575921-d75ac6ba-4e2d-4e98-b8e7-6e38d638712b.png)  

AMI 목록으로 이동했다. 이제 AMI가 생성되었다. Launch 버튼을 클릭하면 이 AMI로 EC2 인스턴스를  
생성할 수 있다.  
EC2 인스턴스를 생성하는 방법은 앞에서 설명한 EC2 인스턴스 생성하기 방법과 동일하므로 따로 설명  
하지 않는다.  
![image](https://user-images.githubusercontent.com/33191974/137576009-76152498-94f5-4e90-970f-08644781c416.png)

사용하지 않는다면 등록취소를 눌러 삭제하자.









