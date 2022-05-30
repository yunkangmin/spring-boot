SNS 토픽에 메시지를 보내는 방법은 다음과 같다.   
- Message: 메시지 내용을 설정한다. UTF-8로 인코딩해야 하며 최대 크기는 262,  
144바이트이다(문자 개수가 아닌 바이트 단위이다).  
- Subject: 이메일 구독에 메시지를 보낼 때 이메일의 제목이다. 다른 프로토콜에는   
사용하지 않는다.  
- TopicArn: SNS 토픽의 ARN을 설정한다.   
- TargetArn: EndpointArn을 설정한다. EndpointArn은 Device Token을 SNS 애플리  
케이션에 추가하면 생성된다. 이 옵션은 SNS 토픽이 아닌 개별적으로 메시지를   
보낼 때 사용한다. TopicArn과 TargetArn은 둘 중 하나만 사용할 수 있다.   
   
- 참고  
<img src="https://user-images.githubusercontent.com/33191974/159148272-c0d97f76-f4fb-4e3b-a199-b6c9194b2446.png" width="50%" height="50%"/>   
     
SNS를 사용하는 방법은 '푸시 알림 서비스 SNS'를 참조하기 바란다.  
sns_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sns = new AWS.SNS();

var params = {
  Message: 'Hello SNS', //필수
  Subject: 'Hello',
  TopicArn: 'arn:aws:sns:ap-northeast-1:232075047203:admin', //필수
  //TargetArn: 'arn:aws:sns:ap-northeast-1:232075047203:endpoint/GCM/ExampleGCM/bae3463b-d6c0-3d81-89cf-44c5a8662de5', //필수 
};

