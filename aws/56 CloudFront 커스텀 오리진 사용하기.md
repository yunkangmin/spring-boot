CloudFront의 기본 오리진은 S3이다. S3이외에 EC2 인스턴스, ELB, 외부 웹 서버를  
오리진으로 사용하는 것을 커스텀 오리진(Custom Origin)이라고 한다. 커스텀 오리진은  
동적 콘텐츠 전송(Dynamic Content Delivery)이 필요할 때 사용한다. Node.js 혹은   
PHP, JSP< ASP등의 서버 사이드 스크립트에서 동적으로 생성되는 웹 페이지를   
캐시할 수 있다. 특히 커스텀 오리진을 사용하면 동일한 도메인에서 POST, PUT,   
DELETE등의 메서드를 사용할 수 있어 로그인이나 글쓰기 기능도 구현할 수 있다.   
  
커스텀 오리진의 필수 조건은 웹 서버이다. 운영체제, 웹 서버 애플리케이션,   
프로그래밍 언어의 종류와는 상관이 없다.  

# EC2와 CloudFront 연동하기 
EC2 인스턴스에 웹 서버를 실행하고 CloudFront와 연동하는 방법을 알아보자.    
EC2와 CloudFront 연동하기  
<img src="https://user-images.githubusercontent.com/33191974/155966900-00316245-1612-466d-8494-2eefa2c2d93f.png" width="50%" height="50%"/>   
이전에 생성한 EC2 인스턴스(Example Server)를 그대로 사용한다.   
EC2 인스턴스가 생성되어 있지 않다면 [EC2 인스턴스 생성하기](https://github.com/yunkangmin/spring-boot/blob/main/aws/08%20EC2%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.md)를 참조하여  
EC2 인스턴스를 생성하기 바란다(Amazon Linux 설치를 권장한다).    
  
웹 서버는 Node.js로 간단하게 실행한다. 웹 서버는 Apache나 Nginx를 사용해도   
상관없다.   

** Linux 인스턴스에서 Node.js를 설정하려면   
1. SSH를 사용하여 `ec2-user`로 Linux 인스턴스에 연결한다.   
2. 명령줄에 다음을 입력하여 nvm(노드 버전 관리자)을 설치한다.   
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```
nvm을 사용하면 여러 버전의 Node.js를 설치할 수 있고 여러 버전 간을 전환할 수   
있기 때문에 여기서는 nvm을 사용하여 Node.js를 설치한다.   
3. 명령줄에 다음을 입력하여 nvm을 활성화한다.   
```
. ~/.nvm/nvm.sh 
```
4. nvm에서 명령줄에 다음과 같이 입력하여 사용하려는 Node.js의 최신 버전을   
설치한다.   
```
nvm install node
```
Node.js를 설치하면 npm(노드 패키지 관리자)도 설치되므로 필요에 따라 추가 모듈을   
설치할 수 있다.   
5. 명령줄에 다음을 입력하여 Node.js가 올바르게 설치되고 실행되는지 테스트한다.   
```
node -e "console.log('Running Node.js ' + process.version)"
```
이렇게 하면 실행중인 Node.js의 버전을 보여주는 메시지가 다음과 같이 표시된다.    
   
ExampleServer라는 디렉토리를 생성하고, 이 디렉터리로 이동한 후 express 모듈을  
설치한다.   
```
mkdir ExampleServer
cd ExampleServer
npm i express
```
해당 경로에 app.js를 추가하고 다음을 코딩하자.   
```
touch app.js
vim app.js
```
ExampleServer 디렉터리 안에 간단한 웹 페이지를 출력하는 코드를 작성한다. 테스트  
편집기를 열고 아래와 같이 작성한 뒤 app.js로 저장한다.   
app.js  
```
var express = require('express');
var app = express();

app.get(['/', '/index.html'], function (req, res) {
        res.send('Hello CloudFront - EC2');
});

app.listen(80);
```
파일 저장이 끝났으면 다음 명령을 입력하여 Node.js 서버를 실행한다.   
```
sudo node app.js
# 위 명령이 실행되지 않는다면 아래 명령 실행 후 해보자(버전은 맞게 수정할 것, 링크 파일을  
잘못만들었다면 -f 옵션을 사용해서 덮어쓰기를 하자(예) -sf)).  
sudo ln -s /home/ec2-user/.nvm/versions/node/v17.6.0/bin/node /usr/bin/node
```
웹 서버 실행 후 Public DNS로 접속해보면 웹 서버가 잘 구동되고 있음을 확인할 수  
있다.   
접속이 안되면 아래와 같이 80포트 인바운드를 허용하자.    
<img src="https://user-images.githubusercontent.com/33191974/155977448-0bbaab14-cbc1-4da4-80a7-618da7e05bfb.png" width="50%" height="50%"/>  

# 이제 CloudFront를 통해 연결해보자
그렇다면 이 EC2를 CloudFront로 연결해보자.   
S3을 CloudFront로 연결한 것과 대동소이하다.   
CloudFront에서 CloudFront 배포 생성을 누른다.   
<img src="https://user-images.githubusercontent.com/33191974/155979737-6d58aecb-51f0-4115-89e3-3dbb2c6527bf.png" width="50%" height="50%"/>    
원본 도메인은 EC2의 도메인 이름을 적어주면 된다. 
주의할 점은 EC2를 오리진으로 사용할 때 EIP를 반드시 사용하도록 하자.   
EC2를 오리진으로 사용할 때 EC2에 EIP를 연결하지 않으면 EC2를 재부팅할 때   
IP가 바뀌기 때문에 Public DNS를 사용할 수 없게 된다.   
따라서 오리진에 접속할 수 없게 되고 캐시 기능도 작동하지 않게 된다.   
EC2를 오리진으로 사용할 때 EIP를 반드시 사용하도록 하자.   
  
deploy를 완료하면 CDN이 잘 작동하는 것을 확인할 수 있다.   
<img src="https://user-images.githubusercontent.com/33191974/155980262-4736f6b0-f45b-4afc-b53d-5b8dfca57bd4.png" width="50%" height="50%"/>       
여기서 중요한 건 이 CloudFront가 생성한 Domain Name이다.   
이게 우리가 생성한 CloudFront 주소이다.  
이 주소로 접속한 후 response header에서 x-cache를 확인해보자.   
  
그런데 문제는, 캐시 설정을 해야 한다는 것이다.   
오리진에서 내용을 업데이트해도 CDN에 저장한 정보는 업데이트 되지 않는다.   
  
이는 업데이트 했음에도 캐시 수명이 다하지 않았기 때문에 캐시에 저장된 과거  
내용이 서빙되기 때문이다.  
EC2의 내용을 수정한 뒤 CDN 도메인으로 접속해보자. 수정사항이 반영되지 않을  
것이다.  
  
이런 경우는 무효화(Invalidation)를 날려줘서 오리진 서버에서 다시 정보를 받아올  
수 있게 해야 한다.   

# Invalidation  
위에서 언급했듯, 오리진의 내용이 변경되었는데도 캐시에 이미지가 남아 있기   
때문에 업데이트된 내용이 아닌 과거 내용이 서빙되는 것이 문제이다.   
  
이 문제를 해결할 방법은 다음과 같다.   
1. invalidation하기(모든 에지 로케이션에 무효화가 적용되는데 10분정보 걸리나  
편리함)    
보다시피 그냥 CloudFront에 들어가서 invalidations을 만들어서 object path를    
일일이 입력해주고 진행해주면 된다.    
<img src="https://user-images.githubusercontent.com/33191974/155981549-049cbff9-808b-41fc-b387-f2191d3a837a.png" width="50%" height="50%"/>    
  
2. 현재 이용하는 객체의 이름 바꾸기(객체 이름과 코드를 수정해야 하는 수작업  
이지만 1분 내로 수정됨)     
예를 들어 현재 image/1을 서빙 중인데 이를 바꾸고 싶다고 생각해보자. 그러면    
업데이트하고 싶은 내용을 image/2로 새로 업로드하고 코드 상에서도 그 객체를  
이용하도록 수정한다.  
3. 캐시의 TTL을 짧게 유지하기 -> 캐시 서버를 이용하는 이유가 없어진다.   
합리적이지 않은 방법이다.   
<img src="https://user-images.githubusercontent.com/33191974/155982024-32f4ed9f-11dd-474c-a9a4-be0317527261.png" width="50%" height="50%"/>      

# CloudFront 설정관련  
- Origin Domain Name:   
 오리진을 사용하려면 이곳에 오리진 서버의 도메인을     
설정하면 된다. EC2 인스턴스(Example Server)의 Public DNS를 입력한다.  
   - ELB(Elastic Load Balancing)의 경우 S3와 마찬가지로 Origin Domain Name   
   부분을 클릭하면 현재 생성된 ELB의 목록이 표시된다. 여기서 ELB를 선택하면     
   되고, 나머지 설정은 EC2 인스턴스 오리진과 동일하다.      
     <img src="https://user-images.githubusercontent.com/33191974/156490815-9b6dd37b-7fbc-4eb4-a129-94c82204092c.png" width="50%" height="50%"/>  
       
   - EC2 인스턴스에서 사용자가 구입한 도메인을 연결했다면 해당 도메인을  
   입력해도 된다(예: example.com)  
- OriginID: 오리진을 구분하는 ID이다. 크게 중요한 것은 아니며 Origin Domain  
Name을 설정하면 자동으로 생성된다.   
- Origin Protocol Policy: CloudFront로 보여질 프로토콜 정책이다. 기본값   
그대로 사용한다(EC2를 Origin Domain Name으로 설정하면 보임). 오리진 프로토콜   
정책은 오리진에 연결할 때 CloudFront에서 사용할 프로토콜(HTTP 또는 HTTPS)을  
결정한다. 다음 옵션을 선택할 수 있다(CloudFront가 Origin에 액세스하는 방식).      
  <img src="https://user-images.githubusercontent.com/33191974/156492055-89addc2b-950d-4ce8-bf80-3c14cac8e113.png" width="50%" height="50%"/>   

   - HTTP Only: HTTP 프로토콜만 사용한다(기본값). 보충 설명 -> CloudFront는  
   HTTP만 사용하여 오리진에 액세스한다. 오리진이 Amazon S3 정적 웹   
   사이트 호스팅 엔드포인트이고 변경할 수 없는 경우의 기본 설정이다.    
   - HTTPS Only: CloudFront는 HTTPS만 사용하여 액세스한다.    
   - Match Viewer: CloudFront에 HTTP로 접속하면 HTTP로 전송하고, HTTPS로  
   접속하면 HTTPS로 전송한다. 보충설명 -> CloudFront는 최종 사용자 요청의   
   프로토콜에 따라 HTTP 또는 HTTPS를 사용하여 오리진과 통신한다. CloudFront  
   에서 이 오리진으로 전달하는 HTTPS 뷰어 요청을 일치시키려면 오리진 서버의   
   SSL 인증서에 있는 도메인 이름 중 하나가 오리진 도메인에 지정한 도메인   
   이름과 일치해야 한다.   
- HTTP Port: HTTP 프로토콜의 포트 번호이다. 기본값 그대로 사용한다.  
- HTTPS Port: HTTPS 프로토콜의 포트번호이다. 기본값 그대로 사용한다.   
- Path Pattern: CloudFront로 파일을 가져올 규칙이다. 기본값은 `*`로 설정되어   
있어서 모든 파일을 가져오게 된다. 이 부분은 여기서는 수정할 수 없고 배포   
(Distribution)를 생성한 뒤 따로 추가할 수 있다.   
  <img src="https://user-images.githubusercontent.com/33191974/156492414-43fe15b1-798a-415f-9ad0-263809687368.png" width="50%" height="50%"/>  

- Viewer Protocol Policy: CloudFront로 보여질 프로토콜 정책을 설정한다.   
기본값 그대로 사용한다(CloudFront에 접속하는 방식).     
   - HTTP and HTTPS: HTTP와 HTTPS를 둘 다 사용한다.
   - Redirect HTTP to HTTPS: 모든 HTTP 접속을 HTTPS로 리다이렉트한다.  
   - HTTPS Only: HTTPS만 사용한다.
- Allowed HTTP Methods: 허용하는 HTTP 메서드 종류이다. 기본값 그대로 사용한다.   
   - GET, HEAD: 파일을 읽기만 할 때 선택한다.   
   - GET, HEAD, PUT, POST, PATCH, DELETE, OPTIONS: 동적 콘텐츠 전송을 사용할   
   때 선택한다.   

> #### EC2 인스턴스 오리진과 Elastic IP   
> EC2 인스턴스를 오리진으로 사용할 때는 EC2 인스턴스에 Elastic IP를 연결했는지   
> 확인한다. Elastic IP를 연결하지 않았을 경우, EC2 인스턴스를 재부팅하면 IP  
> 주소가 바뀌기 때문에 Public DNS도 사용할 수 없게 된다. 이후 CloudFront에서는   
> 오리진에 접속할 수 없어서, 캐시 기능도 동작하지 않게 된다. 따라서 EC2   
> 인스턴스를 오리진으로 사용할 경우 꼭 Elastic IP를 연결한다.   

이어지는 세부설정이다.   
- Object Caching: 파일의 캐시 유지 시간을 설정한다. 유지 시간이 지나면 Cloud  
Front에서 파일이 삭제된다. 기본값 그대로 사용한다(캐시 정책 및 출처 요청 정책  
선택).      
  <img src="https://user-images.githubusercontent.com/33191974/156494957-05a72617-8d5a-4691-8764-22aa63e51db9.png" width="50%" height="50%"/>    

   - Use Origin Cache Headers: 오리진 HTTP 헤더의 캐시 설정(Cache-Control)을  
   따른다. 각 파일마다 캐시 설정을 다르게 할 수 있는 장점이 있다. 캐시 설정이   
   없으면 기본 캐시 유지 시간은 24시간이다.   
   - Customize: 기본 캐시 유지 시간을 따로 설정한다.   
      - Minimum TTL: 최소 캐시 유지시간이다. 초 단위로 설정해야 한다.   
      이 Minimum TTL 설정 시간과 오리진 HTTP 헤더의 캐시 설정(Cache-Control)   
      시간 중 긴 시간이 적용된다.   
- Forward Cookies: 오리진의 쿠키를 CloudFront를 거쳐 사용자에게 전달할지를   
설정한다. 기본값 그대로 사용한다.   
   - None: 쿠키를 전달하지 않는다. 캐시 성능이 좀 더 향상된다.   
   - Whitelist: 쿠키를 선별하여 전달한다.   
      - Whitelist Cookies: 전달할 쿠키 이름을 설정한다. 각 쿠키는 새 줄로   
      구분한다.   
- Forward Query Strings: CloudFront에서 오리진으로 쿼리 문자열을 전달한다.   
오리진에서 쿼리 문자열에 따라 파일을 구분하여 보여주고 싶을 때 설정한다.   
설정하지 않으면 캐시 성능이 향상된다. 기본값 그대로 사용한다(지금은 없는듯).    
- Smooth Streaming: 실시간 스트리밍 프로토콜인 Microsoft Streaming을 사용하고  
싶을 때 설정한다. 기본값 그대로 사용한다.  
- Restrict Viewer Access: Signed URL로 CloudFront 사용을 제한하고 싶을 때   
설정한다. Signed URL에 대해서는 뒤에서 자세히 설명한다. 기본값 그대로 사용한다.     
- Price Class: 요금 수준이다. 에지 로케이션 사용 범위를 설정하는데 실제   
서비스에서 그다지 필요없는 지역을 제외할 때 설정한다. 세부적으로 설정할 수는  
없으며 3가지 옵션이 있다. 기본값 그대로 사용한다.   
   - Use Only US and Europe : 미국과 유럽의 에지 로케이션만 사용한다.   
   - Use Only US, Europe and Asia: 미국과 유럽, 아시아의 에지 로케이션만  
   사용한다.   
   - Use All Edge Location: 모든 에지 로케이션을 사용한다. 위의 두 옵션보다는   
   요금이 많이 나온다.   
- Alternate Domain Names: Route 53에서 도메인을 연결하려면 이 부분을 설정해야   
한다. 여러 도메인이라면 새 줄로 구분하고 최대 10개까지 설정할 수 있다.   
각자 구입한 도메인 이름을 설정하면 된다. 기본값 그대로 비워둔다.   
- SSL Certificate: HTTPS 프로토콜을 사용하기 위한 인증서 설정이다. 기본값   
그대로 사용한다.   
  <img src="https://user-images.githubusercontent.com/33191974/156496469-76d04d91-fd26-4bf4-b7c7-0adacbdea9d6.png" width="50%" height="50%"/>   

   - Default CloudFront Certificate: CloudFront의 인증서를 사용한다.   
   - Custom SSL Certificate: 사용자가 구입한 도메인과 인증서를 사용하고 싶을   
   때 설정한다. 인증서 저장은 IAM에서 할 수 있다.   
- Custom SSL Client Support: 커스텀 SSL 클라이언트 설정이다. 앞에서 Custom  
SSL Certificate를 설정해야 한다.   
   - All Clients: 전용 IP 사용자 지정 SSL 설정이다.   
   - Only Clients that Support Server Name Indication: 서버 이름 표시(SNI)를   
   설정한다.   
- Default Root Object: CloudFront 배포 도메인의 최상위(Root)로 접속했을 때   
기본적으로 보여줄 파일 이름이다. index.html로 설정한다(EC2 인스턴스에서    
사용하는 서버 사이드 프로그래밍 언어에 따라 index.php, index.aspx, index.jsp  
등도 가능하다).  
  <img src="https://user-images.githubusercontent.com/33191974/156497745-b09cd42f-64c6-4375-80a3-4ca7c5f9f1f0.png" width="50%" height="50%"/>    

- Logging: CloudFront 접속 로그 설정이다. 기본값 그대로 사용한다.   
   - Bucket for Logs: CloudFront 로그를 저장할 S3버킷을 선택한다.    
   - Log Prefix: S3 버킷에 로그를 저장할 때, 디렉터리 명을 설정한다.   
- Comment: 메모이다. 추가적인 설명을 기록하고 싶을 때 사용한다. 기본값 그대로   
비워둔다.   
- Distribution State: 배포를 생성한 뒤 배포 상태 설정이다. Enabled로 설정하면   
곧바로 사용할 수 있는 상태가 되며 Disabled로 설정하면 그냥 배포만 생성하고   
비활성화 상태로 둔다. 기본값 그대로 사용한다(현재는 없는듯).  













































