# 정리
- 키종류  
EC2 접속을 위한 키페어  
API와 툴 사용을 위한 액세스 키  

---

CloudFormation 템플릿으로 CloudFormation 스택을 생성해보자. 템플릿은 앞에서  
설명한 기본 템플릿을 조합하여 EC2 인스턴스를 생성하고, 22, 80번 포트를 여는  
Security Group을 설정하고, 아파치 웹 서버를 설치, 실행하는 템플릿을 사용한다.   
  
메모장이나 기타 텍스트 편집기를 열고 아래와 같이 템플릿을 작성한 뒤 httpd.template  
로 저장한다.   
  
EC2 인스턴스 생성, 22, 80번 포트를 여는 Security Group 설정, 아파치 웹 서버   
설치, 실행 템플릿(installhttpdsecuritygroup.template)  
```
{
  "Description": "Create an EC2 instance running the Amazon Linux 64 bit AMI.",
  "Parameters": {
    "KeyPair": {
      "Description": "The EC2 Key Pair to allow SSH access to the instance",
      "Type": "String",
      "Default": "2021-10-05 key"
    }
  },
  "Resources": {
    "Ec2Instance" : {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "KeyName": { "Ref": "KeyPair" },
        "ImageId": "ami-05f34794620202880",
        "SecurityGroups": [{ "Ref": "InstanceSecurityGroup" } ],
        "InstanceType": "t2.micro",
        "UserData": {
          "Fn::Base64": {
            "Fn::Join": ["",
              [ "#!/bin/bash\n",
              "/opt/aws/bin/cfn-init --region ", { "Ref": "AWS::Region" },
              " -s ", { "Ref": "AWS::StackName" },
              " -r Ec2Instance\n" ]
            ]
          }
        }
      },
      "Metadata" : {
        "AWS::CloudFormation::Init": {
          "config": {
            "packages": {
              "yum": {
                "httpd": []
              }
            },
            "services": {
              "sysvinit": {
                "httpd": {
                  "enabled": "true",
                  "ensureRunning": "true"
                }
              }
            }
          }
        }
      }
    },
    "InstanceSecurityGroup": {
      "Type": "AWS::EC2::SecurityGroup",
      "Properties": {
        "GroupDescription": "Allow HTTP and SSH access",
        "SecurityGroupIngress": [{
          "IpProtocol": "tcp",
          "FromPort": "22",
          "ToPort": "22",
          "CidrIp": "0.0.0.0/0"
        }, {
          "IpProtocol": "tcp",
          "FromPort": "80",
          "ToPort": "80",
          "CidrIp": "0.0.0.0/0"
        }]
      }
    }
  },
  "Outputs": {
    "InstanceId": {
      "Description": "The InstanceId of the newly created EC2 instance",
      "Value": {
        "Ref": "Ec2Instance"
      }
    }
  },
  "AWSTemplateFormatVersion": "2010-09-09"
} 
```
AWS 콘솔로 접속한 뒤 메인 화면에서 Deployment & Management의 CloudFormation을  
클릭한다.   
서울리전을 선택한다.  
<img src="https://user-images.githubusercontent.com/33191974/158123603-f3c8691a-980a-43fe-8529-73886d0e47d8.png" width="50%" height="50%"/>  
CloudFormation 템플릿으로 CloudFormation 스택을 생성한다.   
- Name: CloudFormation 스택의 이름이다. httpd를 입력한다.   
- Source: 사용할 CloudFormation 템플릿이다. Upload a template to Amazon S3를   
선택하고 파일 선택 버튼을 클릭한다.  
   - Select a sample template: AWS에서 제공하는 예제 템플릿이다.   
   - **Upload ad template to Amazon S3: 현재 컴퓨터에 있는 템플릿 파일을 S3에  
   올린 후 스택을 생성한다.**     
   - Specify an Amazon S3 Template URL: 템플릿 파일을 미리 S3 버킷에 올렸다면   
   S3 버킷에 저장된 템플릿 파일의 경로이다.    
<img src="https://user-images.githubusercontent.com/33191974/158124847-65fc2605-cb62-4313-a53d-606e1a7cdc76.png" width="50%" height="50%"/>    
방금 저장한 httpd.template 파일을 선택하고, 열기 버튼을 클릭한다. 파일을 선택  
했으면 아래쪽 Next 버튼을 클릭한다(나중에 S3 버킷 목록을 보면 템플릿 파일이   
저장된 S3 버킷이 생성되어 있다).   
<img src="https://user-images.githubusercontent.com/33191974/158125334-d856e3dd-96ea-40c4-970c-7891d3a25993.png" width="50%" height="50%"/>  
EC2 인스턴스에 사용할 키 쌍을 설정한다. 우리가 만든 템플릿에는 awskeypair라는  
키를 기본값으로 설정해 놓았다. 다른 키 쌍을 사용하고 싶으면 KeyPair 부분에 키 쌍  
이름을 입력하면 된다.   
  
