AWS 콘솔로 접속한 뒤 메인 화면에서 Deployment & Management의 OpsWorks를 클릭한다.   
생성한 OpsWorks 스택이 하나도 없을 때 아래 그림과 같은 페이지가 표시된다.    
먼저 스택을 생성하기 전에 OpsWorks 관련 IAM 역할을 생성해야 한다([참고](https://docs.aws.amazon.com/opsworks/latest/userguide/opsworks-security-servicerole.html  
OpsWorks를 사용하기 위해서 최소 필요한 권한을 설명하고 있음).       
<img src="https://user-images.githubusercontent.com/33191974/158520269-d1b5bfc9-9805-46c3-9c9d-1f42edcb5b3d.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158523883-932670dd-b18d-47f7-8471-3059c99b13dc.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158524058-0610fc35-96b1-407f-89b4-32e20d35d5b6.png" width="50%" height="50%"/>   
  
정책에 아래 정책을 넣는다.   
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:DescribeAlarms",
        "cloudwatch:GetMetricStatistics",
        "ec2:*",
        "ecs:*",
        "elasticloadbalancing:*",
        "iam:GetRolePolicy",
        "iam:ListInstanceProfiles",
        "iam:ListRoles",
        "iam:ListUsers",
        "rds:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "ec2.amazonaws.com"
        }
      }
    }
  ]
}
```
<img src="https://user-images.githubusercontent.com/33191974/158524121-06ab9d85-bf95-4d58-b91b-6c1fb71a8f66.png" width="50%" height="50%"/>    

<img src="https://user-images.githubusercontent.com/33191974/158520346-dbcaf606-cf05-4171-b2e5-9284534b4f7c.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158520391-4e1ca9a2-ef83-4f59-89b5-9ec199b35038.png" width="50%" height="50%"/>   
조금 전 선택한 정책을 선택한다.  
<img src="https://user-images.githubusercontent.com/33191974/158524331-b5a59197-bd11-447d-8262-59cc63cd88e7.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/158520550-de209e90-d565-4b43-81e7-f52676c0d56d.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/158513789-5321dc56-9afc-42ec-9000-68f3be973076.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158513932-ef9e41ba-98d2-45d2-ab23-4f300e7a8719.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/158516438-d89d313b-b263-4536-932b-e2b4e974ce84.png" width="50%" height="50%"/>  

OpsWorks 스택을 생성한다.   
- Region: EC2 인스턴스가 생성될 리전이다. 서울리전을 선택한다.   
- Name: 리전을 선택하면 스택 이름을 설정할 수 있다. ExampleStack을 입력한다. 
- VPC: EC2 인스턴스가 위치할 VPC이다. 기본값 그대로 사용한다.  
- Default subnet: EC2 인스턴스가 위치할 서브넷이다. 기본값 그대로 사용한다.  
- Default operating system: EC2 인스턴스에 설치될 운영체제이다. 기본값 그대로    
사용한다.  
- Default root device type: EC2 인스턴스의 Root 장치 유형이다. 기본값 그대로    
사용한다.  
- IAM role: OpsWorks의 IAM 역할이다. 기본값 그대로 사용한다.  
- Default SSH key: EC2 인스턴스에 접속할 때 사용할 키 쌍이다. 앞에서 생성한  
awskeypair를 선택한다.  
- Default IAM instance profile: EC2 인스턴스에 사용할 IAM 역할이다. 기본값    
그대로 사용한다.  
- Hostname theme: EC2 인스턴스에 이름을 붙이는 방식이다. 과일 이름, 태양계 행성  
이름등을 사용할 수 있다. 기본값 그대로 사용한다.   
- Stack color: 스택 상징 색이다. 기본값 그대로 사용한다.  
- Chef version: Chef 버전이다. 기본값 그대로 사용한다.  
- Use custom Chef cookbooks: OpsWorks에서 제공하는 Chef 쿡북 이외에 인터넷에   
공개된 Chef 쿡북이나 사용자가 작성한 Chef 쿡북을 사용하는 옵션이다. 여기서는   
OpsWorks에서 제공하는 Chef 쿡북을 사용할 것이므로 기본값 그대로 No를 선택한다.  
- Custom JSON: Chef 레시피에 넘겨줄 속성(Attribute) 값이다. 다음 코드를 입력한다.  
(Apache의 설정을 변경하는 예제이다).  
```
{
  "apache": {
    "keepalivetimeout": 5
  }
}
```
- Use OpsWorks security groups: OpsWorks용 Security Group을 사용하는 옵션이다.  
기본값 그대로 사용한다.   

<img src="https://user-images.githubusercontent.com/33191974/158520765-693fb5c6-900a-4086-a2fa-7d8f4286125c.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/158520812-04cba3dc-6826-4f43-af5e-a7376485c5bc.png" width="50%" height="50%"/>     
   
설정이 완료되었으면 Add Stack 버튼을 클릭한다.  
  
> #### Apache Chef 속성  
> Apache Chef 속성은 다음 링크를 참조하자.  
> https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/apache2/attributes/apache.rb  
   
OpsWorks 스택이 생성되었다.  
<img src="https://user-images.githubusercontent.com/33191974/158520950-13130de6-0581-401b-a5a0-a9ed154862a4.png" width="50%" height="50%"/>   





























