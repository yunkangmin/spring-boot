이제 새로운 VPC를 추가해보자. AWS 콘솔로 접속한 뒤 메인 화면에서 Compute & Networking의   
VPC를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158048815-db98c6dd-133f-4a71-bf97-305e52343ff9.png" width="50%" height="50%"/>   
서울 리전을 선택한다.  
VPC 목록에서 위쪽 Create VPC 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158049164-ac07a90b-bf03-4f30-8860-bf5df5fc5d55.png" width="50%" height="50%"/>    
VPC를 생성한다.   
- Name tag: VPC의 이름이다. ExampleVPC를 입력한다(입력하지 않아도 상관없다).  
- CIDR block: CIDR 표기법으로 된 IP 대역이다. 192.168.0.0/16을 입력한다.   
   - 192.168.0.0/16의 서브넷 마스크는 255.255.0.0이며 이진수로 바꾸었을 때 1이 16개이다.   
   그래서 192.168.0.0 ~ 192.168.255.255를 뜻한다.   
- Tenancy: 이 VPC에서 EC2 인스턴스를 생성할 때 전용 하드웨어 사용 옵션이다. 기본값 그대로 사용한다.   
설정이 완료되었으면 Yes, Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158049581-af70c311-6582-4fbe-a72d-34d6b648a05e.png" width="50%" height="50%"/>   

> #### 사설망   
> IPv4 주소에서 공인 IP는 개수가 적기 때문에 사용하려면 반드시 구입을 해야 한다. 하지만, 사설망(내부망)을   
> 구성할 때는 공인 IP를 구입하지 않아도 된다. 사설망을 구성할 때 임의로 설정해서 사용하는 IP 주소가   
> 사설 IP 주소이다.   
> 10.0.0.0 ~ 10.255.255.255, 172.16.0.0 ~ 172.31.255.255, 192.168.0.0 ~ 192.168.255.255 3가지가  
> 사설망으로 사용할 수 있는 IP 대역이다. 특히 192.168.0.x는 흔히 볼 수 있는 사설 IP 주소이다.   

VPC 목록에 VPC(ExampleVPC)가 생성되었다.   
<img src="https://user-images.githubusercontent.com/33191974/158049704-98f59a6c-ad07-4da0-b0e3-7b30aaf42c3a.png" width="50%" height="50%"/>  




































