## EC2 인스턴스에서 EBS 볼륨제거하기
EC2 인스턴스에서 사용하지 않는 EBS 볼륨을 제거해보자.  
sudo umount /mnt 명령을 입력하여 장치를 언마운트한다.  
![image](https://user-images.githubusercontent.com/33191974/137571840-b30b7c94-0027-42c3-8e10-454f0a59066a.png)  
df -h 명령을 입력해보면 /mnt 디렉터리가 사라졌다.  
![image](https://user-images.githubusercontent.com/33191974/137571852-3cc5965f-f12d-4060-937e-98ba7c1b5bd0.png)
EBS 볼륨 목록에서 10GiB 짜리 EBS 볼륨을 선택하고 마우스 오른쪽 버튼을 클릭하면 팝업 메뉴가 나온다.  
![image](https://user-images.githubusercontent.com/33191974/137571912-06592e9c-95fb-49ba-a18e-46cea82a46ac.png)
![image](https://user-images.githubusercontent.com/33191974/137571939-9f6367cb-0db2-4d1a-991c-f69f456fa1aa.png)  
EBS 볼륨 목록에서 State 항목을 보면 초록색 아이콘의 in-use에서 파란색 아이콘의 available로 바뀐 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/33191974/137571973-f61afc8f-fe04-4749-8cba-7f1c632dcac7.png)  
이제 EC2 인스턴스(Example Server)에서 EBS 볼륨이 완전히 제거되었다.  
앞으로 이 EBS 볼륨을 사용할 계획이 없다면 팝업 메뉴에서 볼륨삭제를 눌러 완전히 삭제할 수 있다.   
삭제한다.  

> #### EBS 볼륨과 RAID
> EBS 볼륨은 OS에서 봤을 때 하드디스크 또는 SSD와 똑같다.  
> 따라서 EBS도 RAID 구성을 할 수 있다.  
> EC2와 EBS 볼륨이 지원하는 RAID 타입은 RAID 0, RAID 1, RAID 1+0(RAID 10)이다.  
> RAID 5와 RAID 6은 충분한 성능이 나오지 않아 AWS에서는 권장하지 않는다.  



