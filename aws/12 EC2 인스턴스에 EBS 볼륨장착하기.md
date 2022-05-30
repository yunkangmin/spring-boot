## EC2 인스턴스에 EBS 볼륨 장착하기
방금 생성한 10GiB EBS 볼륨을 선택하고 마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다.  
볼륨연결을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137571152-8ceb108e-8ea8-4a82-9762-e9a51370cc86.png)  

EBS 볼륨을 EC2 인스턴스에 장착한다.  
- Volume : 볼륨 ID와 생성된 가용 영역을 보여준다.  
- Instance : Instance 입력 부분을 클릭하면 현재 가용 영역에 생성된 EC2 인스턴스의  
             목록을 보여준다. 이전에 생성한 EC2 인스턴스(Example Server)를 선택한다.  
- Device : EC2 인스턴스를 선택하면 자동으로 설정된다. 기본값 그대로 사용한다.  

![image](https://user-images.githubusercontent.com/33191974/137571268-ab26851d-40c5-4a58-bb02-7689c6ff58a5.png)
![image](https://user-images.githubusercontent.com/33191974/137571301-a0649862-7b28-4af7-a12e-cb88c16508cd.png)
EBS 볼륨 목록에서 State 항목을 보면 파란색 아이콘의 available에서 초록색 아이콘의 in-use로 바뀐 것을  
확인할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/137571350-560b7257-6102-4e48-b703-ae4cd625028d.png)

           
           
             
