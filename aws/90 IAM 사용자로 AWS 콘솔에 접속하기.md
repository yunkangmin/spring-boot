AWS 계정뿐만 아니라 IAM 사용자도 AWS 콘솔에 접속할 수 있다. IAM 사용자가   
AWS 콘솔에 접속하려면 비밀번호를 설정해야 한다. IAM 사용자 목록에서 IAM   
사용자(exampleuser1)를 선택한다. 그리고 Security Credentials 탭을 클릭하고   
Manage Password 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157675917-b7459eee-c8b8-48a3-a455-7dd9a106c69f.png" width="50%" height="50%"/>   
IAM사용자의 비밀번호를 설정한다. Assign a custom password를 선택하고 사용할   
비밀번호를 입력한 뒤 Apply 버튼을 클릭한다(Assign an auto-generated password를  
선택하면 비밀번호가 자동으로 생성된다).  
<img src="https://user-images.githubusercontent.com/33191974/157676505-3804aaad-8c1b-42bc-9fa7-dcc3920ec56e.png" width="50%" height="50%"/>  
IAM 사용자(exampleuser1)의 Sign-In Credential -> Password에 Yes로 표시되었고  
비밀번호가 설정되었다.   
<img src="https://user-images.githubusercontent.com/33191974/157676889-48bdde2d-0bc5-4b8d-892c-2476eb55992e.png" width="50%" height="50%"/>     
IAM 사용자용 AWS 콘솔의 주소는 IAM 메인 페이지에 표시되어 있다. 왼쪽   
메뉴에서 Dashboard를 클릭하고, IAM users sign-in link를 확인한다.   
<img src="https://user-images.githubusercontent.com/33191974/157677426-de0d56d0-a222-45ba-87e6-2d56fb4c0bd0.png" width="50%" height="50%"/>   
IAM 사용자용 AWS 콘솔의 주소는 다음과 같은 형식이다.   
```
https://<AWS 계정 ID>.signin.aws.amazon.com/console
```
Customize를 클릭하면 AWS 콘솔 주소의 앞 부분을 숫자로 된 AWS 계정 ID 대신   
회사 이름등의 문자열로 사용할 수 있다.  
  
웹 브라우저에서 IAM 사용자용 AWS 콘솔에 접속한다. 사용자 이름에 exampleuser1을  
입력하고 암호에 IAM 사용자의 비밀번호를 입력한 후 로그인 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/157678629-f645efa9-dd5b-4d91-822a-718ce1dceeff.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/157678706-232e9075-0fae-458c-8796-87cd3c715f1d.png" width="50%" height="50%"/>     
AWS 콘솔에 접속하면 맨 위 이름 표시 부분에 자신의 이름 대신 'IAM 사용자@AWS  
계정 ID'가 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/157678934-16d988f8-4139-40fd-961a-974e72649bc5.png" width="50%" height="50%"/>  
이제 각 AWS 리소스의 페이지로 이동하는 방법은 익숙해졌을 것이다. EC2 페이지로   
이동한다. IAM 그룹을 생성할 때 Amazon EC2 Full Access 권한을 설정했기 때문에  
EC2 인스턴스 목록(INSTANCES -> Instances)에서 앞에서 생성했던 EC2 인스턴스  
(Example Server)를 볼 수 있고, EC2 인스턴스를 제어할 수 있다.   
  
S3 페이지로 이동한다. 앞에서 IAM 사용자에 Amazon S3 Full Access 권한을   
설정했기 때문에 S3도 정상적으로 사용할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/157680130-a02aefec-bc08-4047-907f-ae09b27838db.png" width="50%" height="50%"/>     
마지막으로 RDS 페이지로 이동한다. RDS DB 인스턴스 목록(Instances)에서 데이터  
베이스 생성 버튼을 클릭하면 에러 메시지가 표시된다. 앞에서 IAM 그룹과   
사용자에 RDS 접근권한을 설정하지 않았기 때문에 RDS는 사용할 수 없다.   
<img src="https://user-images.githubusercontent.com/33191974/157680779-532c60e1-26d1-4194-8a8d-8625ffc23b66.png" width="50%" height="50%"/>    
  
> #### `<Note>` IAM 사용자와 AWS API   
> AWS 콘솔과 마찬가지로 IAM 사용자의 액세스 키로 AWS API를 사용하면 허용된   
> AWS 리소스에만 접근할 수 있다. 허용되지 않은 AWS 리소스는 AWS API를 사용할  
> 수 없다.   








































