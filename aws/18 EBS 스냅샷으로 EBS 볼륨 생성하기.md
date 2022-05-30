## EBS 스냅샷으로 EBS 볼륨 생성하기
EBS 스냅샷으로 EBS 볼륨을 생성할 수 있다.  
EBS 볼륨 자체로는 다른 가용영역(Available Zone)으로 이전할 수 없으므로  
꼭 EBS 스냅샷을 활용해야 한다.  
백업 기능도 있으므로 EBS 볼륨의 내용을 되돌리고 싶을 때 EBS 스냅샷을 활용할 수 있다.  
  
EBS 스냅샷을 선택하고 마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다.  
![image](https://user-images.githubusercontent.com/33191974/137572840-ae94ca51-1f7f-4683-a5e7-9c0d8152a03a.png)  

EBS 스냅샷으로 EBS 볼륨을 생성한다.  
- Snapshot ID : 선택한 EBS 스냅샷의 ID이다.  
- Type : EBS 볼륨 형태이다. 기본값 그대로 사용한다.  
- Size : EBS 볼륨 크기이다. 기본값은 EBS 스냅샷을 생성했을 때의 크기이다.  
         기본값 그대로 사용한다.  
- IOPS : Type을 General Purpose로 설정했기 때문에 IOPS를 설정할 수 없다.  
         Type을 Provisioned IOPS로 선택해야 이 값을 설정할 수 있다.  
- Available Zone : 볼륨이 생성될 가용 영역이다. 이 기능을 활용하여 EBS 볼륨을 다른 가용영역으로  
                   옮길 수 있다. 기본값 그대로 사용한다.  
- Encryption : 볼륨 암호화 옵션이다. 기본값 그대로 사용한다.  

설정이 완료되었으면 볼륨 생성 버튼을 클릭한다. 

![image](https://user-images.githubusercontent.com/33191974/137573090-774a2d16-e845-4d53-bda1-c6bffb1395eb.png)
이제 EBS 볼륨이 생성되었다.  
![image](https://user-images.githubusercontent.com/33191974/137573182-6c05f826-9f47-4623-a6c9-ef8d85168802.png)

볼륨을 사용하지 않는다면 삭제한다. 
