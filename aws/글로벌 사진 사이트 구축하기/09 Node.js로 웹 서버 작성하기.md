필요한 AWS 리소스를 생성하고 설정했다. 이제 Node.js로 웹 서버를 작성해보자.  
다음 예제 코드는 필자의 Github 저장소에 있는 예제 코드를 받아서 사용한다.  
app.js  
```
var express = require('express')
   //multer 모듈은 multipart/form-data를 처리하는 모듈이다.  
   , multer = require('multer')
   , AWS = require('aws-sdk')
   , Sequelize = require('sequelize')
   , mime = require('mime')
   , http = require('http')
   , fs = require('fs')
   , app = express()
   , server = http.createServer(app)
   , s3 = new AWS.S3({ region: 'ap-northeast-2' })
   , sqs = new AWS.SQS({ region: 'ap-northeast-2' });

//이미지 저장용 S3 버킷 이름, SQS 큐 URL, RDS 엔드포인트 주소(MySQL)  
//와 연결 설정은 여러분들이 생성한 AWS 리소스의 정보를 입력한다.  
//파일 정보의 테이블을 정의하고, 테이블을 생성한다.  
var s3Bucket = 'examplephoto.image02';
var sqsQueueUrl = 'https://sqs.ap-northeast-2.amazonaws.com/916803271848/ExamplePhotoQueue02';
var rdsEndpoint = {
  host: 'database-1.comfl2qhabys.ap-northeast-2.rds.amazonaws.com',
  port: 3306
};

//MySQL DB 이름, 계정, 암호
var sequelize = new Sequelize('examplephoto', 'admin', 'admin 암호', {
  host: rdsEndpoint.host,
  port: rdsEndpoint.port
});

//MySQL DB 테이블 정의
var Photo = sequelize.define('Photo', {
  filename: { type: Sequelize.STRING, allowNull: false, unique: true }
});

//MySQL DB 테이블 생성
sequelize.sync();  

//app.use 함수로 multer 모듈을 활성화한다. 그리고 업로드된 파일은  
//uploads 디렉터리에 저장한다.   
//req.file 객체에 결과를 저장한다. 
app.use(multer({ dest: './uploads/' }));

//index.html에 GET 메서드로 접속했을 때 index.html 파일을 출력한다.  
app.get(['/', '/index.html'], function (req, res) {
  fs.readFile('./index.html', function (err, data) {
    res.contentType('text/html');
    res.send(data);
  });
});
 
// /images에 GET 메서드로 접속했을 때 이미지 목록을 출력한다.
//Sequelize 모듈로 MySQL에서 이미지 파일 목록을 가져와서 배열 형태로   
//출력한다.   
//CloudFront에서 이미지 목록을 캐시하지 않도록 HTTP 헤더에 Cache-Con  
//trol을 설정한다. 이 부분을 설정하지 않으면 매번 고정된 내용을 가져   
//오게 되므로 주의한다.  
//이미지 목록 출력
app.get('/images', function (req, res) {
  Photo.findAll().success(function (photoes) {
    var data = [];
    photoes.map(function (photo) { return photo.values; }).forEach(function (e) {
      data.push(e.filename);
    });
    
    res.header('Cache-Control', 'max-age=0, s-maxage=0, public');
    res.send(data);
  });
});

// /images에 POST 메서드로 이미지(사진) 파일을 받는다.
//AWS API로 이미지 파일을 S3 버킷에 저장한다. 파일의 Content Type을 얻을  
//때 mime 모듈을 사용한다.  
//AWS API로 이미지 파일 이름을 SQS 메시지로 보낸다.  
//웹 브라우저에서 이미지 받기
app.post('/images', function (req, res) {
  fs.readFile(req.files.images.path, function (err, data) {
    var filename = req.files.images.name;
    s3.putObject({
      Bucket: s3Bucket,
      Key: 'original/' + filename,
      Body: data,
      ContentType: mime.lookup(filename)
    }, function (err, data) {
      if (err)
        console.log(err, err.stack);
      else {
        console.log(data);
        sqs.sendMessage({
          MessageBody: filename,
          QueueUrl: sqsQueueUrl
        }, function (err, data) {
          if (err)
            console.log(err, err.stack);
          else
            console.log(data);
        });
      }
    });
  });
  res.send();
});

//Node.js의 express 모듈로 80번 포트에 웹서버를 실행한다.  
server.listen(80);
```
다음은 app.js에서 사용한 모듈들의 버전을 정의한 파일이다.          
  
package.json  
```
{
  "name": "ExamplePhotoWebServer",
  "version": "0.0.1",
  "description": "ExamplePhotoWebServer",
  "dependencies": {
    "express": "4.4.x",
    "multer": "0.1.x",
    "aws-sdk": "2.0.x",
    "mime": "1.2.x",
    "sequelize": "1.7.x",
    "mysql": "2.3.2"
  }
}
```
다음 내용을 index.html로 저장한다.   
index.html  
```
<!DOCTYPE HTML>
<html>
<head>
  <title> ExamplePhoto</title>
  <!-- 트래픽을 줄이기 위해 CDN의 jQuery, jQuery UI, jQuery.fileupload,  
  Bootstrap CSS와 JavaScript를 사용한다.  -->
  <link rel="stylesheet" href= "//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
  <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/blueimp-file-upload/9.5.7/css/jquery.fileupload.min.css">
  <script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
  <script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.10.4/jquery-ui.min.js"></script>
  <script src="//cdnjs.cloudflare.com/ajax/libs/blueimp-file-upload/9.5.7/jquery.fileupload.min.js"></script>
</head>
<body>
  <span class="btn btn-success fileinput-button">
    <i class="glyphicon glyphicon-plus"></i>
      <!-- Select files... 버튼을 클릭한 뒤 이미지 파일을 선택하면   
      /images에 POST 메서드로 이미지 파일을 올린다.-->
      <span>Select files...</span>
      <input id="fileupload" type="file" name="images" multiple>
  </span>
  
  <div id="progress" class="progress">
    <div class="progress-bar progress-bar-success"></div>
  </div>
  
  <div id="imagelist"></div>
  
  <script>
    $(function () {
      //웹 서버에 파일을 올리는 과정까지만 프로그레스바로 표시한다.  
      $('#fileupload').fileupload({
        url: '/images',
        dataType: 'json',
        progressall: function (e, data) {
          var progress = parseInt(data.loaded / data.total * 100, 10);
          $('#progress .progress-bar').css('width', progress + '%');
        }
      });
      
      // /images에 접속하여 이미지 파일 목록을 가져온 뒤 imagelist에   
      //<img> 태그로 이미지를 표시한다.  
      $.getJSON('/images', function (data) {
        $.each(data, function (i, e) {
          var img = $('<img>');
          //도메인을 구입하였다면 image 서브 도메인 입력
          //img.attr('src', 'http://image.examplephoto.com/resized/' + e) 
          //도메인을 구입하지 않았다면 CloudFront 배포 도메인 입력
          img.attr('src', 'https://image.slkjfd.cf/resized/resized/' + e)
          .attr({'width': '150px', 'height': '150px'})
          .addClass('img-thumbnail');
          $('#imagelist').append(img);
        });
      });
    });
  </script>
</body>
</html>
```
앞에서 생성한 `<프로젝트 이름>.src` S3 버킷에 ExamplePhotoWebServer라는   
디렉터리를 생성하고 app.js, package.json, index.html 파일을 올린다.  
<img src="https://user-images.githubusercontent.com/33191974/159500978-0f5e0800-13c1-4211-b911-ae13748c110e.png" width="50%" height="50%"/>    




















