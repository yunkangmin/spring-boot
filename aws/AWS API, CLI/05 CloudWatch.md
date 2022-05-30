# 정리    
- CloudWatch는 EC2 인스턴스가 이상이 있을 경우 알림을 받고자 할 때,    
사용량이 급증했을 때, 자동으로 횡적 확장을 하고 부하 분산을 구축할    
때 사용한다. 예를 들어 CPU 사용률 30% 이상일 때 알림을 받고 싶다고    
한다면 여기서 측정치를 30이 된다.    
  
--- 

CloudWatch 커스텀 측정치를 사용하는 방법은 다음과 같다.  
- MetricData: 커스텀 측정치 데이터 정의이다. 사용할 측정치를 배열 형태로  
설정한다.  
   - MetricName: 측정치 이름이다.  
   - Value: 측정치 값이다. 숫자로만 입력해야 하며 소수점을 사용할 수 있다.  
- Namespace : 측정치 대분류이다. AWS/로 시작하는 Namespace는 AWS 리소스에  
예약되어 있다.  
  
CloudWatch 커스텀 측정치를 사용하는 방법은 'CloudWatch 커스텀 측정치 사용  
하기'를 참조하기 바란다.  
  
cloudwatch_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

cloudwatch = new AWS.CloudWatch();

var params = {
  MetricData: [ //필수
    {
      MetricName: 'World 1', //필수  
      Value: 10.0
    },
    {
      MetricName: 'World 2', //필수
      Value: 20.0
    }
  ],
  Namespace: 'Hello' //필수
};

cloudwatch.putMetricData(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else 
    console.log(data);
});   
```
  
AWS CLI  
```
aws cloudwatch put-metric-data --namespace "Hello" --metric-name "World 1" --value 10
``` 
  
다른 함수들의 사용 방법은 다음 링크를 참조하기 바란다.  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html  
  
  
  
  
  
  
  
  
  
  
