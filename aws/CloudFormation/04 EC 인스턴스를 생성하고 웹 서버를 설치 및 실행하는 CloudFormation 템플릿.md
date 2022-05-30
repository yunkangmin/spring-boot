다음은 EC2 인스턴스를 생성하고 아파치 웹 서버를 설치, 실행하는 템플릿이다.   
이제 템플릿이 점점 복잡해지고 있다. 그래서 앞에서 설명한 부분은 생략하고 새로  
추가된 부분만 설명한다.   
  
- Ec2Instance -> Properties -> UserData: EC2 인스턴스가 생성된 직후 실행할   
스크립트이다. 예제에서는 bash 스크립트로 작성했다. /opt/aws/bin/cfn-init은   
CloudFormation에서 기본적으로 제공하는 Bootstrap 스크립트이다. 여기서 cfn-init을   
실행해야 아래 packages에서 정의한 패키지를 설치할 수 있고, services에서 정의한   
패키지를 실행할 수 있다. cfn-init의 -r 옵션에는 생성될 EC2 인스턴스의 리소스   
ID를 지정해야 한다. 리소스 ID는 Resources의 Ec2Instance와 같이 사용자가 원하는   
대로 설정할 수 있다.   
- Ec2Instance -> Metadata -> AWS::CloudFormation::Init -> config:  
   - package: cfn-init이 설치할 패키지이다. apt, rpm, yum, python, rubygems와   
   Windows의 msi 패키지를 지원한다. 예제에서는 yum을 이용하여 아파치 웹 서버인  
   httpd를 설치한다.   
      - "httpd": [2.4.9]처럼 패키지 버전을 설정할 수 있다.   
      - "rpm": { "epel": http://download.fedoraproject.org/pub/epel/5/i386/epel-  
      release-5-4.noarch.rpm }처럼 패키지 파일의 URL을 설정할 수 있다.   
   - services: cfn-init이 실행할 패키지이다. Linux에서는 sysvinit을 지원하고   
   Windows에서는 Windows 서비스 매니저(키: windows)를 지원한다.   
      - enabled: 부팅할 때 패키지나 서비스를 실행하는 옵션이다.   
      - ensureRunning: cfn-init 스크립트 실행 종료 후 패키지나 서비스를 실행하는  
      옵션이다.   
         
EC2 인스턴스를 생성하고 yum으로 아파치 웹 서버(httpd)를 설치하는 템플릿(install  
httpd.template)     
```
{ 
  "Description": "Create an EC2 instance running the Amazon Linux 64 bit AMI.", 
  "Parameters": {
    "KeyPair": {
      "Descirption": "The EC2 Key Pair to allow SSH access to the instance",
      "Type": "String",
      "Default": "awskeypair"
    }
  },
  "Resources": {
    "Ec2Instance": {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "KeyName": { "Ref": "KeyPair" },
        "ImageId" : "ami-c9562fc8",
        "InstanceType": "t1.micro",
        "UserData": {
          "Fn::Base64": {
            "Fn::Join":["",
              [ "#!/bin/bash\n",
              "/opt/aws/bin/cfn-init --region", { "Ref": "AWS::Region" },
              " -s ", { "Ref": "AWS::StackName" },
              " -r Ec2Instance\n" ]
            ]
          }
        }
      },
      "Metadata": {
        "AWS::CloudFormation::Init" : {
          "config": {
            "packages": {
              "yum" : {
                "https": []
              }
            },
            "services": {
              "sysvinit" : {
                "httpd": {
                  "enabled": "true",
                  "ensureRunning": "true"
                }
              }
            }
          }
        }
      }
    }
  },
  "Outputs": {
    "InstanceId" : {
      "Description": "The InstancId of the newly created EC2 instance",
      "Value": {
        "Ref": "Ec2Instance"
      }
    }
  },
  "AWSTemplateFormatVersion": "2010-09-09"
}
```

> #### AWS::CloudFormation::Init   
> AWS::CloudFormation::Init(cfn-init) 기능에 대한 자세한 내용은 다음 링크를   
> 참조하기 바란다.   
> http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-  
> init.html

> #### Bootstrap  
> bootstrap은 컴퓨터가 부팅할 때 실행되는 일련의 과정을 뜻하며 요즘 많이 쓰는  
> bootstrap 웹 프레임워크와는 다른 뜻이다.  



















