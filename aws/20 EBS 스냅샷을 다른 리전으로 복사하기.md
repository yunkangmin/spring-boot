## EBS 스냅샷을 다른 리전으로 복사하기
EBS 볼륨 자체로는 다른 가용 영역(Available Zone)으로 이전이 불가능할 뿐만 아니라  
다른 리전(Region)으로 이전도 불가능하다.  
따라서 EBS 볼륨을 EBS 스냅샷으로 생성한 뒤 다른 리전으로 복사해야 한다.  
  
EBS 스냅샷 목록에서 복사하고자 하는 EBS 스냅샷을 선택하고, 마우스 오른쪽 버튼을  
클릭하면 팝업메뉴가 나온다. 복사를 클릭한다. 
![image](https://user-images.githubusercontent.com/33191974/137576106-cf9b0c0d-9135-4799-b68a-461aa464d496.png)  
- Snapshot : 선택한 EBS 스냅샷의 ID가 표시된다. 
- Destination : 복사할 리전이다. US West(N. California)를 선택한다.
- Description : 새로 만들어질 EBS 스냅샷의 설명이다. 기본값 그대로 사용한다. 

설정이 완료되었으면 복사버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137576161-0e4715cd-1d20-41ce-925b-e0a173562776.png)  
![image](https://user-images.githubusercontent.com/33191974/137576179-dd0b3684-3f11-4aff-a180-868a65e4b9ce.png)  
EBS 스냅샷 목록으로 이동했다.  
화면 맨위의 리전이 캘리포니아로 선택되어 잇는지 확인한다.  
방금 복사한 EBS 스냅샷이 보인다.  
완전히 복사되기까지 시간이 걸린다.(진행상황 참고)  
![image](https://user-images.githubusercontent.com/33191974/137576222-61bd6032-fcdd-4975-bb0a-4d0f8ea1701a.png)  
이제 이 EBS 스냅샷으로 캘리포니아 리전에서 EBS 볼륨이나 AMI를 생성하여 사용하면 된다.  

스냅샷이 필요없다면 지운다.  




