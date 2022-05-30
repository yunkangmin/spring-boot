## Security Group으로 방화벽 설정하기
Security Group은 EC2 인스턴스에 적용할 수 있는 방화벽 설정이다.  
서버 보안의 기본은 방화벽 설정에서 출발한다.  
예로, Linux 서버의 SSH 접속 포트인 22번만 여는 것은 기본이고,  
여기에 접속 가능한 IP 대역까지 설정하면 공격 위협이 상당수 줄어든다.  
![image](https://user-images.githubusercontent.com/33191974/137578304-91f58b71-8c79-4c8d-a6a8-e17227531059.png)  
방화벽 설정의 기본요소는 다음과 같다.  
- Inbound: 외부에서 EC2 인스턴스로 들어오는 트래픽이다.  
  대표적인 것들로는 HTTP, HTTPS, SSH, RDP등이 있다.  
- Outbound : EC2 인스턴스에서 외부로 나가는 트래픽이다.  
             EC2 인스턴스 안에서 인터넷을 사용할 경우 Outbound라 할 수 있다.  
             대표적으로 파일을 다운로드하거나, 외부 SSH로 접속하는 것 등이 있다.  
- Type : 프로토콜 형태이다. 프로토콜은 크게 TCP, UDP, ICMP로 나눌 수 있다.  
- Port, Port Range : 포트 번호이다. TCP, UDP 프로토콜은 0 ~ 65535 사이의 포트번호를 사용하게 된다.  
                     (ICMP는 포트 번호를 사용하지 않는다.) 우리가 익히 알고 있는 HTTP는 80번  
                     SSH는 22번처럼 각 서버 애플리케이션들은 고유의 포트번호를 사용하고 있다.  
- Source/Destination : 연결 혹은 접속 가능한 IP 대역을 뜻한다.  
                       Inbound일 경우 Source, Outbound일 경우 Destination이라 부른다.  
                       IP 주소 하나만 지정할 수도 있고, CIDR 표기 방법을 이용하여 일정한 대역을  
                       설정할 수 있다.  
- Rule : 지금까지 설명한 Inboud, Outbound, Type, Port, Source/Destination을 조합한 것을 Rule(규칙)이라 한다.  

아래 그림처럼 AWS 콘솔의 EC2 페이지에서 왼쪽 보안그룹을 클릭하면 현재 생성된 Security Group 목록이  
표시된다. Group Name이 default인 것은 기본적으로 제공되는 Security Group이며 Inbound는 동일한  
Security Group인 default에만 허용되어 있고, Outbound는 모든 접속이 허용되어 있다.   
launch-wizard-1은 EC2 인스턴스(Example Server)를 만들 때 함께 만들어진 것이다.  
  
Launch-wizard-1에 Inbound 탭을 보면 SSH, TCP, 22, 0.0.0.0/0으로 설정된 규칙을 볼 수 있다.  
이 것은 모든 IP에서 EC2 인스턴스로 SSH 접속을 허용하겠다는 규칙이다.  
이 규칙이 있었기 때문에 우리가 PuTTY 또는 ssh명령으로 접속을 할 수 있었다.  
![image](https://user-images.githubusercontent.com/33191974/137579143-ec3bc188-2168-444f-992c-0e5571aa89df.png)  
Inbound 규칙을 설정하는 창이 표시된다.  
아래 Add Rule 버튼을 클릭한다.  
![image](https://user-images.githubusercontent.com/33191974/137579201-cb821e44-b3c1-43de-9e3b-93ca54271ddf.png)  
아래 그림처럼 규칙을 설정하는 부분이 한 줄 더 추가되었다.  
- Type : HTTP를 선택하면 자동으로 Protocol은 TCP, Port Range는 80으로 설정된다.  
- Source : 기본값 그대로 Anywhere를 사용한다.(HTTP 접속은 보통 모든 IP 주소에서 접속할 수 있도록  
           허용한다.)Custom IP를 선택하면 CIDR 표기법을 사용하여 IP 주소 대역을 설정할 수 있다.  
           My IP를 선택하면 현재 사용하고 있는 컴퓨터 혹은 공유기의 공인 IP를 자동으로 알아내서  
           설정해준다.    
![image](https://user-images.githubusercontent.com/33191974/137579413-ae64e453-d644-4560-a2d9-af807a4b8b09.png)
![image](https://user-images.githubusercontent.com/33191974/137579449-3f3bd266-d5ad-4bfb-b76a-e9e2f2c8772a.png)

> #### CIDR 표기법
> CIDR은 Classess Inter-Domain Rounting의 약어로 IP 주소 할당 방법이다.  
> 급격히 부족해지는 IPv4 주소를 보다 효율적으로 사용하기 위해 CIDR 표기법을 사용한다.  
> xxx.xxx.xxx.xxx/yy 형태로 표기하는데 맨 뒤의 yy는 Subnet Mask를 2진수로 바꾸었을 때  
> 1의 개수이다.  
> 255.255.255.0을 2진수로 바꾸면 11111111.11111111.11111111.00000000이 된다.  
> CIDR 표기법으로는 xxx.xxx.xxx.xxx/24가 된다.  
> 192.168.0.0/24라면 192.168.0.1부터 192.168.0.254까지라는 의미이다.  
> (192.168.0.0는 네트워크 192.168.0.255는 브로트캐스트).  
> 192.168.0.15/32이면 1이 32개이고 Subner Mask가 255.255.255.255가 되므로  
> 192.168.0.15 한개의 IP만 표현하게 된다.  
> 참고 https://dev.classmethod.jp/articles/vpc-3/

> #### 방화벽 설정은 항상 체크
> EC2 인스턴스를 생성하고 열심히 여러가지 서버들을 설치했는데  
> 외부에서 접속이 되지 않을 때가 많다. 이럴 때에는 먼저 Security Group 설정을 확인한다.  
> 설치된 서버들이 사용하는 포트가 Inbound 설정에서 열려 있는지, 포트 번호가 잘못 설정되지는  
> 않았는지, 프로토콜이 잘못 설정되지는 않았는지, Inbound 설정을 Outbound에다가 하지는 않았는지  
> 살펴보자.  


           



































