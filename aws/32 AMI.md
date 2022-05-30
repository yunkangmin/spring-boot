## AMI
AMI(Amazon Machine Images)는 EC2 인스턴스를 생성하기 위한 기본 파일이다.  
AWS에서는 빈 EC2 인스턴스에 직접 OS를 설치할 수는 없다.  
그렇기 때문에 미리 OS가 설치된 AMI를 이용하여 EC2 인스턴스를 생성하게 된다.  
이 AMI는 단순히 OS만 설치되는 것이 아니라 각종 서버 애플리케이션, 데이터베이스,  
방화벽 등의 네트워크 솔루션, 각종 비즈니스 솔류션 등도 함께 설치될 수 있다.  

>#### 프리티어에서 사용가능  
>AMI는 프리티어에서 무료로 사용가능하다. 

모든 설치와 과정이 완료된 AMI를 이용하여 EC2 인스턴스를 신속하게 늘려나가는  
자동 횡적 확장(Auto Scaling)을 가능하게 한다(Auto Scaling)은 뒤에서 자세히  
설명한다.)

![image](https://user-images.githubusercontent.com/33191974/138083739-3abf6161-3554-4e33-a352-cc964302be0d.png)  
AMI는 설치 및 설정이 완료된 EC2 인스턴스를 신속하게 생성해야 할 때, Auto Scaling등으로 자동화 할 때,  
EC2 인스턴스를 다른 리전으로 이전해야 할 때, 상용 솔루션을 사용하고자 할 때 사용한다.  
  
'EBS 스냅샷 생성하기'에서 EBS 스냅샷으로 AMI를 생성해보았다.  
이번에는 AMI에 대해 좀더 자세히 알아보자.  

>#### VM Import/Export
>빈 EC2 인스턴스에 직접 OS를 설치할 수 없지만 가상화 소프트웨어(VMware, Microsoft Hyper-V)를 이용하여  
>OS를 설치한 뒤 VM이미지를 AMI로 변환할 수 있다.  
