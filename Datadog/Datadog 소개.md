## Datadog 소개

### 1. 기능 설명에 앞서, Datadog 사용에 필요한 배경지식
#### 1. Datadog의 namespace에 대한 이해
- Datadog을 사용할 때, 가령 필요한 정보를 가공하거나 특정 대상의 정보를 취득하고자 할 때  
  metrics를 읽는 방법을 이해하기 위해서는 Datadog의 namespace를 이해할 필요가 있다.
- 예를 들어, 보통 aws.ec2.cpuutilization과 같은 형식으로 표현되는데, 이 것이 의미하는 것은  
  AWS EC2의 CPU 점유율을 나타낸다.
- 주된 감시대상이 되는 AWS metrics 표기법은 아래 링크에서 확인가능하다.  
   - [AWS EC2 metrics](https://docs.datadoghq.com/integrations/amazon_ec2/#metrics)
   - [AWS RDS metrics](https://docs.datadoghq.com/integrations/amazon_rds/?tab=standard#metrics)
   - [AWS ELB metrics](https://docs.datadoghq.com/integrations/amazon_elb/#metrics)

#### 2. 정보수집 대상을 선택하는 법
- metrics를 선택했다면, 다음은 어떤 대상으로부터 해당 정보를 수집할 것인지 선택하게 된다.
- Datadog에서 제공하는 선택방법은 아래와 같이 다양하다
   - host 이름 : 예) host:i-xxxxxxxxx
   - instance 이름
   - region 이름
   - AZ 이름 : 예) availability-zone:ap-northeast-1a
   - target group 이름
   - security group 이름
   - AWS 등에서 설정한 tag
   - image 이름
   - 그 외 여러가지 방법

### 2. 몇가지 주요 기능들
다양한 기능들이 있지만, 이 번에 사용해본 기능들만 다룬다.

#### Dashboards
- 중요한 성능수치를 시각적으로 추적, 분석, 표시할 수 있다.
- Datadog에서 제공하는 디폴트 측량값을 사용하거나, 커스텀하여 대시보드를 만드는 것도 가능하다.
- 화면예시
![image](https://user-images.githubusercontent.com/33191974/139014663-f86692d4-1f29-4d97-9c72-7c40b88bac84.png)

#### Integrations
- Datadog은 현재 여러가지 서비스와 연계하여 모니터링 할 수 있다.
- 이번에 사용했던 도구는 다음과 같다.
|도구|목적|
|:-:|:-:|
|Amazon Web Services Integration|EC2나 RDS등 AWS의 서비스에 대한 시스템 정보를 수집|
|Slack|모니터링 결과를 통지하기 위해|
- AWS의 경우, 자동으로 설정하는 방법과 수동으로 설정하는 방법이 있는데  
   - 자동으로 설정하기를 하는 경우, CloudFormation(리소스를 자동으로 생성해주는 서비스  
     )을 통해서 설정이 되고
   - 수동인 경우, 감시 대상 호스트에 datadog-agent를 설치하여 몇 가지 설정을 통해 정보  
     수집이 가능해진다.
- 화면예시
![image](https://user-images.githubusercontent.com/33191974/139015481-c03198e5-aa1d-4d57-8572-8c36fbf3302f.png)  

#### Logs
- 로그 수집 및 모니터링 환경 구축 가능
   - 인프라 및 어플리케이션에서 발생한 대량의 로그를 효율적으로 수집, 처리, 저장, 검색,  
     모니터링할 수 있다.
- Logs의 설정(Pipelines)으로 수집한 로그에 대한 처리를 어떻게 할 것인지 정의할 수 있다.
- AWS의 CloudWatch를 사용하는 경우, CloudWatch에서 보내는 로그를 그대로 표시할 수도 있다.
- 단, 로그를 보기 좋은 형태로 가공하기 위해서는 몇가지 설정이 필요하다.
...
    
     

































