## AMI를 다른 리전으로 복사하기
생성한 EBS 스냅샷을 다른 리전으로 복사할 수 있는 것처럼 AMI도 다른 리전으로 
복사할 수 있다. AMI는 리전별로 관리되기 때문에 AMI를 다른 리전에서 사용하려면  
AMI를 해당 리전으로 복사해주어야 한다.  
  
AMI 목록에서 복사하고자 하는 AMI를 선택하고 마우스 오른쪽 버튼을 클릭하면  
팝업 메뉴가 나온다. Copy AMI를 클릭한다. 
![image](https://user-images.githubusercontent.com/33191974/138216413-6a8f29db-7243-4e8f-96b1-1724d6943436.png)

AMI를 복사한다.
- Destination Region : 복사할 리전을 설정한다. US West(N. California)를 선택한다. 
- Name : 새로 만들어질 AMI의 이름이다. 기본값 그대로 사용한다. 
- Description : 새로 만들어질 AMI의 설명이다. 기본값 그대로 사용한다.

설정이 완료되었으면 Copy AMI 버튼을 클릭한다.   
![image](https://user-images.githubusercontent.com/33191974/138216949-9d885c29-0443-425e-bcd3-00356b6471a8.png)  
AMI 복사가 시작되었다는 알림 창이 표시된다. Visit the AMIs in us-west-1 링크를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/138217086-58fd7988-6898-4be5-8d07-24294ee35851.png)  
AMI 목록으로 이동했다(화면 맨 위의 리전이 N. California로 선택되어 있는지 확인한다).  
방금 복사한 AMI를 확인할 수 있다. 복사가 완료되는데 시간이 약간 걸린다.  
![image](https://user-images.githubusercontent.com/33191974/138217250-4a1640fb-a1f7-4fe1-852a-0ec0085e41f0.png)