sns.publish(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
AWS CLI   
```
$ aws sns publish --message "Hello SNS" --subject "Hello" --topic-arn arn:aws:sns:apnortheast-1:232075047203:admin
$ aws sns publish --message "Hello SNS" --subject "Hello" --target-arn arn:aws:sns:ap-northeast-1:232075047203:endpoint/GCM/ExampleGCM/bae3463b-d6c0-3d81-89cf-44c5a8662de5
```
  
SNS 토픽에 프로톨콜별로 다른 메시지를 보내는 방법은 다음과 같다.  
- Message: 메시지 내용을 설정한다. JSON 형태로 작성해야 하며, 텍스트 형식  
으로 변환해서 전달해야 한다. 각 프로토콜별로 보낼 메시지를 설정할 수 있고  
여기서 설정하지 않은 프로토콜에는 default에 설정한 메시지를 보낸다.
- TopicArn: SNS 토픽의 ARN을 설정한다.  
- MessageStructure: Message를 JSON 텍스트로 설정했다면 json으로 설정한다.  
  
sns_2.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sns = new AWS.SNS();

var message = {
   default: 'Hello SNS Topic',
   email: 'Hello SNS email',
   sqs: 'Hello SNS SQS',
   APNS: {
      aps: {
         alert: 'Hello SNS APNS',
         sound: 'default'
      }
   },
   GCM: {
      data: {
         message: 'Hello SNS GCM'
      }
   }
};

var params = {
   Message: JSON.stringify(message), //필수
   TopicArn: 'arn:aws:sns:ap-northeast-1:232075047203:admin', //필수
   MessageStructure: 'json'
};

sns.publish(params, function (err, data) {
   if (err)
      console.log(err, err.stack);
   else
      console.log(data);
});
```
  
메시지 내용을 JSON 형태로 작성한다.   
```
{
   "default": "Hello SNS Topic",
   "email": "Hello SNS email",
   "sqs": "Hello SNS SQS",
   "http": "Hello SNS http",
   "APNS": {
      "aps": {
         "alert": "Hello SNS APNS",
         "sound": "default"
      }
   },
   "GCM": {
      "data": {
         "message": "Hello SNS GCM"
      }
   }
}         
```
--message 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로 묶어서  
설정한다.  

AWS CLI  
```
$ aws sns publish --message '{"default": "Hello SNS Topic", "email": "Hello SNS email", "sqs": "Hello SNS SQS", "http": "Hello SNS http", "APNS": {"aps": {"alert": "Hello SNS APNS", "sound": "default" }}, "GCM": {"data": {"message": "Hello SNS GCM" }}}' --topic-arn arn:aws:sns:ap-northeast-1:232075047203:admin --message-structure.json
```

SNS 토픽의 구독 목록을 출력하는 방법은 다음과 같다.  
- TopicArn: SNS 토픽의 ARN을 설정한다.  
- NextToken: API 호출 한 번에 내용을 모두 받아올 수 없을 때 NextToken   
값이 리턴된다. 두 번째 API 호출부터 직전 API 호출에서 받은 NextToken 값을  
설정하면 다음 내용을 받아온다.  
  
sns_3.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sns = new AWS.SNS();

var params = {
   TopicArn: 'arn:aws:sns:ap-northeast-1:232075047203:admin', //필수
   //NextToken:''
};

sns.listSubscriptionByTopic(params, function (err, data) {
   if (err)
      console.log(err, err.stack);
   else
      console.log(data);
});
```
   
AWS CLI   
```
$ aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:ap-northeast-1:232075047203:admin
```

SNS 토픽에 구독을 생성하는 방법은 다음과 같다.   
- Protocol: 생성할 구독의 프로토콜을 설정한다. http, https, email,    
emailjson, sms, sqs, application을 사용할 수 있다.  
- TopicArn: SNS 토픽의 ARN을 설정한다.  
- Endpoint: 각 프로토콜의 엔드포인트를 설정한다.  
   - http: http://로 시작하는 주소를 설정한다.  
   - https: https://로 시작하는 주소를 설정한다.  
   - email, emailjson: 이메일 주소를 설정한다.  
   - sms: 핸드폰 번호를 설정한다.  
   - sqs: SQS 큐의 ARN을 설정한다.  
   - application: 모바일 장치의 EndpointArn을 설정한다. EndpointArn은   
   Device Token을 SNS 애플리케이션에 추가하면 생성된다.   
     
sns_4.js   
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sns = new AWS.SNS();

var params = {
   Protocol: 'application', //필수
   TopicArn: 'arn:aws:sns:ap-northeast-1:232075047203:admin', //필수
   Endpoint: 'arn:aws:sns:ap-northeast-1:232075047203:endpoint/GCM/ExampleGCM/bae3463b-d6c0-3d81-89cf-44c5a8662de5'
};

sns.subscribe(params, function (err, data) {
   if (err)
      console.log(err, err.stack);
   else  
      console.log(data);
});
```
  
AWS CLI   
```
$ aws sns subscribe --protocol application --topic-arn arn:aws:sns:apnortheast-1:232975047203:admin --notification-endpoint arn:aws:sns:ap-northeast-1:232075047203:endpoint/GCM/ExampleGCM/bae3463b-d6c0-3d81-89cf-44c5a8662de5  
```

SNS 토픽에서 구독을 삭제하는 방법은 다음과 같다.  
- SubscriptionArn: 구독 ARN을 설정한다. 구독 ARN은 Subscribe 함수로 SNS   
토픽에 구독을 생성할 때 리턴된다. 또는, listSubscriptionsByTopic 함수로   
구해도 된다.  
  
sns_5.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sns = new AWS.SNS();

var params = {
   SubscriptionArn: 'arn:aws:sns:ap-northeast-1:232075047203:admin:748d2cf0-2794-4eb6-b26b-3bd1-6-6cda5' //필수
};

sns.unsubscribe(params, function (err, data) {
   if (err)
      console.log(err, err.stack);
   else
      console.log(data);
});   
``` 
  
AWS CLI   
```
$ aws sns unsubscribe --subscription-arn arn:aws:sns:ap-northeast-1:232075047203:admin:748d2cf0-2794-4eb6-b26b-3bd10606cda5  
```

SNS 애플리케이션에 엔드포인트를 추가하는 방법은 다음과 같다.  
  
- GCM, APNS 모두 사용 방법은 동일하다.   
- PlatformApplicationArn: Application ARN을 설정한다.  
- Token: Device Token을 설정한다.   
  
sns_6.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sns = new AWS.SNS();

var params = {
   PlatformApplicationArn: 'arn:aws:sns:ap-northeast-1:232075047203:app/GCM/ExampleGCM', //필수
   Token: 'APA91bEtMOwbiMBK9xos6ASX8ayIQENNmX6NQ7pHQhy4rSvUtXJzjLCOCsiOK69mwy7qu9hIEWHTmFtKCSYS0c5v_m3RojYLfy1LrcCbv9vdL12qtKAMwLFX1-MpCAC0PQa8I', //필수
};

sns.createPlatformEndpoint(params, function (err, data) {
   if (err)
      console.log(err, err.stack);
   else
      console.log(data);
});
```
  
AWS CLI  
```
$ aws sns create-platform-endpoint --platform-application-arn arn:aws:sns:ap-northeast-1:232075047203:app/GCM/ExampleGCM --token APA91bEtMOwbiMBK9xos6ASX8ayIQENNmX6NQ7pHQhy4rSvUtXJzjLCOCsiOK69mwy7qu9hIEWHTmFtKCSYS0c5v_m3RojYLfy1LrcCbv9vdL12qtKAMwLFX1-MpCAC0PQa8I 
```

다른 함수들의 사용 방법은 다음 링크를 참조하자.   
  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html  
  












