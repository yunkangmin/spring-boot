EC2만 생성하면 외부에서 접근을 할 수 없다. 따라서 방화벽인 Security Group을 설정  
해줘야 한다. 다음은 EC2 인스턴스를 생성하고 SSH 포트인 22번, HTTP 포트인 80번을  
여는 Security Group을 설정한다.   
  
- Ec2Instance -> Properties -> SecurityGroups: EC2 인스턴스에서 사용할 Security  
Group이다. 여러 개를 사용할 수 있다. 
- InstanceSecurityGroup: InstanceSecurityGroup이라고 임의로 리소스 ID를 정했다.  
   - Type: AWS 리소스 종류이다. Security Group은 AWS::EC2::SecurityGroup이다.   
   - Properties: Security Group을 생성할 때 필요한 옵션이다.   
      - SecurityGroupIngress: 외부에서 들어오는 Inbound 트래픽이다. AWS 콘솔에서   
      Security Group을 생성하는 규칙과 동일하다. FromPort와 ToPort를 다르게   
      설정하면 포트 포워딩 기능이 된다. 보통 FromPort와 ToPort는 동일하게   
      설정한다.  

EC2 인스턴스를 생성하고 SSH, HTTP 포트를 여는 Security Group을 설정하는   
템플릿(createec2instanceandsecuritygroup.template)   
```
{
  "Description": "Create an EC2 instance running the Amazon Linux 64 bit AMI.",
  "Parameters": {
    "KeyPair": {
      "Description": "The EC2 Key Pair to allow SSH access to the instance",
      "Type": "String",
      "Default": "awskeypair"
    }
  },
  "Resources": {
    "Ec2Instance": {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "KeyName": { "Ref": "KeyPair" },
        "ImageId": "ami-c9562fc8",
        "SecurityGroups": [{ "Ref": "InstanceSecurityGroup" }],
      "InstanceType": "t1.micro"
    }
  },
  "InstanceSecurityGroup": {
    "Type": "AWS::EC2::SecurityGroup",
    "Properties": {
      "GroupDescription": "Allow HTTP and SSH access",
      "SecurityGroupIngress" : [{
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
AWS 웹사이트의 문서를 보면서 템플릿을 만드는 것도 좋은 방법이지만, AWS에서  
미리 만들어서 제공하는 템플릿을 참조하는 것도 큰 도움이 된다. 또한, 지금까지   
EC2 인스턴스를 생성하는 방법만 간단하게 설명했지만, ELB, Auto Scaling, RDS,   
ElastiCache, S3, Route 53등 다양한 AWS 리소스를 생성하고 설정하는 템플릿도  
제공한다.  
  
https://aws.amazon.com/ko/cloudformation/aws-cloudformation-templates/
  
특히 위 링크에서는 클릭 한 번으로 미리 만들어진 템플릿을 CloudFormation 스택으로  
만들 수 있다. 그리고 Chef나 Puppet과 연동한 템플릿도 제공한다.   
  
> #### Chef와 Puppet 연동 방법   
> Chef와 Puppet에 대해 모두 설명하기에는 내용이 방대하므로 다음 링크를 참조하기  
> 바란다.   
> https://aws.amazon.com/ko/cloudformation/aws-cloudformation-articles-and-  
> tutorials/  





















