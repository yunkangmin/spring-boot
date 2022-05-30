이제 EC2 인스턴스를 생성하는 템플릿을 알아보자.   
- Parameters -> KeyPair: EC2 인스턴스 접속에 사용할 키 쌍 이름을 입력받는다. 입력 받는 형식은 문자열   
(String)이며 Default로 기본값을 정할 수 있다.   
- Resources:    
   - EC2Instance: 템플릿에서 AWS 리소스를 구분할 리소스 ID이다. MyInstance, ExampleInstance등과 같이   
   사용할 수 있다.   
   - Type: AWS 리소스 종류를 설정한다. EC2 인스턴스는 AWS::EC2::Instance이다.   
   - Properties: AWS 리소스를 생성할 때 필요한 옵션이다. KeyName에 Ref를 지정하면 Parameters에 지정한   
   대로 사용자가 입력한 값을 사용하고, "KeyName": "awskeypair"처럼 값을 바로 지정할 수 있다. ImageId에는   
   AMI 이미지 ID를 설정하고, InstanceType에는 인스턴스 유형을 설정한다.   
  
EC2 인스턴스를 생성하는 CloudFormation 템플릿(createec2instance.template)   
```json
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
            "InstanceType": "t1.micro"
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
ImageId에 설정한 ami-c9562fc8은 Tokyo 리전에서 amzn-ami-pv-2014.03.1x86_64  
-ebs의 AMI 이미지 ID이다. 하지만, Oregon 리전에서는 ami-043a5034이다. 같은   
AMI라도 리전에 따라 이미지 ID가 다르다. 그러므로 템플릿을 작성할 때 이미지  
ID와 리전이 맞는지 반드시 확인한다.   
  
> #### AWS Resource Types Reference  
> AWS 리소스 종류에 대한 자세한 내용은 다음 링크를 참조하기 바란다.  
> http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-  
> template-resource-type-ref.html  

CloudFormation 템플릿 파일에는 한글을 사용할 수 없다(UTF-8로 저장해도   
한글이 표시되지 않는다).   

> #### UTF-8과 BOM CloudFormation 템플릿 파일은 UTF-8 인코딩으로 저장해야   
> 하며 BOM(Byte Order Mark)이 없어야 한다(UTF-8 without BOM).  
> Vim에서 BOM을 지우려면 다음 명령을 입력하면 된다.   
> :set nobomb  
> :wq
> 자세한 내용은 다음 링크를 참조하기 바란다.  
> http://ko.wikipedia.org/wiki/바이트 순서 표식
























