# 정리
- 환경 -> 애플리케이션을 실행하는 플랫폼  
- 애플리케이션 -> 사용자가 애플리케이션을 업로드하는 공간  
- 애플리케이션 버전 -> S3에 업로드된 소스  
  
---
  
Elastic Beanstalk에 사용자가 애플리케이션을 업로드하는 공간을 애플리케이션(  
Application)이라고 하고, 애플리케이션을 실행하는 플랫폼을 환경(Environment)  
이라고 한다. Node.js 애플리케이션과 환경을 생성하고, 간단한 문자열을 출력해  
보자.   
  
AWS 콘솔로 접속한 뒤 메인 화면에서 Deployment & Management의 Elastic Beanstalk을  
클릭한다.   
서울 리전을 선택한다.  
    
생성한 Elastic Beanstalk 애플리케이션이 하나도 없을 때 아래 그림과 같은   
페이지가 표시된다. 위쪽 Create New Application을 클릭한다(Select a Platform   
에서 플랫폼을 선택하고 Lauch Now 버튼을 클릭하면 옵션 설정없이 바로 생성된다).   
<img src="https://user-images.githubusercontent.com/33191974/158175777-29ba6b5f-d8b3-4e8b-bfc4-c5f1d2a26dcc.png" width="50%" height="50%"/>   
- Environment tier: 환경 종류이다. Web Server를 선택한다.   
   - Web Server: 인터넷에서 접속할 수 있는 웹 서버이다.   
   - Worker: 백그라운드 작업을 위한 환경이다. 이 환경은 인터넷에 연결되어  
   있지 않다. 워커와 웹 서버는 SQS(Simple Queue Service)로 데이터를 주고   
   받아야 한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158175916-e9c5d6e8-c1e3-4583-a60d-30d4f564e769.png" width="50%" height="50%"/>   
  
새 Elastic Beanstalk 애플리케이션을 생성한다.   
- Application name: 애플리케이션 이름을 설정한다. exampleapp을 입력한다.   
- Description: 애플리케이션의 설명이다. 입력하지 않아도 상관없다.  
  <img src="https://user-images.githubusercontent.com/33191974/158176884-2d20bfa2-7758-4962-86b8-5386d18bd74e.png" width="50%" height="50%"/>         
  
Elastic Beanstalk 환경의 URL을 설정한다. 웹 브라우저에서 이 URL에 접속하여  
애플리케이션을 사용하게 된다.   
- Environment name: 환경의 이름이다. 기본값 그대로 사용한다.   
- Environment URL: 환경의 URL이다. URL은 유일해야 하므로 Check availability  
버튼을 클릭하여 중복되지 않는지 확인한다. 중복되면 다른 URL을 입력해야 한다.  
- Description: 환경의 설명이다. 입력하지 않아도 상관없다.    
  <img src="https://user-images.githubusercontent.com/33191974/158177394-e36bf7ab-f5a5-4463-8a4e-6274dae04e61.png" width="50%" height="50%"/>    
  
애플리케이션을 실행할 환경(Environment)을 생성한다. 환경은 EC2 인스턴스,  
Auto Scaling, ELB 등을 조합한 플랫폼을 뜻한다.  
- Launch a new envinronment running this application: 애플리케이션이 실행될   
환경을 생성하는 옵션이다. 애플리케이션은 환경이 없으면 실행할 수 없으니,   
나중에라도 생성해야 한다. 이미 생성된 환경을 사용할 수도 있다.   
- Predefined configuration: 개발 언어이다. Node.js를 선택한다.   
   - Docker로 서버를 구축하고 배포하려면 Docker를 선택하면 된다.   
- Environment Type: 환경의 구성 방식이다. 기본값 그대로 사용한다.   
   - Load Balancing, autoscaling: 부하 분산과 자동 확장을 사용한다.   
   - Single Instance: EC2 인스턴스 하나만 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158177714-ac998f91-be7f-48fc-9a86-bd55a69886e9.png" width="50%" height="50%"/>    
   
실행될 애플리케이션의 소스를 업로드하거나 예제 애플리케이션을 사용한다.   
기본값 그대로 예제 애플리케이션을 선택한다.  
- Sample application: Elastic Beanstalk에서 제공하는 예제 소스이다(s3에 자동으로 업로드한다(버킷도 자동으로 생성)).   
- Upload your own: 사용자가 갖고 있는 소스를 업로드한다.   
- S3 URL: S3 버킷에 저장된 소스를 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/158177870-e8e21a99-3c26-4115-b03d-36368853f391.png" width="50%" height="50%"/>      
  
