# 정리  
- 스냅샷  
EBS에서 특정시점을 파일로 저장한 형태가 스냅샷     
  
- AMI   
EC2 인스턴스를 생성할 수 있는 AMI  
ami(Amazon Machine Images)는 EC2 인스턴스를     
생성하기 위한 기본파일이다. AWS에서는 빈 EC2 인스턴스에 직접 OS를 설치할 수는 없다.     
그렇기 때문에 미리 OS가 설치된 AMI를 이용하여 EC2 인스턴스를 생성하게 된다.   
   
---

CloudFormation의 기본 개념에 대해 알아보고, 템플릿 기본 구조를 살펴보자. 그리고 EC2 인스턴스를   
생성하는 템플릿, 웹 서버를 설치하는 템플릿, Security Group을 설정하는 템플릿을 작성해보자. 또한,  
작성한 템플릿으로 CloudFormation 스택을 생성하는 방법에 대해 알아보자.   
  
CloudFormation은 미리 만든 템플릿을 이용하여 AWS 리소스 생성과 배포를 자동화한다. CloudFormation은  
실제로 서비스를 제공하는 AWS 리소스가 아니라서 사용 요금이 없다. EC2 인스턴스 1,000개를 생성하고   
웹 서버를 설치한 뒤 소스 코드를 복사하여 웹 서비스를 시작해야 한다면 어떻게 해야 할까? AMI 이미지를   
만들어 놓고 EC2 인스턴스를 생성하면 된다. 하지만, 소스 코드가 바뀔 때마다 AMI 이미지를 만들어야 한다.   
그리고 EC2 인스턴스뿐만 아니라 EBS 볼륨과 S3 버킷, RDS 인스턴스, ELB 로드 밸런서를 함께 생성해야 한다면   
AMI로는 불가능하다.  
  
서비스에 필요한 EC2 인스턴스, EBS 볼륨, S3 버킷, RDS 인스턴스를 미리 구성한대로 자동 생성하는 기능이  
CloudFormation이다. CloudFormation에서 지원하는 AWS 리소스는 다음과 같다.   
- Auto Scaling
- RDS
- CloudFront
- Redshift
- CloudWatch
- Route 53
- DynamoDB
- S3
- EC2
- SimpleDB
- ElastiCache
- SNS
- Elastic Beanstalk
- SQS
- Elastic Load Balancing
- VPC
- IAM

사용자가 구성한 시스템 구조를 CloudFormation 템플릿으로 만들면 복잡하고 반복적인 작업을 간단하게  
처리할 수 있다. 템플릿 파일은 JSON(JavaScript Object Notation) 형식의 텍스트 파일이다. AWS에서   
제공하는 미리 만들어진 템플릿을 사용할 수도 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158059155-9d21e14c-0148-4851-8e3d-e84d9d141d3e.png" width="50%" height="50%"/>    
CloudFormation 템플릿은 AWS 리소스 생성 및 설정뿐만 아니라 EC2 인스턴스에 소프트웨어를 설치하고  
설정할 수 있다. 소프트웨어 설치 자동화에 Chef, Puppet, cloud-init을 지원한다(Chef와 Puppt은 cfn-init을  
통해 실행된다).  
  
> #### Chef, Puppet, cloud-init   
> Chef와 Puppet은 루비로 작성된 오픈 소스 자동화 솔루션이다. 소프트웨어 설치, 설정, 빌드, 배포를   
> 자동화한다.   
> cloud-inif은 클라우드 인스턴스의 초기화를 위한 스크립트이다. Ubuntu Linux를 만든 캐노티컬에서   
> 개발했다.   

<img src="https://user-images.githubusercontent.com/33191974/158059349-69757df7-81ed-40e1-b9fb-e4bda78a54e6.png" width="50%" height="50%"/>   
CloudFormation 템플릿으로 생성한 AWS 리소스 조합을 CloudFormation 스택(Stack)이라고 한다.   
이 스택을 삭제하면 관련된 AWS 리소스도 모두 삭제된다.  
<img src="https://user-images.githubusercontent.com/33191974/158059434-f5d85e18-5757-41d6-b579-bb7da26ad46f.png" width="50%" height="50%"/>   




































