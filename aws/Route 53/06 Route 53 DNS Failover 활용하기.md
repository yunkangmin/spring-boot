# 정리
ELB와 Route 53 Failover 차이는?  
- ELB는 한 곳에 집중되는 HTTP, TCP, SSL 트래픽을 여러 EC2 인스턴스로  
분산한다. 그리고 서버가 정상적으로 가동중인지 확인(Health Check)하여   
일부 EC2 인스턴스가 중단되더라도 트래픽을 정산 EC2 인스턴스로만  
보낸다. 
- Route 53 Failover는 장애가 발생한 서버의 IP 주소 또는 도메인(ELB)은  
알려주지 않는 기능이다. 따라서 장애가 발생하여 동작하지 않는 서버에는   
트래픽이 가지 않는다.  

- 프로세스 예시 
1. slkjfd.cf 접속
2. Route 53에서 Failover 설정 기능에 대한 Primary 상태검사후 정상이면
Primary(EC2 or ELB) IP 주소를 알려줌 or 비정상이면 Secondary(EC2 or  
ELB) IP 주소를 알려줌
3. EC2 IP 주소면 바로 응답결과 반환 or ELB면 ELB 내에서 연결된 EC2  
인스턴스들의 상태검사후 정상인 EC2 IP 주소로 연결하여 응답결과를  
반환한다.   

---


Route 53의 DNS Failover 기능을 활용해보자. DNS Failover 기능이 동작하는지   
확인하기 위해 EC2 인스턴스 2개가 필요하다. 'EC2 인스턴스 생성하기'를 참조하여   
EC2 인스턴스를 2개 생성한다. 그리고 '고정 IP를 제공하는 Elastic IP'를 참조하여  
EC2 인스턴스에 Elastic IP를 연결한다.  
  
이미 EC2 인스턴스(Example Server)를 만들어 놓았다면 추가로 하나 더 생성한다.   
그리고 'EC2와 CloudFront 연동하기'를 참조하여 EC2 인스턴스에 Node.js를   
설치하고 웹 서버를 실행할 수 있도록 준비한다. 이번 실습은 EC2 인스턴스   
생성, 웹 서버 설치 등 생략되는 부분이 많이 때문에 앞 부분을 학습한 뒤    
진행하는 것을 권장한다.  
  
첫 번째 EC2 인스턴스를 Primary로 사용한다. EC2 인스턴스(Primary)에 접속하여   
텍스트 편집기를 열고 다음과 같이 작성한 뒤 app.js로 저장한다.   
  
app.js  
```
var express = require('express');
var app = express();

app.get(['/', '/index.html'], function (req, res) {
  res.send('EC2 Primary');
});

app.listen(80);
```
파일 저장이 끝났으면 다음 명령을 입력해 서버를 실행한다.  
```
sudo node app.js
```
EC2 인스턴스 목록에서 EC2 인스턴스(Primary)의 공인 IP 주소를 확인한다.   
    
<img src="https://user-images.githubusercontent.com/33191974/159402666-f35cf73c-b4ff-4d74-a27b-80fb417248b7.png" width="50%" height="50%"/>  
  
두번째 인스턴스는 Failover 기능을 위한 Secondary로 사용한다. EC2 인스턴스( 
Secondary)에 접속하여 텍스트 편집기를 열고 다음과 같이 작성한 뒤 app.js로   
저장한다.   
  
app.js   
```
var express = require('express');
var app = express();

app.get(['/', '/index.html'], function (req, res) {
  res.send('EC2 Secondary');
});

app.listen(80);
```
파일 저장이 끝났으면 다음 명령을 입력해 서버를 실행한다.   
```
sudo node app.js
```
EC2 인스턴스 목록에서 EC2 인스턴스(Secondary)의 공인 IP 주소를 확인한다.   

<img src="https://user-images.githubusercontent.com/33191974/159403029-9b483bed-6d0e-4e41-912f-6223b29681c6.png" width="50%" height="50%"/>   
  
Route 53 메인 페이지에서 왼쪽 Health Checks를 클릭하고, Create Health  
Check 버튼을 클릭한다.  
  
<img src="https://user-images.githubusercontent.com/33191974/159403231-1e01f2e0-285f-48d5-b925-9f86ad989117.png" width="50%" height="50%"/>     
  