추가 리소스를 생성하고 설정한다.   
- Create an RDS DB Instance with this environment: RDS DB 인스턴스를 생성하는  
옵션이다. 이번에는 DB 인스턴스를 생성하지 않을 것이므로 기본값 그대로 체크하지  
않는다.   
- Create this environment inside a VPC: 환경을 다른 격리된 VPC에 생성하는   
옵션이다. 외부에서 접속할 수 없고 내부에서만 접속해야 할때 사용한다. VPC에  
VPN으로 연결하여 사내망을 구축할 때 활용하면 된다. 이번에는 인터넷에서 접속할   
수 있게 할 것이므로 기본값 그대로 체크하지 않는다.   
  <img src="https://user-images.githubusercontent.com/33191974/158169442-68d5647c-98e7-4510-badb-19e48d06d5fc.png" width="50%" height="50%"/>    
    
Elastic Beanstalk 환경의 세부 설정이다.  
- Instance type: EC2 인스턴스의 유형이다. 기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/158172933-9e354d70-6904-41da-a2e1-ab946d90e8d1.png" width="50%" height="50%"/>   
     
- EC2 key pair: EC2 인스턴스에 접속할 때 사용할 키 쌍이다. 앞에서 생성한 awskey  
pair를 선택한다.   
- Email address: 환경에서 주요 내용이 바뀌면 이메일로 내용을 받아본다. 이메일을    
입력하지 않아도 상관없다.   
- Application health check URL: ELB에서 웹서버(Node.js, 아파치 웹서버, Nginx)가   
정상적으로 실행되고 있는지 확인할 URL이다. 입력하지 않으면 / 경로를 사용한다.   
기본값 그대로 비워둔다.   
- Enable rolling updates: 단계적 업데이트를 사용하는 옵션이다. EC2 인스턴스의   
유형을 변경하거나 EC2 인스턴스를 교체할 때 EC2 인스턴스가 정지되므로 일시적으로  
서비스가 중단된다. 서비스가 중단되지 않도록 일부 EC2 인스턴스가 운영 중인 상태에서   
부분적으로 업데이트한다. 업데이트가 완료된 EC2 인스턴스에서 트래픽을 처리하고 남은   
EC2 인스턴스도 업데이트한다. 이 기능은 애플리케이션 배포와는 관련이 없다. 이번에는  
사용하지 않을 것이므로 기본값 그대로 비워둔다.     
- Cross zone load balancing: 여러 가용 영역에 EC2 인스턴스를 생성하여 로드  
밸런싱을 하는 옵션이다. 기본값 그대로 사용한다.    
  <img src="https://user-images.githubusercontent.com/33191974/158173305-af294ae4-06ae-4e12-9c70-01957cab7d4b.png" width="50%" height="50%"/>  
  <img src="https://user-images.githubusercontent.com/33191974/158173427-3bf3aa6a-1f9a-44b9-82a7-d369d87f30b5.png" width="50%" height="50%"/>  
  <img src="https://user-images.githubusercontent.com/33191974/158173560-54bd4dd5-152e-4a9f-8a3e-905a9511562b.png" width="50%" height="50%"/>   
   
- Connection dranining: Auto Scaling이 사용자의 요청을 처리 중인 EC2 인스턴스를   
바로 삭제하지 못하도록 방지하는 기능이다. 자세한 내용은 '부하 분산과 고가용성을   
제공하는 ELB'를 참조하기 바란다. 기본값 그대로 사용한다(현재는 안보임).     
- Connection draining timeout: Connection draining 대기 시간이다. 기본값 그대로   
사용한다(현재는 안보임).  
- Instance profile: EC2 인스턴스에서 사용할 IAM 역할이다. 미리 만들어놓은 IAM  
역할을 선택할 수도 있고 새로 생성할 수도 있다. 기본값 그대로 Create Default  
Profile을 선택한다(현재는 안보임).  
  
Elastic Beanstalk의 환경에 태그를 설정한다. 7개까지 생성할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/158174280-4bc8e7ac-519d-4cfe-ad38-0b94c22aee75.png" width="50%" height="50%"/>   
지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Launch 버튼을  
클릭한다.   
  
