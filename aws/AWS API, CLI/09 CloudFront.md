CloudFront 콘텐츠를 갱신(Invalidation)하는 방법은 다음과 같다.  
- DistributionId: CloudFront 배포 ID를 설정한다.  
- InvalidationBatch: 갱신할 파일 정보를 정의한다.  
   - CallerReference: 함수가 중복 호출되지 않도록 고유한 값을 설정한다. 문자열   
   형식이며 특별할 규칙은 없다.  
   - Paths: 갱신할 파일 목록이다. Quantity에 갱신할 파일 개수를 설정하고, Items에  
   갱신할 파일을 배열 형태로 설정한다.   

CloudFront를 사용하는 방법은 '전 세계에 콘텐츠를 배포하는 CDN 서비스인 Cloud  
Front'를 참조하기 바란다.  
  
cloudfront_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var cloudfront = new AWS.CloudFront();

var params = {
  DistributionId: 'E2BSR9DHH70GFH', //필수
  InvalidationBatch: { //필수
    CallerReference: 'ExampleInvalidation1', //필수  
    Paths: { //필수
      Quantity: 2, //필수
      Items: [
        '/index.html',
        '/hello.html'
      ]
    }
  }
};  

cloudfront.createInvalidation(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
갱신할 내용을 JSON 형태로 작성한다.  
```
{
  "Paths": {
    "Quantity": 2,
    "Items": [
      "/index.html",
      "/hello.html"
    ]
  },
  "CallerReference": "ExampleInvalidation1"
}
```
  
--invalidation-batch 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로  
묶어서 설정한다.   
  
AWS CLI   
```
//cloudfront 명령을 사용할 수 있도록 활성화   
$ aws configure set preview.cloudfront true
$ aws cloudfront create-invalidation --distribution-id E2BSR9DHH70GFH --invalidation-batch '{ "Paths": { "Quantity": 2, "Items": ["/index.html", "/hello.html" ]}, "CallerReference": "ExampleInvalidation1" }'
```
다른 함수들의 사용 방법은 다음 링크를 참조하기 바란다.   
    
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudFront.html  
  




