EC2 인스턴스(Primary)에 대한 Health Check를 생성한다.  
- Name: 서버 동작 상태 체크의 이름이다. 기본값 그대로 비워둔다.  
- Protocol: 서버 동작 상태를 체크할 프로토콜을 설정한다. HTTP, HTTPS, TCP를   
선택할 수 있다. 기본값 그대로 사용한다.  
- Specify Endpoint By: 서버 동작 상태를 체크할 때 IP 주소로 할지, 도메인으로  
할지 설정한다. 기본값 그대로 사용한다.   
- IP Address: 서버 동작 상태를 체크할 때 접근할 IP 주소를 설정한다. EC2   
인스턴스(Primary)의 공인 IP 주소를 입력한다.  
- Host Name: 동작 상태를 체크하는 서버의 이름이다. 입력하지 않아도 상관없다.  
- Port: 서버 동작 상태를 체크할 때 접근할 포트 번호이다. 기본값 그대로 사용한다.   
- Path: 서버 동작 상태를 체크할 때 접근할 파일의 URL이다. /index.html, /hello  
/healthcheck.html과 같이 입력하면 된다. 기본값 그대로 비워둔다.   
- Request Interval: 서버 동작 상태 주기를 설정한다. 30초와 10초를 선택할  
수 있다. 여기서는 10초를 선택한다.  
- Failure Threshold: 서버 접근에 실패했을 때 재시도 횟수이다. 기본값 그대로  
사용한다.  
- Enable String Matching: 서버에서 출력하는 파일의 내용(body)에서 특정 문자열로   
상태를 판단할지 설정한다. 기본값 그대로 사용한다.   
- Search String: 파일 내용에서 검색할 문자열을 설정한다.  
- URL: 서버 동작 상태를 체크할 때 접근할 URL이다. 위에서 설정한 내용대로   
자동 생성된다.   
- Health Check Type: 서버 동작 상태 체크 종류이다. 위에서 설정한 내용대로   
자동 생성된다. 요금에 관해서는 View Pricing 링크를 참조하기 바란다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.   
    
<img src="https://user-images.githubusercontent.com/33191974/159408201-3f6f8b74-9bda-4a30-8ba6-f1f3c4921acf.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/159408253-729b149b-1d6a-411c-9199-947d10b7ccc6.png" width="50%" height="50%"/>     
<img src="https://user-images.githubusercontent.com/33191974/159403891-33d665b6-82bf-4b34-aef7-e14859ba23c7.png" width="50%" height="50%"/>      
   
Route 53 Health Check 목록에 Health Check가 생성되었다.   
<img src="https://user-images.githubusercontent.com/33191974/159404056-f71aec47-5060-4190-9fe1-e566a1a9e94c.png" width="50%" height="50%"/>      
  
