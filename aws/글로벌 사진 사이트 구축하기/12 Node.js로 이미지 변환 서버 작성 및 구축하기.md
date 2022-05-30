Node.js로 이미지(사진) 파일의 해상도를 줄이는 서버를 작성해보자. 다음 예제  
코드는 필자의 GitHub 저장소에 있는 예제코드를 받아서 사용한다.   

큐에서 메세지를 받아서 메세지에 있는 파일이름으로 S3에 있는 이미지를 가져온다.  
    
app.js  
```
var AWS = require('aws-sdk');
  , Sequelize = require('sequelize')
  , im = require('imagemagick')
  , mime = require('mime')
  , s3 = new AWS.S3({ region: 'ap-northeast-2' })
  , sqs = new AWS.SQS({ region: 'ap-northeast-2' });
  
//이미지 저장용 S3 버킷 이름, SQS 큐 URL, RDS 엔드포인트 주소(MySQL)와  
//연결 설정은 여러분들이 생성한 AWS 리소스의 정보를 입력한다.  
var s3Bucket = 'examplephoto.image';
var sqsQueueUrl = 'https://sqs.ap-northeast-2.amazonaws.com/916803271848/ExamplePhotoQueue02';
var rdsEndpoint = {
  host: 'examplephoto.cnlconsezo7y.ap-northeast-2.rds.amazonaws.com',
  port: 3306
};

//MySQL DB 이름, 계정, 암호
var sequelize = new Sequelize('examplephoto', 'admin', 'admin암호', {
  host: rdsEndpoint.host,
  port: rdsEndpoint.port
});

//MySQL DB 테이블 정의
//MySQL에서 파일 정보를 가져올 수 있도록 테이블을 정의한다.  
var Photo = sequelize.define('Photo', {
  filename: { type: Sequelize.STRING, allowNull: false, unique: true }
});

//SQS 메시지 삭제
function deleteMessage(ReceiptHandle) {
  sqs.deleteMessage({
    QueueUrl: sqsQueueUrl,
    ReceiptHandle: ReceiptHandle
  }, function (err, data) {
    if (err)
      console.log(err, err.stack);
    else
      console.log(data);
  });
}

//MySQL에 데이터 저장
function insertPhoto(filename) {
  sequelize.sync().success(function () {
    Photo.create({
      filename: filename
    });
  });
}

//SQS 메시지 받기 
//재귀함수 형태이며 무한루프이다.   
//receiveMessage 함수에 WaitTimeSeconds를 10초로 설정하여 긴 폴링을   
//사용하고 있다.  
//SQS 큐에서 메시지를 받은 뒤 메시지의 내용이 있으면 resizeImage 함수를  
//호출한다.  
function receiveMessage() {  
  sqs.receiveMessage({
    QueueUrl: sqsQueueUrl,
    MaxNumberOfMessages: 1,
    VisibilityTimeout: 10,
    WaitTimeSeconds: 10
  }, function (err, data) {
    if (!err && data.Messages && data.Messages.length > 0)
      resizeImage(data.Messages[0]);
    else if (err)
      console.log(err, err.stack);
    receiveMessage();
  });
}

//이미지 해상도 변환
//S3 버킷에서 이미지 파일을 가져온다.  
//imagemagick 모듈을 이용하여 이미지 파일의 해상도를 가로 800픽셀로  
//줄인다.
//해상도를 줄인 이미지 파일을 S3 버킷의 resized 디렉터리에 올린다.   
//SQS 메시지를 삭제한다.  
//Sequelize 모듈로 이미지 파일 이름을 MySQL에 저장한다.   
function resizeImage(Message) {
  var filename = Message.Body;
  s3.getObject({
    Bucket: s3Bucket,
    Key: 'original/' + filename
  }, function (err, data) {
    im.resize({
      srcData: data.Body,
      width: 800
    }, function (err, stdout, stderr) {
      s3.putObject({
        Bucket: s3Bucket,
        Key: 'resized/' + filename,
        Body: new Buffer(stdout, 'binary'),
        ACL: 'public-read',
        ContentType: mime.lookup(filename)
      }, function (err, data) {
        console.log('err :' + err);
        console.log('data: ' + data);
        console.log('Complete resize ' + filename);
        deleteMessage(Message.ReceiptHandle);
        insertPhoto(filename);
      });
    });
  });
}
receiveMessage();
```
  
다음 내용을 package.json으로 저장한다. app.js에서 사용한 모듈들의 버전을   
정의한 파일이다.   
package.json  
```
{
  "name": "ExamplePhotoResizeServer",
  "version": "0.0.1",
  "description": "ExamplePhotoResizeServer",
  "dependencies": {
    "aws-sdk": "2.0.x",
    "mime": "1.2.x",
    "sequelize": "1.7.x",
    "mysql": "2.3.2",
    "imagemagick": "0.1.x"
  }
}
```
앞에서 생성한 `<프로젝트 이름>.src` s3 버킷에서 ExamplePhotoResizePhoto라는   
디렉터리를 생성하고 app.js, package.json 파일을 올린다.  
<img src="https://user-images.githubusercontent.com/33191974/159637571-f3363175-ee1f-47e5-bff7-a1813d4a3b77.png" width="50%" height="50%"/>  



















