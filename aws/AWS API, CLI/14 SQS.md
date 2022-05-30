# 정리
- 메시지 메타데이터(MessageAttributes)    
메시지 속성을 사용하여 사용자 지정 메타데이터를 애플리케이션에 대한    
Amazon SQS 메시지에 연결할 수 있다. 메시지 시스템 속성을 사용하여     
AWS X-Ray와 같은 다른 AWS 서비스에 대한 메타데이터를 저장할 수     
있다.    
- Amazon SQS 메시지 속성  
Amazon SQS를 사용하여 구조화된 메타데이터(예: 타임스탬프, 지형 정보 데이터,  
서명 및 식별자)를 사용하여 메시지에 포함할 수 있다. 각 메세지에는 최대 10개의   
속성이 있을 수 있다. 메시지 속성은 선택사항이며 메시지 본문과 분리된다(단,  
메시지 본문과 함께 전송됨). 소비자는 메시지 본문을 먼저 처리하지 않고도   
메시지 속성을 사용하여 특정 방식으로 메시지를 처리할 수 있다. 

  
- 메타데이터(metadata)는 데이터에 관한 구조화된 데이터로, 다른 데이터를    
설명해주는 데이터를 말한다. 즉 동영상, 소리, 문서 등과 같이 실제로    
존재하거나 사용할 수 있는 데이터는 아니지만, 이러한 실제 데이터와     
직접적 또는 간접적으로 연관된 정보를 제공해주는 데이터를 말한다.   

- 보기 제한 시간(Visibility Timeout): 메시지를 받은 뒤 특정 시간 동안   
다른 곳(다른 서버)에서 동일한 메시지를 다시 꺼내볼 수 없게 하는 기능  
이다. 큐 하나에 여러 서버가 메시지를 받을 때 동일한 메시지를 동시에   
처리하는 것을 방지한다. 보기 제한 시간은 큐가 메시지를 서버에게 전송  
할 때 시작되고 이 시간 동안 메시지를 받은 서버는 메시지를 처리하고   
큐에 삭제요청을 보낸다. 따라서 메시지를 한 번만 수신해야 하는 서버는   
가시성 시간 내에 메시지를 삭제하는 요청을 보내야 한다.   
한 메시지 소비자가 대기열에서 수신한 메시지가 다른 메시지 소비자에게  
보이지 않게 되는 시간.  그러니까 Queue가 하나 있고 해당 Queue에서  
메시지를 받는 서버가 2대 있으면, 한 서버에서 메시지가 가고 나서 정해둔   
시간만큼은 다른 서버에 그 메시지를 보내지 않는다로 이해할 수 있다.   
- 지연 전송(Delay Delivery): 특정 시간 동안 메세지를 받는 서버가   
메시지를 받지 못하게 하는 기능이다.  
대기열에 추가된 각 메시지의 첫 번째 전송에 대한 지연시간. 일부러   
첫 번째 전송을 늦추고 싶은 경우 전송 지연을 설정할 수 있다.  
사용하는 이유는 메시지를 받는 서버에서 메시지를 처리하기 위해 추가   
시간이 필요한 경우 사용한다.  
  
- 둘다 큐에 설정한다.  
  
- 지연 전송을 하면 메시지를 보낸 뒤에 일정 시간 동안 메시지를 받을 수   
없고, 보기 제한 시간을 설정하면 메시지를 받은 뒤 일정 시간 동안에는  
다른 곳에서 메시지를 받을 수 없다.  

    
---

SQS 큐에 메시지를 보내는 방법은 다음과 같다.  
- MessageBody: 메시지 내용이다.  
- QueueUrl: 메시지를 보낼 SQS 큐 URL이다. 
- DelaySeconds: 지연 전송 시간이다(특정 시간동안 메세지를 받지 못하게 하는 기능).    
- MessageAttributes: 메시지 추가 속성이다(추가적으로 설정할 정보(이름, 타입, 값을 설정한다)).         
   - DataType: String, Number, Binary를 사용할 수 있다.  
   - StringValue: DataType이 String, Number일 때 사용한다.  
   - BinaryValue: DataType이 Binary일 때 사용하며 Buffer 형식으로 값을 설정한다.  
  
<img src="https://user-images.githubusercontent.com/33191974/159150911-70221ac6-2c72-483c-915f-693555937a50.png" width="50%" height="50%"/>  
  
