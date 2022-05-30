## EC2 인스턴스로 AMI 생성하기
앞에서 EBS 스냅샷으로 AMI를 생성해보았다.  
이번에는 EC2 인스턴스를 바로 AMI로 생성하는 방법을 알아보자.  
  
EC2 인스턴스 목록에서 AMI를 생성할 EC2 인스턴스(Example Server)를 선택하고  
마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다.
![image](https://user-images.githubusercontent.com/33191974/138091606-58e97699-b902-46e2-9284-653f7bd822d6.png)  

아래 그림처럼 EC2 인스턴스로 AMI를 생성한다.  
- Image name : AMI 이름이다. Example AMI 2로 설정한다. 
- Image Description: AMI 설명이다.(입력하지 않아도 상관없다.)
- No reboot: AMI를 생성할 때 EC2 인스턴스를 재부팅하지 않고 생성하는 옵션이다.  
  EC2 인스턴스를 재부팅하지 않고 AMI를 생성하면 파일시스템의 무결성을 보장하지 않는다.
- Instance Volumes: 기본 장착될 EBs 볼륨 설정이다. 기본값 그대로 사용한다.  

설정이 완료되었으면 Create Image 버튼을 클릭한다.
![image](https://user-images.githubusercontent.com/33191974/138215079-5332cc34-4227-4bb3-a8da-9861a967f36d.png)

> #### 파일시스템 무결성  
> 파일시스템의 성능을 높이기 위해 파일을 디스크에 바로 쓰지 않고 메모리에 모아놓았다가  
> 쓴다. 이 방식은 파일 시스템마다 알고리즘이 다르다.  
> 다양한 이유로 쓰기 동작이 완료되지 않은 상태가 생길 수 있는데 이 상태가 파일시스템의 무결성이  
> 깨진 상태이다. 대부분 큰 문제없이 복구되지만 최악의 경우 파일을 읽을 수 없거나  
> OS를 부팅할 수 없는 상태가 되기도 한다.  
> 재부팅하지 않고 AMI를 생성하면 파일이 메모리에 남아있을 수도 있겟다는 생각이 든다. 

AMI 목록으로 이동하자.  
AMI 목록에 방금 생성한 AMI가 표시된다. Launch 버튼을 클릭하면 이 AMI로 EC2 인스턴스를 생성할 수 있다.  
생성하는 방법은 앞서 설명한 EC2 생성하기 방법과 동일하므로 따로 설명하지 않는다.
![image](https://user-images.githubusercontent.com/33191974/138216061-9d86b321-a525-420b-bab9-cfd5495bc4a7.png)




