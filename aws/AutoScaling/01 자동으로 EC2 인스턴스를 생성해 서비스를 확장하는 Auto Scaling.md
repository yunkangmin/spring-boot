Auto Scaling의 기본 개념에 대해 알아보자. 그리고 Auto Scaling에 사용할 AMI를 생성하는 방법,   
EC2 인스턴스 생성 옵션을 설정하는 방법, Auto Scaling 그룹을 생성하고 CloudWatch 측정치를 설정하는   
방법, ELB 로드 밸런서와 연동하는 방법에 대해 설명한다.   
  
Auto Scaling은 트래픽이 늘어나면 자동으로 EC2 인스턴스를 생성해 서비스를 확장하는 기능이다.   
Auto Scaling은 실제로 서비스를 제공하는 AWS 리소스가 아니라서 사용 요금이 없다. Auto Scaling을   
사용하면 서비스가 잘돼서 트래픽이 폭주할 때도 손쉽게 대처할 수 있다. 사용자가 많지 않은 새벽 시간에는   
EC2 인스턴스의 개수를 줄여 비용을 절감할 수 있다. Auto Scaling은 클라우드 서비스이기 때문에 가능한   
것이며 AWS의 대표적인 기능이다.   
  
트래픽 폭주 상황과 하루 동안 사용량에 따른 인스턴스 그래프   
<img src="https://user-images.githubusercontent.com/33191974/158008405-f7baeee7-f8e0-4a02-b204-30639523eb91.png" width="50%" height="50%"/>   
  
보통 Auto Scaling은 ELB(Elastic Load Balancing)와 함께 사용한다. Auto Scaling은 생성한 EC2 인스턴스를   
ELB 로드 밸런서에 연결하고, ELB 로드 밸런서는 새로 생성된 EC2 인스턴스에 트래픽을 분산한다.   
  
Auto Scaling은 CloudWatch와 연동하여 EC2 인스턴스의 CPU 사용률, 네트워크 사용량이 늘어났을 때  
EC2 인스턴스를 생성하고, CPU 사용률, 네트워크 사용량이 줄어들면 EC2 인스턴스를 삭제한다. CPU 사용률,   
네트워크 사용량뿐만 아니라 CloudWatch에서 지원하는 모든 측정치(Metric)와 연동할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158008797-528cb8c7-35d0-40bf-b779-a04b8ea175c4.png" width="50%" height="50%"/>     

  
  
























































