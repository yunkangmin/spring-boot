AWS 콘솔에서 Elastic Beanstalk node.js 애플리케이션을 Elastic Beanstalk 환경에   
배포하자.   
  
AWS 콘솔에서 Elastic Beanstalk 작업 흐름  
<img src="https://user-images.githubusercontent.com/33191974/158298934-49bcb6ec-0bf4-4e36-9eac-d5f246ee9afd.png" width="50%" height="50%"/>   
  
간단한 웹 페이지를 작성한다. 메모장이나 기타 텍스트 편집기를 열고 다음과 같이  
작성한 뒤 app.js로 저장한다.   
app.js  
```
var express = require('express')
 , http = require('http')
 , app = express();
 
app.get(['/', '/index.html'], function (req, res) {
  res.send('Hello Elastic Beanstalk');
});
 
http.createServer(app).listen(process.env.PORT || 3000);
```
Node.js npm 패키지 사용을 위해 다음과 같이 작성한 뒤 package.json으로 저장한다.   
package.json  
```
{
  "name": "hello",
  "description": "Hello Elastic Beanstalk",
  "version": "0.0.1",
  "dependencies": {
    "express": "4.4.x"
  }
}
```
app.js 파일과 package.json 파일을 zip으로 압축한다. 필자는 exampleapp.zip으로  
압축했다. Elastic Beanstalk 메인 페이지에 애플리케이션과 환경 목록이 표시된다.   
방금 생성한 Elastic Beanstalk 환경(exampleapp-env)을 클릭한다.  
  
Elastic Beanstalk 환경 페이지에서 Upload and Deploy 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158302345-5106fc25-2310-4da1-a398-d62f8e65f1b2.png" width="50%" height="50%"/>    
업로드 및 배포 창이 표시된다. 파일 선택 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158302549-a2ad7829-7073-4af1-a355-32bdac3a15d6.png" width="50%" height="50%"/>   
파일 열기 창에서 방금 압축했던 exampleapp.zip 파일을 선택하고 열기 버튼을 클릭한다.   
  
파일을 열면 Version label에 파일명이 그대로 입력된다. 애플리케이션을 업로드할 때  
마다 Version label이 달라져야 한다. 여기서는 `Sample Application-1`을 입력한다(  
다음 번에는 Sample Application-2, Sample Application-3 등으로 입력한다).  
설정이 완료되었으면 Deploy 버튼을 클릭한다.     
  
잠시 기다리면 Health가 Updating에서 Green으로 바뀌고, Elastic Beanstalk    
애플리케이션 배포가 완료된다. 이제 위쪽 `<환경이름>.elasticbeanstalk.com` 링크를  
클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158303151-9d16240c-d1b0-4b08-89c7-3d4efd0093c5.png" width="50%" height="50%"/>     
웹 브라우저에서 Elastic Beanstalk 환경 URL에 접속하면 app.js의 내용이 표시된다.  
<img src="https://user-images.githubusercontent.com/33191974/158303299-e9a7f186-f25a-409b-823c-8e19d8da229a.png" width="50%" height="50%"/>   
이처럼 AWS 콘솔에서 Elastic Beanstalk 애플리케이션을 간단하게 배포할 수 있다.  




































