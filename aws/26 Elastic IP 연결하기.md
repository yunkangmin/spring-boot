# 정리
Elastic IP를 삭제하려면 릴리스 버튼을 누른다.

---

## Elastic IP 연결하기
이제 Elastic IP를 사용할 수 있도록 EC2 인스턴스(Example Server)에 연결해보자.  
Elastic IP 목록에서 새로 할당 받은 Elastic IP를 선택하고 탄력적 IP 주소 연결 버튼을  
클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137580659-6c3008a7-b983-48f8-89da-aaa3edef15c8.png)  
![image](https://user-images.githubusercontent.com/33191974/137580679-f4e0e3cd-8016-4167-9596-8064627728c3.png)
- Instance : Instance 입력 부분을 클릭하면 현재 실행되고 있는 EC2 인스턴스의 목록이 표시된다.  
             이전에 만든 Example Server를 선택한다.  
- Network Interface : Network Interface 입력 부분을 클릭하면 현재 생성되어 있는 Network Interface가 표시된다.   
                      우리는 EC2 인스턴스에 연결하기로 했으므로 이 부분은 비워둔다.
- Private IP Address : 내부 사설 IP 주소이다. Instance에서 EC2 인스턴스를 선택하면 자동으로 설정된다.  
- Reassociation : 해당 Elastic IP가 이미 다른 곳에 연결되어 있는데도 다시 새로운 곳에 연결하려고 하면  
                  에러가 발생한다. Reassociation에 체크하면 다른 곳에 연결되어 있더라도 강제로 가져와서  
                  연결한다.

![image](https://user-images.githubusercontent.com/33191974/137580699-ef70c697-a143-465e-aea0-8da33b17353b.png)  
탄력적 IP 목록을 보면 EC2 인스턴스에 탄력적 IP가 연결되었다.  
![image](https://user-images.githubusercontent.com/33191974/137580892-ebee397e-acde-45a6-8a0a-993ab02b8257.png)  
EC2 인스턴스 목록에서도 앞에서 생성한 EC2 인스턴스를 선택하면 아래 세부 내용에 연결된   
탄력적 IP가 표시된다.(탄력적 IP를 연결하면 Pubilc IP도 탄력적 IP주소와 같은 IP주소로 설절된다.)  
![image](https://user-images.githubusercontent.com/33191974/137580969-096a094b-d772-4c3a-8763-17f36f5d0aa1.png)  
이제 이 공인 IP를 DNS 서버에서 도메인과 연결하거나 HTTP, SSH, RDP 등의 접속을 할 수 있다.  