[여기](https://github.com/yunkangmin/spring-boot/blob/main/aws/Route%2053/02%20Route%2053%20Hosted%20Zone%20%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0.md)를 참조하여 Hosted Zones을 생성하자.  
왼쪽 메뉴에서 Hosted Zones를 클릭하여 Hosted Zone 목록으로 이동한다.  
그리고 도메인을 선택하고 위쪽 Go to Record Sets 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159404760-ace214bd-3f95-4002-841c-ea736ec47d37.png" width="50%" height="50%"/>   
도메인의 레코드 목록이 표시된다. 위쪽 Create Record Set 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159404922-dd34acd1-a945-4259-9f09-2da82fe83e7e.png" width="50%" height="50%"/>   

EC2 인스턴스(Primary)에 대한 A 레코드를 생성한다.   
- Name: 생성할 도메인 이름을 설정한다. examplefailover를 입력한다.   
- Type: 레코드 종류를 설정한다. 기본값 그대로 A - Ip4 address를   
선택한다.  
- Alias: IP 주소를 사용할 것이므로 기본값 그대로 사용한다.  
- TTL: Time To Live의 약자이며 A 레코드가 갱신되는 주기를 설정한다.  
초 단위로 설정한다. DNS Failover 기능은 60초를 입력한다.   
- Value: 도메인 네임을 쿼리했을 때 알려줄 IP 주소를 설정한다. EC2   
인스턴스(Primary)의 공인 IP 주소를 입력한다.   
- Routing Policy: 라우팅 정책을 설정한다. Failover를 선택한다.  
- Failover Record Type: 현재 레코드가 Primary인지, Secondary인지   
설정한다. Primary이므로 기본값 그대로 사용한다.  
- Set ID: Failover 레코드끼리 서로 구분하는 ID이다. 기본값 그대로    
사용한다.   
- Associate with Health Check: Health Check 설정과 연동할지 설정한다.   
Yes를 선택한다.   
- Health Check to Associate: 현재 생성되어 있는 모든 Health Check   
목록이 표시된다. 방금 생성한 EC2 인스턴스(Primary) Health Check를  
선택한다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.   
<img src="https://user-images.githubusercontent.com/33191974/159405951-9f3b01a9-0961-49dc-b132-c452f267c58c.png" width="50%" height="50%"/>   
  
도메인의 Primary A 레코드가 생성되었다. 위쪽 Create Record Set 버튼을   
클릭하여 Secondary A 레코드를 생성한다.   
<img src="https://user-images.githubusercontent.com/33191974/159406141-aa26a4a0-935d-478a-93c1-708e3af65d28.png" width="50%" height="50%"/>     

EC2 인스턴스(Secondary)에 대한 A 레코드를 생성한다.   
- Name: 생성할 도메인 이름을 설정한다. examplefailover를 입력한다.   
- Type: 레코드 종류를 설정한다. 기본값 그대로 A - IPv4 address를   
선택한다.   
- Alias: IP 주소를 사용할 것이므로 기본값 그대로 사용한다.  
- TTL: Time To Live의 약자이며 A 레코드가 갱신되는 주기를 설정한다.  
초 단위로 설정한다. DNS Failover 기능은 60초를 권장하고 있으므로 60을   
입력한다.   
- value: 도메인 네임을 쿼리했을 때 알려줄 IP 주소를 설정한다. EC2   
인스턴스(Secondary)의 공인 IP 주소를 입력한다.   
- Routing Policy: 라우팅 정책을 설정한다. Failover를 선택한다.   
- Failover Recodr Type: 현재 레코드가 Primary인지, Secondary인지 설정  
한다. Secondary이므로 Secondary를 선택한다.   
- Set ID: Failover 레코드끼리 서로 구분하는 ID이다. 기본값 그대로   
사용한다   
- Associate with Health Check: Health Check 설정과 연동할지 설정한다.   
Secondary는 Health Check가 필요 없으므로 기본값 그대로 No를 선택한다.   
  
설정이 완료되었으면 Create 버튼을 클릭한다.  
<img src="https://user-images.githubusercontent.com/33191974/159406957-0514f945-3a3e-4f0e-bac5-584e145756b3.png" width="50%" height="50%"/>     
  
도메인의 Secondary A 레코드가 생성되었다.   
<img src="https://user-images.githubusercontent.com/33191974/159407050-aad6d034-e422-4e84-a686-e690cc12b479.png" width="50%" height="50%"/>    
  
이제 웹 브라우저로 Failover 기능을 설정한 도메인에 접속한다. EC2 인스  
턴스(Primary)의 내용이 표시된다.   
<img src="https://user-images.githubusercontent.com/33191974/159407197-35ec7a57-a27b-444b-9ec8-d5ac95f3e468.png" width="50%" height="50%"/>   
  
이제 EC2 인스턴스(Primary)의 웹 서버(Node.js)를 종료하고, 약 1분 정도  
후에 새로고침을 해보자. Failover 기능이 잘 동작하여 EC2 인스턴스(Secon  
dary)의 내용이 표시된다.  
  
<img src="https://user-images.githubusercontent.com/33191974/159408829-597a13aa-1bd4-4f72-9d90-ad568366b055.png" width="50%" height="50%"/>   
  
접속이 안 될때는 EC2 인스턴스의 Security Group에 80번 포트가 열려   
있는지 확인한다. 웹 브라우저에서 각 IP 주소로 접속해서 확인하면 된다.  
  
> #### DNS Failover와 ELB   
> ELB를 사용할 때는 A 레코드의 Alias를 사용하면 된다. ELB 접속 주소는  
> IP 주소가 아닌 도메인으로 제공된다. A 레코드 Alias는 IP 주소 대신   
> 도메인에 연결하는 기능이다.  














