sqs_1.js   
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sqs = new AWS.SQS();

var params = {
  MessageBody: 'Hello SQS', //필수
    QueueUrl: 'https://sqs.ap-northeast-1.amazonaws.com/232075047203/ExampleQueue', //필수
    DelaySeconds: 0,
    MessageAttributes: {
      someKey1: {
        DataType: 'String', //필수
        StringValue: 'Hello String'
      },
      someKey2: {
        DataType: 'Number', //필수
        StringValue: '10'
      },
      someKey3: {
        DataType: 'Binary', //필수
        BinaryValue: new Buffer([1, 2, 3, 4])
      }
   }
};
  
sqs.sendMessage(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});   
```
메세지 추가 속성을 JSON 형태로 작성한다.  
```
{ 
  "someKey1": {
    "DataType": "String",
    "StringValue": "Hello String"
  },
  "someKey2": {
    "DataType": "Number",
    "StringValue": "10"
  }
}
```
  
--message-attributes 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로  
묶어서 설정한다.  
  
AWS CLI  
```
$ aws sqs send-message --queue-url https://sqs.ap-northeast-1.amazonaws.com/232075047203/ExampleQueue --message-body "Hello SQS" --message-attibutes '{"someKey1": {"DataType": "String", "StringValue": "Hello String" }, "someKey2": {"DataType": "Number", "StringValue": "10" }}'
```
SQS 큐에 여러 메시지를 보내는 방법은 다음과 같다.  
- Entries: 보낼 메시지를 배열 형태로 설정한다.   
   - Id: 각 메시지의 ID이다. 특별한 형식은 없고 Enties 배열 안에서만 고유하면   
   된다.  
- 나머지는 메시지 보내기와 동일하다.  
  
sqs_2.js   
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sqs = new AWS.SQS();

var params = {
  Entries: [ //필수
    {
      Id: 'abcd1',
      MessageBody: 'Hello SQS 1', //필수
      DelaySeconds: 0,
      MessageAttributes: {
        someKey1: {
          DataType: 'String', ///필수
          StringValue: 'Hello String'
        }
      }
    },
    {
      Id: 'abcd2',
      MessageBody: 'Hello SQS 2', //필수
      DelaySeconds: 0,
      MessageAttributes: {
        someKey1: {
          DataType: 'String', //필수
          StringValue: 'Hello String'
        }
      }
    }
  }
],
QueueUrl: 'https://sqs.ap-northeast-1.amazonaws.com/232075047203/ExampleQueue',//필수
};

sqs.sendMessageBatch(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
보낼 메시지를 JSON 형태로 작성한다.  
```
[
  {
    "Id": "abcd1",
    "MessageBody": "Hello SQS 1",
    "DelaySeconds": 0,
    "MessageAttributes": {
      "someKey1": {
        "DataType": "String",
        "StringValue": "Hello String"
      }
    }
  },
  {
    "Id": "abcd2",
    "MessageBody": "Hello SQS 2",
    "DelaySeconds": 0
    "MessageAttributes": {
      "someKey1": {
        "DataType": "String",
        "StringValue": "Hello String"
      }
    }
  }
]     
```
  
--entries 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로 묶어서  
설정한다.   
  
AWS CLI   
```
$ aws sqs send-message-batch --queue-url https://sqs.ap-northeast-1.amazonaws.com/232075047203/ExampleQueue --entries '[{ "Id": "abcd1", "MessageBody": "HelloSQS 1", "DelaySeconds": 0, "MessageAttributes": { "someKey1": { "DataType": "String", "StringValue": "Hello String" }}}, {"Id": "abcd2", "MessageBody": "Hello SQS 2", "DelaySeconds": 0, "MessageAttribues": { "someKey1": { "DataType": "String", "StringValue": "Hello String" }}}]'
```
  
**SQS 큐**에서 **메시지를 받는 방법**은 다음과 같다.  
- QueueUrl: 메시지를 받을 SQS 큐 URL이다.  
- AttributeNames: 메시지의 속성을 함께 받는다. 속성을 모두 받으려면 All을  
설정해도 된다.  
- MaxNumberOfMessages: 함수 호출 한 번에 받을 최대 메시지 개수이다. 이 옵션을   
설정한다 하더라도 설정한 개수만큼 받을 수도 있고, 적게 받을 수도 있다.  
- MessageAttributeNames: 메시지 추가 속성을 함께 받는다. 속성 키 이름을   
배열 형태로 설정하면 되고, 속성을 모두 받으려면 All을 설정하면 된다.   
- VisibilityTimeout: 초 단위로 된 보기 제한 시간이다.   
- WaitTimeSeconds: 짧은 폴링, 긴 폴링 사용 설정이다. 0으로 설정하면 짧은  
폴링, 1이상 설정하면 긴 폴링을 사용한다.  
  
sqs_3.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sqs = new AWS.SQS();

var params = {
  QueueUrl: 'https://sqs.ap-northeast-1.amazonaws.com/2322075047203/ExampleQueue', //필수
  AttributeNames: [
    'ApproximateFirstReceiveTimestamp',
    'ApproximateReceiveCount',
    'SenderId',
    'SentTimestamp'
  ],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ['someKey1', 'someKey2' ],
  VisibilityTimeout: 0,
  WaitTimeSeconds: 0
};

sqs.receiveMessage(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
AWS CLI   
```
$ aws sqs receive-message --queue-url https://sqs.ap-northeast-1.amazonaws.com/2322075047203/ExampleQueue --attribute-names ApproximateFirstReceiveTimestamp ApproximateReceiveCount SenderId SentTimestamp --max-number-of-messages 1 --messageattribute-names someKey1 someKey2 --visibility-timeout 0 --wait-time-seconds 0 
```
  
SQS에서 메시지를 삭제하는 방법은 다음과 같다.   
- QueueUrl: 메시지를 삭제할 SQS 큐 URL이다.   
- ReceiptHandler: 삭제할 메시지의 Receipt handle이다. 이 값은 ReceiveMessage   
함수로 메시지를 받았을 때 얻을 수 있다.    
  
sqs_4.js   
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sqs = new AWS.SQS();

var params = {
  QueueUrl: 'https://sqs.ap-northeast-1.amazonaws.com/2322075047203/ExampleQueue',  //필수
  ReceiptHandle: 'sfLDkndKlsdfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk==', //필수
};
  
sqs.deleteMessage(params, function (err, data) [
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```

