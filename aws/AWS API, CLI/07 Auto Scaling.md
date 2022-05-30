# 정리  
- 키 쌍 : EC2 접속을 위한 것  
- 액세스 키 : API(코드로 리소스 제어)와 툴 사용을 위한 것  
AWS 계정은 모든 AWS 리소스에 무제한으로 접근이 가능하므로 AWS    
계정의 액세스 키를 사용하기 보다는 IAM에서 계정을 생성한 뒤 권한을    
제어할 것을 권장한다.     
AWS 계정으로 AWS 콘솔 페이지에 접속하듯이 IAM 사용자도 AWS 콘솔에    
접속할 수 있다. IAM 사용자에게 특정 AWS 리소스에만 접근할 수 있도록    
설정했다면 AWS 콘솔에서도 허용된 AWS 리소스만 제어할 수 있다.    
또한 IAM 계정은 액세스 키를 별도로 생성할 수 있고, 이 액세스 키를    
이용하여 AWS API를 사용할 수 있다(IAM 사용자는 IAM 그룹에 포함되고    
IAM 그룹에서 권한을 부여하며 IAM 사용자는 그룹에 속한다. ).   

---

다음과 같다.   
- AutoScalingGroupNames: 정보를 가져올 Auto Scaling 그룹 이름을 배열 형태로   
설정한다.  
- Auto Scaling관련 함수들은 EC2 권한으로 사용할 수 있다(EC2에서 IAM 역할로 사용할 때  
역할에 EC2 권한이 있어야 되는 걸 말하는 듯?).     
  
Auto Scaling을 사용하는 방법은 '자동으로 EC2 인스턴스를 생성해 서비스를 확장  
하는 Auto Scaling'을 참조하기 바란다.  
   
autoscaling_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var autoscaling = new AWS.AutoScaling();

var params = {
  AutoScalingGroupNames: [
    'ExampleAutoScalingGroup'
  ]
};

autoscaling.describeAutoScalingGroups(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
AWS CLI   
```
$ aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names ExampleAutoScalingGroup
```
  
다른 함수들의 사용 방법은 다음 링크를 참조하기 바란다.   
  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/AutoScaling.html  
  




















