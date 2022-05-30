SES로 메일을 보내는 방법은 다음과 같다.  
- SES는 Tokyo(ap-northeast-1) 리전에 없으므로 Oregon(us-west-2) 리전을 사용한다.   
- Message: 메일의 제목과 내용을 설정한다. Body에서 Text 키 대신 html 키를   
사용하면 HTML 메일을 보낼 수 있다.  
- Source: 보내는 사람 이메일 주소를 설정한다. SES에서 인증한 이메일 주소나   
도메인으로 설정해야 한다.  
- ReplyToAddress: 답장을 받을 이메일 주소를 설정한다.  
- ReturnPath: 리턴 메일을 받을 이메일 주소를 설정한다.  

SES를 사용하는 방법은 '이메일 전송 서비스 SES'를 참조하자.  
  
ses_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');
AWS.config.update({ region: 'us-west-2' });

var ses = new AWS.SES();

var params = {
  Destination: { //필수
    //숨은 참조 주소
    BccAddresses: [],
    //참조 주소
    CcAddresses: [],
    ToAddresses: [ 'user1@example.net' ]
  },
  Message: { //필수
    Body: { //필수
      Text: {
        Data: 'Hello SES', //필수
        Charset: 'utf-8'
      },
    },
    Subject: {//필수
      Data: 'Hello', //필수
      Charset: 'utf-8'
    }
  },
  Source: 'admin@example.net', //필수
  ReplyToAddress: [],
  ReturnPath: 'return@example.net'
};

ses.sendEmail(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
받을 사람을 JSON 형태로 작성한다.   
```
{
  "BccAddresses": [],
  "CcAddresses": [],
  "ToAddresses": ["user1@example.net"]
}
```
  
메일의 제목과 내용을 JSON 형태로 작성한다.  
```
{
  "Body": {
    "Text": {
      "Data": "Hello SES",
      "Charset":  "utf-8"
    }
  },
  "Subject": {
    "Data": "Hello",
    "Charset": "utf-8"
  }
}
```
  
--destination, --message 옵션에 작성한 JSON을 한 줄로 만들고, ''(작은 따옴표)로  
묶어서 설정한다.   
  
AWS CLI  
```
$ aws ses send-email --from admin@example.net --destination '{"BccAddresses": [], "CcAddresses": [], "ToAddress": ["user1@example.net"]}' --message '{"Body": {"Text": {"Data": "Hello SES", "Charset": "utf-8" }}, "Subject": {"Data": "Hello", "Charset": "utf-8" }}' --region=us-west-2
```
   
SES의 메일 전송 할당량, 보낸 메일 개수, 초당 전송 한계를 조회하는 방법은  
다음과 같다.   
  
ses_2.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');
AWS.config.update({ region: 'us-west-2' });

var ses = new AWS.SES();

ses.getSendQuota({}, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
AWS CLI   
```
$ aws ses get-send-quota --region=us-west-1
```
다른 함수들의 사용 방법은 다음 링크를 참조하자.   
  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html  





