CloudFormation 스택을 생성할 때 사용할 옵션이다(기본값 그대로 둔다).     
- Tags: CloudFormation 스택의 태그이다. 기본값 그대로 비워둔다.   
- Advanced: 추가 옵션이다. 이번에는 따로 설정하지 않고, 각 항목 설명만 한다.   
   - Notification options: CloudFormation 스택 이벤트가 발생하면 SNS로 알림을   
   보낸다.   
   - Timeout CloudFormation: 스택 생성을 시작하고 여기에 설정한 시간(분)이상   
   지나면 스택 생성에 실패한 것으로 보고, 모든 AWS 리소스와 설정을 되돌린다.   
   기본적으로 Timeout 값이 설정되어 있지 않으므로 스택 생성이 성공할 때까지   
   기다린다.   
   - Rollback on failure: CloudFormation 스택을 생성하다가 중간에 실패하면   
   AWS 리소스와 설정을 되돌린다.   
   - Stack policy: CloudFormation 스택 업데이트 정책이다. 실수로 생성된 스택의   
   설정을 변경하지 않도록 할 수 있다.  
Next 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/158126790-d4823862-5cbf-4f96-b51f-521e4b6d5b0f.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158126931-7862460c-da82-48fb-ac4c-f1d5631f788c.png" width="50%" height="50%"/>     
지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Create 버튼을   
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158127115-a86aaa30-e661-43c0-8819-a336f2c09d9b.png" width="50%" height="50%"/>   
CloudFormation 스택 목록에 CloudFormation 스택(httpd)이 생성되었다. Status를  
보면 CREATE_IN_PROGRESS로 나오며 AWS 리소스를 생성하고 설정하고 있다. 완전히   
생성되는 시간은 AWS 리소스의 종류와 개수에 따라 달라진다.   
<img src="https://user-images.githubusercontent.com/33191974/158127478-4a8bf5a3-c28a-4dc8-850c-78eb1afa368f.png" width="50%" height="50%"/>   
CloudFormation 스택 생성이 완료된 뒤 Output 탭을 클릭하면 생성된 EC2 인스턴스의   
ID가 표시된다. 템플릿에서 Output 부분에 설정한 내용이 이곳에 표시된다.   

> #### 리소스 생성에 실패했다면  
> ami ID, 키페어 이름(EC2 접속을 위한 키페어와 API와 툴 사용을 위한 키쌍은 다르다), 인스턴스 타입이 t2.micro인지 확인해보자    
  
<img src="https://user-images.githubusercontent.com/33191974/158133162-7b8a04ff-db52-48f7-a015-24a96e8f7ea1.png" width="50%" height="50%"/>  
EC2 인스턴스 목록에 CloudFormation이 생성한 EC2 인스턴스가 추가되었다.  
위 CloudFormation 스택 Outputs에 나온 인스턴스 ID와 같다. 그리도 아래  
세부 내용을 보면 Security groups에 템플릿에서 정의한 InstanceSecurity  
Group이 설정되었다.   
   
<img src="https://user-images.githubusercontent.com/33191974/158133720-874ed43c-73e5-4a9d-a258-7ac5004d2fb5.png" width="50%" height="50%"/>   
웹 브라우저를 실행하고 표시된 EC2 인스턴스의 Public IP에 접속한다.   
아파치 웹 서버(httpd)의 기본 웹 페이지 내용이 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/158134418-abaac586-4d3d-4441-93ed-cfe5268a198b.png" width="50%" height="50%"/>    
이처럼 CloudFormation 템플릿으로 AWS 리소스를 조합한 CloudFormation 스택을  
생성할 수 있다. CloudFormation 스택을 삭제하면 지금까지 생성된 AWS 리소스는  
모두 삭제된다. CloudFormation 스택에 포함된 EC2 인스턴스 등의 유형을 변경  
하거나 AWS 리소스의 설정을 변경하고 싶으면 스택 업데이트 기능을 사용하면   
된다. 템플릿의 내용을 수정한 뒤 Update Stack 버튼을 클릭하고 해당 템플릿을  
선택하면 된다. 설정 화면은 스택 생성과 동일하다. 예를 들면 CloudFormation  
스택으로 EC2 인스턴스를 100개 만든 뒤 EC2 인스턴스 유형을 변경하면, 새   
EC2 인스턴스 100개가 모두 생성되고 나서 기존 EC2 인스턴스 100개가 동시에   
삭제된다.  
  
**스택 업데이트 기능은 많은 AWS 리소스의 설정을 일괄적으로 변경할 때 유용하다.   
설정 한 두개를 변경할 때는 AWS 콘솔의 해당 AWS 리소스 페이지에서 직접 설정을   
변경해도 된다.**  





















   



