AWS CLI   
```
$ aws sqs delete-message --queue-url https://sqs.ap-northeast-1.amazonaws.com/2322075047203/ExampleQueue --receipt-handle sfLDkndKlsdfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk==
```
  
SQS 큐에서 여러 메시지를 삭제하는 방법은 다음과 같다.   
- Entries: 삭제할 메시지를 배열 형태로 설정한다.  
   - Id: 각 메시지의 ID이다. 특별한 형식은 없고 Entries 배열 안에서만  
   고유하면 된다.  
- 나머지는 메시지 삭제하기와 동일하다.  
  
sqs_5.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

var sqs = new AWS.SQS();

var params = {
  Entries: [ //필수
  { 
    Id: 'abcd1',
    ReceiptHandle: 'sfLDkndKfdgSDfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk==' //필수
  },
  {
    Id: 'abcd2',
    ReceiptHandle: 'sfLDkndKlsdfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk==' //필수
  }
],
QueueUrl: 'https://sqs.ap-northeast-1.amazonaws.com/2322075047203/ExampleQueue', //필수
};

sqs.deleteMessageBatch(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
삭제할 메시지를 JSON 형태로 작성한다. 
```
[
  {
    "Id": "abcd1",
    "ReceiptHandle": "sfLDkndKfdgSDfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk=="
  },
  {
    "Id": "abcd2",
    "ReceiptHandle": "sfLDkndKlsdfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk=="
  }
]
```
--entries 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로 묶어서 설정한다.  
  
AWS CLI  
```
$ aws sqs delete-message-batch --queue-url https://sqs.ap-northeast-1.amazonaws.com/2322075047203/ExampleQueue --entries '[{"Id": "abcd1", "ReceiptHandle": "sfLDkndKfdgSDfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk==" }, { "Id": "abcd2", "ReceiptHandle": "sfLDkndKlsdfmsdfsdfsdfDS+dsf/sdfsSDFFsGWMdsdnfk==" }]'
```

다른 함수들의 사용 방법은 다음 링크를 참조하자.   
   
http://docs.aws.amazon.com/AWS/JavaScriptSDK/latest/AWS/SQS.html    
  




























