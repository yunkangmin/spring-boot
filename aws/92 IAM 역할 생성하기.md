AWS 루트 계정으로 로그인한 뒤 IAM 역할 목록에서 위쪽 Create New Role 버튼을 
클릭한다.    
<img src="https://user-images.githubusercontent.com/33191974/157683960-09bd3b0a-1185-4fb3-93fe-d9d4f6731df0.png" width="50%" height="50%"/>   
IAM 역할을 생성한다.   
- Role Name: IAM 역할 이름이다. ExampleEC2Role을 입력한다.   

IAM 역할 유형이다. EC2 인스턴스용 IAM 역할을 생성하기로 했으므로 Amazon EC2의    
Select 버튼을 클릭한다.   
- AWS Service Roles: AWS 리소스 전용 IAM 역할이다. EC2, CloudHSM, Data Pipeline,  
Elastic Transcoder, OpsWorks 등을 선택할 수 있다.   
- Role for Cross-Account Access: 다른 AWS 계정 혹은 다른 AWS 계정에 속한 IAM  
사용자를 설정할 수 있다.   
- Role for Identity Provider Access: Facebook, Google, Amazon.com 계정과   
SAML(Security Asserting Language) 프로토콜을 지원하는 Identity Provider를   
설정할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157685692-97365a93-36cb-4bb1-91a1-4aa591447dd3.png" width="50%" height="50%"/>     
IAM 역할이 S3에만 접근할 수 있도록 설정해보자. Select Policy Template에는   
AWS의 모든 리소스에 대한 접근 권한을 Full Access, Read Only Access, 기타   
Access로 구분하여 준비해 놓았다. 개수가 상당히 많으므로 스크롤을 내려   
Amazon S3 Full Access의 Select 버튼을 클릭한다.    
  
AWS Policy Generator나 직접 정책(Custom Policy)을 작성하여 설정할 수도 있다.    
<img src="https://user-images.githubusercontent.com/33191974/157687119-8134bd6d-5951-4485-a4dc-4d6bf4949cdb.png" width="50%" height="50%"/>  
S3에만 접근할 수 있도록 해주는 정책 파일(Policy Document)이 자동으로 생성된다.   
Next Step 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157688601-94bd3daf-b816-4634-8485-734167d38853.png" width="50%" height="50%"/>   
  
지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Create Role  
버튼을 클릭한다.   

IAM 역할 목록에 IAM 역할(ExampleEC2Role)이 생성되었다. IAM 역할(ExampleEC2  
Role)을 선택한 뒤 세부 내용을 보면 Permissions에서 현재 설정된 정책이 표시된다.   
Attach Role Policy 버튼으로 정책을 계속 추가할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157689189-986ec06e-ff9a-4b22-b8b9-aa2c0a45b4d4.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/157689906-34b9de59-dee9-48c3-a5ae-396e4ac0698b.png" width="50%" height="50%"/>  































