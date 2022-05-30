## EBS 볼륨 생성하기
![image](https://user-images.githubusercontent.com/33191974/137570504-99539a9f-1935-42a1-898c-cb857f0f909d.png)  
현재 생성된 EBS 볼륨의 목록이 표시된다.  
이전에 EC2 인스턴스(Example Server)를 생성할 때 함께 생성한 EBS볼륨을 볼 수 있다.  
이 EBS 볼륨은 OS가 설치되어 있는 볼륨이다.  
위쪽에 볼륨생성 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137570615-eba7ddc6-481b-4020-bc7b-bfe50af34b0e.png)  
EBS 볼륨을 생성한다.  
- Type : EBS 볼륨 형태이다.  
- Size : EBS 볼륨 크기이다. 10GiB를 생성할 것이므로 10을 입력한다.  
- IOPS : Type을 General Purpose로 설정했기 때문에 IOPS를 설정할 수 없다.  
         Type을 Provisioned IOPS로 선택해야 이 값을 설정할 수 있다.  
- Available Zone : 볼륨이 생성될 가용 영역이다.   
                   EC2 인스턴스가 생성된 가용 영역과 같은 곳에 위치해야 EC2 인스턴스에서 사용  
                   할 수 있다. 이전에 생성한 EC2 인스턴스(Example Server)를 참고하여 같은 곳에  
                   생성한다.  
- Snapshot ID : 생성해놓은 EBS 스냅샷이 있다면 여기서 선택할 수 있다.  
                기본값 그대로 비워둔다.  
- Encryption : 볼륨 암호화 옵션이다. 기본값 그대로 사용한다. 

설정이 완료되었으면 볼륨생성 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137570994-9aaddb90-0d61-473f-9493-35048c987ba5.png)   
볼륨이 생성되었다.  
![image](https://user-images.githubusercontent.com/33191974/137571068-19df03af-1eea-4c45-bbc9-18ec191bed49.png)


         


