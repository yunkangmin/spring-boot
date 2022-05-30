Elastic Beanstalk의 애플리케이션에는 여러 개의 환경을 만들 수 있다. 이러한   
특징을 이용하여 서비스를 중단하지 않고 애플리케이션을 최신 버전으로 업데이트할   
수 있다. 애플리케이션 소스를 업데이트한 뒤 웹 서버를 아무리 빨리 재시작하더라도   
약간의 서비스 중단은 피할 수 없다. Elastic Beanstalk의 환경 URL 교체(Swap Environ  
ment URLs) 기능을 사용하면 약간의 서비스 중단 시간도 없앨 수 있다.   
  
**환경 URL 교체 기능을 사용하려면 애플리케이션 1개(이름이 같아야 한다)와 환경이  
2개 필요하다.** 아직   
애플리케이션과 환경을 만들지 않았다면 'Elastic Beanstalk으로 Node.js 애플리케이션과   
환경 생성하기'를 참조하여 애플리케이션과 환경을 생성하기 바란다.   
  
Elastic Beanstalk 메인 페이지로 이동한다. 아래 그림과 같이 애플리케이션과 환경   
목록이 표시된다. 오른쪽 위의 Actions 버튼을 클릭하면 팝업 메뉴가 나온다. 여기서  
Launch New Environment를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158361091-109ce056-dc7c-4067-9ac7-88a3edd9be3b.png" width="50%" height="50%"/>      
  
URL을 교체할 환경을 생성한다.  
- Environment tier: 환경 종류이다. We Server를 선택한다.   
   - Web Server: 인터넷에서 접속할 수 있는 웹 서버이다.   
   - Worker: 백 그라운드 작업을 위한 환경이다. 이 환경은 인터넷에 연결되어  
   있지 않다. 워커와 웹 서버는 SQS(Simple Queue Service)로 데이터를 주고받아야  
   한다.  
- Predefined configuration: 개발 언어이다. Node.js를 선택한다.   
   - Docker로 서버를 구축하고 배포하려면 Docker를 선택하면 된다.   
- Evironment Type: 환경의 구성 방식이다. 기본값 그대로 사용한다.   
   - Load Balancing autoscaling: 부하 분산과 자동 확장을 사용한다.  
   - Single instance: EC2 인스턴스 하나만 사용한다.    

**애플리케이션 이름이 먼저 생성된 환경의 애플리케이션과 같아야 한다.**     
<img src="https://user-images.githubusercontent.com/33191974/158362097-d11572d1-783e-4f65-be36-c4159706164c.png" width="50%" height="50%"/>   
<img src="https://user-images.githubusercontent.com/33191974/158362318-2025e319-ce80-4d8c-8002-ed3c8d1637ed.png" width="50%" height="50%"/>  
<img src="https://user-images.githubusercontent.com/33191974/158362455-68e678b3-cca3-4a44-b8ed-6743a832cf06.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/158362626-8e953cc7-d6ed-48ed-9b8c-6ad8326d320f.png" width="50%" height="50%"/>   
지금까지 설정한 내용에 이상이 없는지 확인한다. 이상이 없으면 Launch 버튼을 클릭한다.   
  
이제 Node.js 애플리케이션 소스를 수정하여 새로 만든 환경(exampleapp-env2)에 배포한다.   
앞에서 만들었던 app.js를 열고 내용을 수정한다. app.js와 package.json을 작성하지  
않았다면 'AWS 콘솔에서 Elastic Beanstalk Node.js 애플리케이션 배포하기'를 참조하여  
두 파일을 작성하기 바란다.  
  
다음 코드의 res.send 함수에서 '- Env2'를 추가한다.  
app.js   
```
var express = require('express')
   , http = require('http')
   , app = express();
   
app.get(['/', '/index.html'], function (req, res) {
  res.send('Hello Elastic Beanstalk - Env2');
});

http.createServer(app).listen(process.env.PORT || 3000);
```
app.js와 package.json 파일을 exampleapp.zip으로 압축한다.  
방금 생성한 Elastic Beanstalk 환경(exampleapp-env2)의 페이지에서 Upload and   
Deploy 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158364314-b73b6cf9-d542-44ac-8e04-c45c4f5ac82c.png" width="50%" height="50%"/>  

> #### Git으로 배포   
> AWS 콘솔에서 zip 파일을 올리지 않고 Git으로 배포해도 된다.   

잠시 기다리면 Health가 Updating에서 Green으로 바뀌고, Elastic Beanstalk 애플리  
케이션 배포가 완료된다. 오른쪽 위의 Actions 버튼을 클릭하면 팝업 메뉴가 나온다.   
여기서 Swap Environment URLs를 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/158364839-d3db23b7-c97f-40e7-9c68-05f0f044150f.png" width="50%" height="50%"/>    
이제 Elastic Beanstalk 환경(exampleapp-env2)의 URL을 교체한다. Select an Environ  
ment to Swap의 Environment name에 현재 생성된 환경의 목록이 표시된다. 처음에   
생성했던 Elastic Beanstalk 환경(exampleapp-env)을 선택하고 Swap 버튼을 클릭한다.  

> #### 만약 Swap Environment URLs를 눌렀는데 에러가 난다면 
> 애플리케이션 이름이 같은지 확인하자.       
  
<img src="https://user-images.githubusercontent.com/33191974/158374916-ee63fb38-4260-4d8f-ae9d-888c0c4c30d1.png" width="50%" height="50%"/>   
잠시 기다리면 Health가 Updating에서 Green으로 바뀌고, Elastic Beanstalk 환경   
URL이 교체된다. 위쪽 환경 URL이 exampleapp-33333.ap-northeast-2.elasticbeanstalk.com  
에서 exampleapp-env22.ap-northeast-2.elasticbeanstalk.com로 바뀌었다.   
이제 이 링크를 클릭하자.   
<img src="https://user-images.githubusercontent.com/33191974/158375495-db144376-3b3b-4402-8916-75598aae11e0.png" width="50%" height="50%"/>  
웹 브라우저에서 Elastic Beanstalk 환경 URL에 접속하면 방금 수정했던 app.js의  
내용이 표시된다(현재는 env3로 수정).     
<img src="https://user-images.githubusercontent.com/33191974/158375641-8bdc5d05-c382-4c87-bff1-397a8ab16e58.png" width="50%" height="50%"/>  
이처럼 Elastic Beanstalk 환경 URL 교체 기능을 사용하면 서비스 중단 없이 애플리  
케이션을 업데이트할 수 있다(먼저 생성된 환경의 URL로 나중에 생성한 환경의 URL을  
변경했기 때문에 먼저 생성한 환경은 다른 작업을 할 수 있고 사용자는 나중에 생성한  
환경의 애플리케이션 버전을 보게된다(애플리케이션은 같고 버전은 각자 환경에  
설정된 버전을 보기 때문에 다르다).      









































