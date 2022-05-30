## 인스턴스 스토리지를 Root 장치로 사용하는 EC2 인스턴스 생성하기
일반적으로 인스턴스를 생성하면 항상 EBS를 Root 장치로 사용하게 된다(EBS-backed instance).  
이번에는 인스턴스 스토리지를 Root 장치로 사용하는  
EC2 인스턴스(Instance store-backed instance)를 생성해보자.  
  
EC2 인스턴스 목록에서 Launch Instance 버튼을 클릭한다.  
AMI를 선택하는 페이지에서 Community AMIs를 클릭하고 Instance store에 체크한다.  
AMI 목록에 인스턴스 스토리지를 Root 장치로 사용하는 AMI 목록이 표시된다.  
여기서 최신 버전을 선택하여 EC2 인스턴스를 생성하면 된다.  
(HVM보다 paravirtual이 성능이 더 좋다.)  
![image](https://user-images.githubusercontent.com/33191974/137576435-0ff5f360-7e71-4cea-a3b9-d40fd9936d48.png)
![image](https://user-images.githubusercontent.com/33191974/137576473-996e8192-b10c-4b73-bbb5-4a883dcbab36.png)  
마이크로 인스턴스(t1.micro)는 인스턴스 스토리지를 Root 장치로 사용할 수 없다.  
상위 유형을 선택한다.  
![image](https://user-images.githubusercontent.com/33191974/137576705-ebce6765-90af-41fa-904e-d5c395e22073.png)
![image](https://user-images.githubusercontent.com/33191974/137576795-c4d333b1-e470-4f3b-a8d5-4e8b8627ad0e.png)  
나머지 설정 방법은 EBS를 Root 장치로 사용하는 EC2 인스턴스와 동일하다. 
인스턴스 스토리지를 Root 장치로 사용하는 EC2 인스턴스는 정지(Stop) 기능이 없다.  
재부팅이나 삭제(Terminate)만 할 수 있다.  


