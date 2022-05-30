## EBS 스냅샷 생성하기

EBS 스냅샷을 생성하는 방법은 2가지가 있다.  
EBS 볼륨 목록에서 생성하는 방법과 EBS 스냅샷 목록에서 생성하는 방법이 있다.  
두 방식 모두 위치만 다를 뿐 과정은 완전히 같으므로 EBS 볼륨 목록(ELASTIC BLOCK STOR  
-> Volumes)에서 생성하는 방법을 설명할 것이다.  
  
EBS 볼륨 목록에서 8GiB EBS 볼륨을 선택하고 마우스 오른쪽 버튼을 클릭하면  
팝업 메뉴가 나온다.  
![image](https://user-images.githubusercontent.com/33191974/137572518-eab50bda-1dcb-4c35-b03d-9f35272f95ff.png)  

EBS 스냅샷 생성 세부 설정이다.  
- Name : EBS 스냅샷 이름이다. Example Snapshot으로 입력한다.(입력하지 않아도 상관없다.)
- Description : EBS 스냅샷의 설명이다.(입력하지 않아도 상관없다.)
- Encrypted : 볼륨을 암호화했다면 Yes로 표시된다.   
![image](https://user-images.githubusercontent.com/33191974/137572632-6cd2c43e-0406-42fb-b354-f7f2f507e25f.png)  
스냅샷 아이디를 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137572687-b7d96d95-2c4d-460d-b1d0-89f3f53466db.png)  
EBS 스냅샷 목록으로 이동했다.  
이제 EBS 스냅샷이 생성되었다. 완전히 생성이 완료되기까지 약간 시간이 걸린다.   
Status가 completed이면 생성이 완료된 것이다.   
![image](https://user-images.githubusercontent.com/33191974/137572749-0de663c0-1721-451f-a82b-0ee87a26a666.png)





