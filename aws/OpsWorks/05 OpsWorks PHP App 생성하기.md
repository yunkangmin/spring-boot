# 정리  
- 레이어는 EC2 인스턴스 생성, Elastic IP 할당, 애플리케이션을 구성하는   
템플릿이다.  
- VPC는 리전별로 생성  
- 서브넷은 가용영역별로 생성  
  
---

간단한 PHP 소스 코드를 작성해 App을 생성해보자. 다음 내용을 index.php로 저장한다.   
index.php  
```
<html>
  <head>
    <title>Hello OpsWorks</title>
  </head>
  <body>
    <?php eco 'Hello OpsWorks';?>
  </body>
</html>
```
index.php 파일을 app.zip으로 압축한 뒤 S3 버킷에 올린다. S3 버킷을 생성하고  
파일을 올리는 방법은 'S3 버킷 생성하기, S3 버킷에 파일 올리기/받기'를  
참조하기 바란다.   
  
S3 버킷에 PHP 소스 코드 업로드 
<img src="https://user-images.githubusercontent.com/33191974/158534937-1a0811ae-5d75-4de6-8ae9-87cce6fdd183.png" width="50%" height="50%"/>   
OpsWorks App 목록에서 Add an app을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158535131-be22e2b3-88ae-4ff6-a858-1b40e69106c5.png" width="50%" height="50%"/>   
OpsWorks PHP App을 생성한다.   
- Name: App 이름이다. examplephp를 입력한다.   
- Type: App 종류이다. 기본값 그대로 사용한다.   
- Document root: 작성한 소스 코드가 디렉터리로 구분되어 있을 때 최상위 문서의   
경로이다. 기본값 그대로 사용한다.  
- Data source type: 데이터베이스 사용 옵션이다. RDS를 사용하거나, OpsWorks로   
구성한 데이터베이스를 사용할 수 있다. 기본값 그대로 사용한다.   
- Repository type: 소스 저장소 종류이다. Git, Subversion, Http Archive,  
S3 Archive를 사용할 수 있다. S3 Archive를 선택한다.   
   - Repository URL: S3 버킷에 올린 app.zip 파일의 URL을 입력한다.  
   - Access key ID: S3 버킷에 접근할 수 있도록 액세스 키를 입력한다.  
   - Secret access key: 액세스 키의 시크릿 키를 입력한다. 액세스 키와   
   시크릿 키를 생성하는 방법은 'API와 툴 사용을 위한 액세스 키 생성하기'를   
   참조하기 바란다.  
   
<img src="https://user-images.githubusercontent.com/33191974/158536699-dd3d1840-06e0-46fa-9bbf-7726ae52a5e4.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/158536760-c2dff801-9e36-4eab-aa0c-989ca771eee3.png" width="50%" height="50%"/>   
OpsWorks PHP App이 생성되었다.   
<img src="https://user-images.githubusercontent.com/33191974/158536856-4ab946c8-7cc6-454f-895a-855b45b79d72.png" width="50%" height="50%"/>   
  
---  
  
#### Node.js App 작성하기  
다음 내용을 server.js로 저장한다. 파일이름을 app.js로 해서 OpsWorks에    
배포하면 동작하지 않는다.   
server.js  
```
var express = require('express')
   , http = require('http')
   , app = express();
 
app.get(['/', '/index.html'], function (req, res) {
  res.send('Hello OpsWorks');
});

http.createServer(app).listen(80);
```
  
다음 내용을 package.json으로 저장한다.   
package.json   
```
{
  "name": "hello",
  "description": "Hello OpsWorks",
  "version": "0.0.1",
  "dependencies": {
    "express": "4.4.x"
  }
}
```
두 파일을 Git 저장소에 올리거나 app.zip으로 저장하여 S3 버킷에 올린다.  





























