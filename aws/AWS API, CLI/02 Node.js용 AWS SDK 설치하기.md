이 장에서는 Node.js에서 JavaScript용 AWS SDK를 이용한 예제 코드와 AWS CLI를  
중심으로 설명한다. Node.js를 설치하는 방법은 'EC2와 CloudFront 연동하기'를  
참조하기 바란다.   
  
다음 명령을 입력하여 npm으로 Node.js용 AWS SDK를 설치한다.    
```
npm install aws-sdk
```
  
다음과 같이 액세스 키, 시크릿 키, 리전을 설정한 뒤 config.json으로 저장한다(   
EC2 인스턴스에 IAM 역할을 사용하도록 설정했다면 config.json 파일은 필요가  
없다). 액세스 키와 시크릿 키를 생성하는 방법은 'API와 툴 사용을 위한 액세스  
키 생성하기'를 참조하자.  
```
{
  "accessKeyId": "액세스 키",
  "secretAccessKey": "시크릿 키",
  "region": "ap-northeast-2"
}
```
다음 코드처럼 require 함수로 AWS SDK를 로딩하고, AWS.config.loadFromPath   
함수에 config.json 파일을 지정하면 된다(EC2 인스턴스에 IAM 역할을 사용  
하도록 설정했다면 AWS.config.loadFromPath 함수는 호출하지 않아도 된다).  

```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');
```
  
다음 코드처럼 AWS.config.update 함수를 사용하면 중간에 리전을 변경할 수 있다(  
AWS.config.updpate 함수 호출 전에 생성한 AWS 리소스 객체에는 적용되지   
않는다).   
```
AWS.config.update({ region: 'us-west-1' });
```
AWS 리소스 객체를 생성할 때도 리전을 설정할 수 있다.  
```
var ec2 = new AWS.EC2({ region: 'us-west-1' });
var s3 = new AWS.S3({ region: 'ap-northeast-1' });
```

자세한 설정 방법은 다음 링크를 참조하자.   
  
http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-configuring.html  
  
> #### 액세스 키와 IAM 역할   
> AWS 외부에서 AWS API를 사용하려면 액세스 키와 시크릿 키를 사용해야 한다.   
> 하지만, EC2 인스턴스에서는 IAM 역할을 사용하면 액세스 키와 시크릿 키를   
> 따로 설정하지 않아도 되므로 편리하고, 자동화에도 유리하다.   

> JavaScript용 AWS API의 모든 함수와 옵션을 설명하기에는 내용이 방대하므로  
> 자주 사용하는 함수와 옵션 위주로 설명한다. 자세한 내용은 다음 링크를  
> 참조하자.   
> http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/frames.html  














