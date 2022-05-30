## Elastic IP 할당받기
이제 Elastic IP를 하나 할당받아보겠다.  
AWS 콘솔의 EC2 페이지에서 왼쪽 탄력적 IP를 클릭한다. 
![image](https://user-images.githubusercontent.com/33191974/137580379-f0049598-9b04-4f03-baa4-14dc7d9a8f47.png)
![image](https://user-images.githubusercontent.com/33191974/137580422-b6e07336-cbfa-4fc9-b5f6-1c96010e12c3.png)
Elastic IP 목록에서 새로 할당된 IP주소를 확인할 수 있다.  
이 상태로는 아무것도 할 수 없고, 그대로 두면 요금이 계속부과된다.  
EC2 인스턴스 등 AWS 리소스에 꼭 연결(Associate)해야 Elastic IP를 사용할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/137580438-f3ccad48-939a-4659-8db6-bfd614f369bb.png)