Elastic Beanstalk 애플리케이션과 환경 생성이 시작되었다. Health를 보면 Launching  
이라고 표시된다. 완전히 생성되기까지 약 5분 정도 소요된다.   
<img src="https://user-images.githubusercontent.com/33191974/158174519-d4493624-3bc3-44a4-ae16-ae18b4f2257f.png" width="50%" height="50%"/>   
  
Elastic Beanstalk 애플리케이션과 환경 생성이 완료되었으면 위쪽 `<환경 이름>`.  
elasticbeanstalk.com 링크를 클릭한다.   

<img src="https://user-images.githubusercontent.com/33191974/158178659-11ef9534-23d1-4f56-a8f7-2f3f43c10f95.png" width="50%" height="50%"/>  
웹 브라우저에서 Elastic Beanstalk URL에 접속하면 예제 애플리케이션의 내용이  
표시된다.    
  
<img src="https://user-images.githubusercontent.com/33191974/158180795-31d8dba7-bc18-4bb5-a75e-dc3e2e67019b.png" width="50%" height="50%"/>   
  
> Elastic Beanstalk과 EC2 인스턴스 Elastic Beanstalk으로 애플리케이션과 환경을   
> 생성하면 EC2 인스턴스 목록에 Elastic Beanstalk이 생성한 EC2 인스턴스가 추가되어  
> 있다. 이 EC2 인스턴스는 삭제하거나 설정을 변경하지 않는 것이 좋다. 삭제하더라도   
> Elastic Beanstalk이 다시 생성한다.   

> #### Elastic Beanstalk 환경, 애플리케이션, S3 버킷에 저장된 애플리케이션 버전 삭제   
> Elastic Beanstalk 환경, 애플리케이션, 애플리케이션 버전은 따로 삭제해야 한다.   
> 환경을 모두 삭제하더라도 애플리케이션과 S3 버킷에 저장된 애플리케이션 버전은   
> 남아 있다.   
> - 환경삭제  
> 1. 삭제할 환경에서 Actions 버튼을 클릭하면 팝업 메뉴가 나온다.  
> 2. Terminate Environment를 클릭한다.  
> 3. Terminate 버튼을 클릭한다.   
>
> - 애플리케이션 버전 삭제: 애플리케이션을 삭제하기 전에 애플리케이션 버전을   
> 먼저 삭제해야 한다. 그렇지 않으면 애플리케이션이 저장된 S3 버킷이 계속 남아  
> 있게 된다.   
> 1. Elastic Beanstalk 애플리케이션 목록에서 Actions 버튼을 클릭하면 팝업 메뉴가   
> 나온다.  
> 2. View Application Versions를 클릭한다.  
> 3. 애플리케이션 버전 목록에 표시된 버전들을 모두 선택한다.  
> 4. Delete 버튼을 클릭한다.   
> 5. Delete versions from Amazon S3를 체크한다.   
> 6. Delete 버튼을 클릭한다.   
> 7. Done 버튼을 클릭한다.   
>
> - 애플리케이션 삭제
> 1. Elastic Beanstalk 애플리케이션 목록에서 Actions 버튼을 클릭하면 팝업  
> 메뉴가 나온다.  
> 2. Delete Application을 클릭한다.  
> 3. Delete 버튼을 클릭한다.  
>
> - 애플리케이션을 먼저 삭제하여 남겨진 S3 버킷(애플리케이션 버전이 저장된)을  
> 삭제하는 방법
> 1. S3 버킷 목록으로 이동한다.
> 2. S3 버킷 목록에서 애플리케이션 버전이 저장된 S3 버킷을 클릭한다.
> 3. S3 버킷에 저장된 모든 파일을 삭제한다.   
> 4. 위쪽 Properties 버튼을 클릭한다.   
> 5. Permissions 탭을 클릭한다.  
> 6. Bucket Policy Editor 창에서 Delete 버튼을 클릭한다.
> (elasticbeanstalk에 의해 생성된 버킷은 기본적으로 삭제 권한이 없다. 정책 자체를  
> 삭제해주고 버킷 삭제를 해주면 깔끔히 삭제된다. 버킷 정책에서 삭제를 막아놨다.)    
> 7. 확인 창에서 확인 버튼을 클릭한다.   
> 8. All Buckets 링크를 클릭하여 버킷 목록으로 이동한다.   
> 9. 삭제할 S3 버킷을 선택하고, 위쪽 Actions 버튼을 클릭하면 팝업 메뉴가 나온다.   
> 10. Delete를 클릭한다.
> 11. 확인 창에서 확인 버튼을 클릭한다.  







  





























