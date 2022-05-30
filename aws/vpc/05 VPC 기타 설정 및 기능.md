VPC는 앞서 설명한 주요 기능들 이외에도 다양한 기능을 제공한다. 이 부분은 간략한 설명으로 대신한다.   
- Route Tables: VPC의 라우팅 테이블을 생성하고 설정한다. 
- DHCP Option Sets: VPC의 DHCP(Dynamic Host Configuration Protocol) 옵션을 생성하고 설정한다.   
- Elastic IPs: 고정 IP인 Elastic IP를 할당받고 연결한다. '고정 IP를 제공하는 Elastic IP'를 참조하기   
바란다.   
- Peering Connections: VPC와 VPC를 연결하는 기능이다. 내 AWS 계정의 VPC끼리 연결할 수도 있고, 다른   
AWS 계정의 VPC와 연결할 수 있다.   
- Network ACLs: 서브넷 안의 IP 주소 사이에, 다른 서브넷의 IP 주소(IP 대역)와 접근을 제어하는 방화벽   
설정 기능이다.   
- VPN Connections: VPN 연결을 위한 Customer Gateway, Virtual Private Gateway, VPN Connection을   
생성한다.   
  
> #### EC2 인스턴스에 VPN 구축  
> VPN 하드웨어 장비나 서버를 구축하지 않고 개인 PC에서 VPN을 사용하려면 AWS Marketplace의 OpenVPN  
> AMI를 사용하면 된다(따로 Linux나 Windows에서 PPTP 서버를 구축해도 된다). 이 때는 VPC에 VPN으로   
> 연결하는 것이 아니라 EC2 인스턴스에 VPN으로 연결하는 것이지만 효과는 동일하다.   


























