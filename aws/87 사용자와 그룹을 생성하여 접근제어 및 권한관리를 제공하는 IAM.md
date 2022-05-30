IAM의 기본 개념과 통합 결제 기능에 대해 알아보자. 그리고 IAM 사용자와 그룹을   
생성하는 방법, IAM 사용자로 AWS 콘솔에 접속하는 방법, IAM 역할을 생성하고, IAM  
역할을 사용하는 EC2 인스턴스를 생성하는 방법에 대해 설명한다.    
  
IAM은 Identity and Access Management(식별 및 접근 관리)의 약어로 사용자와 그룹을  
생성하고 AWS의 각 리소스에 대해 접근제어와 권한관리를 제공한다. IAM은 실제로   
서비스를 제공하는 AWS 리소스가 아니라서 사용 요금이 없다.   
    
IAM은 AWS 계정 안에 IAM 그룹과 사용자를 생성하여 접근제어 및 권한관리를   
세분화할 수 있다. 어떤 IAM 사용자는 EC2만 관리할 수 있고, 어떤 IAM 사용자는   
S3의 내용을 읽을 수만 있도록 구성할 수 있다. 따라서 전체 권한이 아닌 필요한  
권한만 주기 때문에 보안성이 높다. IAM 그룹은 동일한 권한을 여러 IAM 사용자에게   
적용시킬 때 사용한다.   
  
IAM 기본 개념도  
<img src="https://user-images.githubusercontent.com/33191974/157583107-1ba8a18a-086f-41d7-b359-bb1e375b1bf2.png" width="50%" height="50%"/>  
AWS 계정으로 AWS 콘솔 페이지에 접속하듯이 IAM 사용자도 AWS 콘솔에 접속할 수  
있다. IAM 사용자에게 특정 AWS 리소스에만 접근할 수 있도록 설정했다면 AWS   
콘솔에서도 허용된 AWS 리소스만 제어할 수 있다. 또한, IAM 계정은 액세스 키를  
별도로 생성할 수 있고, 이 액세스 키를 이용하여 AWS API를 사용할 수 있다.   
  
> #### IAM과 리전  
> IAM은 리전별로 설정할 수 없고 모든 리전에서 동일하게 작동한다.   
  
IAM은 소규모 벤처기업이나 스타트업뿐만 아니라 규모가 크고 인원이 많은 조직에서   
매우 유용하다. 아래 그림처럼 AWS 요금 결제와 제품 개발, 서비스 운영을 분리하여  
구성할 수 있다.   
  
AWS 요금 결제, 제품 개발, 서비스 운영을 부서별로 분리  
<img src="https://user-images.githubusercontent.com/33191974/157614563-3f3dcef6-bacb-47db-ad6c-224cf8f57694.png" width="50%" height="50%"/>    
  
AWS 계정에는 다른 AWS 계정의 요금을 합쳐서 지불하는 통합 결제(Consolidated  
Billing) 기능이 있다. 일반적인 회사에서는 비용 지출을 재무회계팀에서 최종적으로  
관리한다. 따라서 재무회계팀에서 AWS 통합 계정을 사용하여 회사 내 모든 부서의  
AWS 요금을 관리하고 지불할 수 있다.   
  
부서가 다를 때는 AWS 계정을 분리하여 구성하면 매우 편리하다. 제품개발부의   
개발용 EC2 인스턴스에 서비스 운영부의 사람들이 마음대로 접근해서는 안된다.   
이처럼 실제 조직의 특성에 막제 접근제어와 권한관리를 설정할 수 있다.   
  
> #### AWS 통합 결제  
> AWS 콘솔의 Billing & Cost Management에서 통합 결제 기능을 사용할 수 있다.   
> 결제를 하는 계정(Payer Account)이 AWS 리소스를 사용하는 계정(Linked Account)  
> 에 연결요청을 보내는 방식이다.   

IAM 그룹과 사용자에게 권한을 설정하는 것과는 달리 EC2 인스턴스 전용으로 권한을   
설정할 수 있다. 이 것을 IAM 역할(Role)이라고 한다. IAM 역할은 EC2 인스턴스뿐만   
아니라 다른 AWS 계정(또는 다른 AWS 계정의 IAM 사용자), Facebook, Google,   
Amazon.com 계정전용으로 권한을 설정할 수 있다.   
  
IAM 역할 기본 개념  
![image](https://user-images.githubusercontent.com/33191974/157616798-9c3c1054-d4f5-4e91-89f7-de8e9c0637c9.png)



  




  



























