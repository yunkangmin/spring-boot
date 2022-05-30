ELB 로드 밸선서에 EC2 인스턴스를 연결하는 방법은 다음과 같다.  
- Instances: ELB 로드 밸런서에 연결할 EC2 인스턴스 ID이다. 객체를 배열 형태로   
설정한다.   

ELB 로드 밸런서에서 EC2 인스턴스 연결을 해제하는 deregisterInstancesFromLoad  
Balancer 함수도 사용 방법이 동일하다. ELB 로드 밸런서를 사용하는 방법은 '부하  
부산과 고가용성을 제공하는 ELB'를 참조하기 바란다.   
  
elb_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var elb = new AWS.ELB();

var params = {
  Instances: [ //필수 
    {
      InstanceId: 'i-0e7abc17'
    },
    {
      InstanceId: 'i-4d7bbd54'
    }
  ],
  LoadBalancerName: 'exampleelb' //필수
};

elb.registerInstancesWithLoadBalancer(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
AWS CLI  
```
$ aws elb register-instances-with-load-balancer --load-balancer-name exampleelb --instances i-0e7abc17 i-4d7bbd54
```
다른 함수들의 사용 방법은 다음 링크를 참조하기 바란다.  
  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/ELB.html  
  
  











