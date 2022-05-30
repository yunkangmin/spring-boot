# 정리
- 예비요청에 대한 응답 코드 200(응답코드가 200(에러코드가 아니라)),    
콘솔창에는 빨갛게 에러표시  
-> 빨갛게 에러표시가 된 것은 CORS 정책 위반으로 인한 에러표시(   
브라우저에서 예비 요청에 대한 응답을 받고 CORS 에러라고 판단하고  
보여주는 에러)이고 예비 요청에 대한 응답자체는 성공했다는 것이다.    
브라우저에서는 이 예비요청에 대한 응답을 받고 CORS 에러를 표시한다.  
  
- 예비요청에 대한 응답 코드 200, CORS 정책 위반일 수도 있고 
예비요청에 대한 응답 코드 200이 아님, CORS 정책 위반이 아닐수도 있음.  
  
- 서버에 CORS 정책이 설정되어 있지 않으면 브라우저 자체적으로 응답을   
분석하여 CORS 정책에 위반되면 응답을 버린다. 서버에 CORS 정책이 설정    
되어 있다면 서버에서 보내준 응답을 분석하여 CORS 정책 위반여부를     
판단한뒤 응답을 보여줄지 말지를 결정한다.  
  
- Credentialed Request 시나리오    
same-origin은 같은 출처 간 요청에만 인증 정보를 담을 수 있다. 
요청을 보낼 때 요청을 보낼 서버주소가 같다면 인증정보를 담아 보낸다.  
만약 서버주소와 현재 요청을 보내는 주소가 다르다면 리소스 요청시   
인증정보가 포함되지 않는다.    
credentials: include 옵션을 사용한다면 CORS 위반 여부 규칙에 2가지가  
추가된다. **일반적으로는 Preflight Request를 사용한다.**   
- Preflight Request  시나리오  
브라우저(예비요청) -> 서버(응답, 헤더에 Access-Control-Allow-Origin 값을   
포함) -> 브라우저(예비 요청의 Origin과 응답의 Access-Control-Allow-Origin  
비교한 후 안전하다고 판단되면 본 요청, 안전하지 않다고(출처가다르면)     
판단되면 에러발생) -> 서버(최종 응답) 
  
- 브라우저가 판단하는게 중요하다. 브라우저가 CORS 정책 위반 여부에 따라    
응답을 버리기 때문이다.   
  
- 프런트엔드 프레임워크(Nodejs) 사용시  
   - 프로덕션 
   요청 출처와 서버 출처가 같은 경우에 사용  
   a에서 a로 요청 O  
   a에서 b로 요청  

   - 로컬
   localhost:3000(localhost:3000/api로 요청(Access-Control-Allow-Origin에    
   localhost:3000 설정이 안되어 있음) -> 웹팩이 `https://api.evan.com`으로    
   요청을 프록싱해줌. 프록싱을 통해 CORS 정책을 우회.  
      
   프록시도 로컬 내부에 있다. api 서버만 따로 있다. 즉, 로컬에서 요청을 하면      
   내부에 있는 프록시가 받아서 api서버로 요청하는 프로세스.   
       
   - 상용   
   어플리케이션 소스를 서빙하는 출처(프록싱), API 서버의 출처(서버)이 같아야함  
   아래는 다른 경우  
   https://www.evan.com https://api.evan.com   
   
---

교차출처 -> 다른 출처  

# 출처(Origin)란?  
서버의 위치를 의미하는 https://google.com과 같은 URL들은 마치 하나의 문    
자열 같아 보여도, 사실은 여러 개의 구성요소로 이루어져 있다.   
- https:// -> Protocol
- www.evan-moon.com -> Host
- /users -> Path
- ?sort-asc&page=1 -> Query String
- #foo = Fragment

출처는 Protocol과 Host 그리고 위에는 나와있지 않지만 :80과 같은 포트 번호    
까지 모두 합친 것을 의미한다. 즉, 서버의 위치를 찾아가기 위해 필요한 가장    
기본적인 것들을 합쳐놓은 것이다.     
  
개발자 도구 콘솔에서 origin을 알아낼 수 있다.    
```
console.log(location.origin); //https://evan-moon.github.io
```
  
웹 생태계에는 다른 출처로의 리소스 요청을 제한하는 것과 관련된 두 가지    
정책이 존재한다. 한 가지는 CORS, 또 한 가지는 SOP(Same-Origin Policy)이다.    
  
SOP는 같은 출처에서만 리소스를 공유할 수 있다. 그러나 웹이라는 오픈     
스페이스 환경에서 다른 출처에 있는 리소스를 가져와서 사용하는 일은    
굉장히 흔한 일이라 무작정 막을 수는 없어서 몇 가지 예외 조항을 두고   
이 조항에 해당하는 리소스 요청은 출처가 다르더라도 허용하기로 하는데    
그 중 하나가 "CORS 정책을 지킨 리소스 요청"이다.  
  
우리가 다른 출처로 리소스를 요청한다면 SOP 정책을 위반한 것이되고,    
거기다가 SOP의 예외 조항인 CORS 정책까지 지키지 않는다면 아예 다른    
출처의 리소스를 사용할 수 없게 되는 것이다.   
    
즉, 이렇게 다른 출처의 리소스를 사용하는 것을 제한하는 행위는 하나의    
정책만으로 결정된 사항이 아니라는 의미가 되며, SOP에서 정의된 예외    
조항과 CORS를 사용할 수 있는 케이스들이 맞물리지 않을 경우에는 아예    
리소스 요청을 할 수 없는 케이스도 존재할 수 있다.    
    
왜 이런 정책을 만들었을까?   
웹에서 돌아가는 클라이언트 어플리케이션은 사용자의 공격에 너무나도    
취약하다. 당장 브라우저의 개발자 도구만 열어도 DOM이 어떻게 작성되어    
있는지, 어떤 서버와 통신하는지, 리소스의 출처는 어디인지와 같은 각종    
정보들을 아무런 제재없이 열람할 수 있다.  
     
최근에는 자바스크립트 소스 코드를 난독화해서 읽기 어렵다고 하지만,    
난독화는 어디까지나 난독화일 뿐이지 암호화가 아니다. 그리고 아무리    
난독화되어있다고 해도 사람이 바로 이해할 수 없는 정도도 아닌데다가    
소스 코드를 직접 볼 수 있다는 것 자체가 보안적으로 상당히 취약한 부분    
이다. 심지어 아직까지도 소스 코드의 난독화가 안되어 개발자 도구만 열면    
`<script>` 태그 안에 날 것 그대로의 소스 코드가 떡하니 노출되느느 사이트    
들도 많다.    
    
이런 상황 속에서 다른 출처의 어플리케이션이 서로 통신하는 것에 대해     
아무런 제약도 존재하지 않는다면, 악의를 가진 사용자가 소스 코드를 쓱    
구경한 후 CSRF(Cross-Site Request Forgery)나 XSS(Cross-Site Scripting)    
와 같은 방법을 사용하여 여러분의 어플리케이션에서 코드가 실행된 것처럼    
꾸며서 사용자의 정보를 탈취하기가 너무나도 쉬워진다.  
  
# 같은 출처와 다른 출처의 구분
사실 두 개의 출처가 서로 같다고 판단하는 로직 자체는 굉장히 간단한데, 두 URL의  
구성요소 중 Scheme, Host, Port 이 3가지만 동일하면 된다.  
  
`https://evan-moon.github.io:80`라는 출처를 예로 들면 `https://`라는 스킴에  
`evan-moon.githbub.io` 호스트를 가지고 `:80`번 포트를 사용하고 있다는 것만  
같다면 나머지는 전부 다르더라도 같은 출처로 인정이 된다는 것이다.
  
필자의 블로그 출처인 `https://evan-moon.github.io`와 같은 출처로 인정되는  
예시는 대략 이런 느낌이다.  
(URL | 같은 출처 | 이유)  
- https://evan-moon.github.io/about | O | 스킴, 호스트, 포트가 동일  
- https://evan-mmon.github.io/about?q=cors | O | 스킴, 호스트, 포트가 동일  
- https://user:password@evan-moon.github.io | O | 스킴, 호스트, 포트가 동일  
- http://evan-moon.github.io | X | 스킴이 다름
- https://api.github.io | X | 호스트가 다름
- https://evan-moon.naver.com | X | 호스트가 다름
- https://evan-moon.github.io:8000 | ? | 브라우저의 구현에 따라 다름

맨 마지막에 있는 케이스의 경우, 만약 출처에 `https://evan-moon.github.io:80` 처럼  
포트 번호가 명시되어 있다면 명백하게 다른 출처로 인정되는 부분이지만, 예시로  
든 출처의 경우 포트 번호가 포함되지 않았기 때문에 판단하기가 애매하다. 이런 경우  
각 브라우저들의 독자적인 출처 비교 로직을 따라가게 된다.  
  
**여기서 중요한 사실 한 가지는 이렇게 출처를 비교하는 로직이 서버에 구현된 스펙이  
아니라 브라우저에 구현되어 있는 스펙이라는 것이다.**  
  
만약 우리가 CORS 정책으 위반하는 리소스 요청을 하더라도 해당 서버가 같은 출처에서  
보낸 요청만 받겠다는 로직을 가지고 있는 경우가 아니라면 서버는 정상적으로  
응답을 하고, 이 후 브라우저가 이 응답을 분석해서 CORS 정책 위반이라고 판단되면   
그 응답을 사용하지 않고 그냥 버리는 순서인 것이다.   
서버는 CORS를 위반하더라도 정상적으로 응답을 해주고, 응답의 파기 여부는 브라우저가  
결정한다.  
<img src="https://user-images.githubusercontent.com/33191974/153001026-41315e5f-3f85-464c-b7b7-56a72a26826c.png" width="50%" height="50%"/>      
**즉, CORS는 브라우저의 구현 스펙에 포함되는 정책이기 때문에, 브라우저를 통하지  
않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다. 또한 CORS 정책을 위반하는  
리소스 요청 때문에 에러가 발생했다고 해도 서버 쪽 로그에는 정상적으로 응답을  
했다는 로그만 남기 때문에 CORS가 돌아가는 방식을 정확히 모르면 에러 트레이싱에  
난항을 겪을 수도 있다.**  

# CORS는 어떻게 동작할까?  
본격적으로 어떤 방법을 통해 서로 다른 출처를 가진 리소스를 안전하게 사용할 수   
있는지 알아보도록 하자.  
  
기본적으로 웹 클라이언트 어플리케이션이 다른 출처의 리소스를 요청할 때는 HTTP  
프로토콜을 사용하여 요청을 보내게 되는데, 이 때 브라우저는 요청 헤더에 `Origin`  
이라는 필드에 요청을 보내는 출처를 함께 담아보낸다.  
```
Origin: https://evan-moon.github.io
```
이 후 서버가 이 요청에 대한 응답을 할 때 응답 헤더의 `Access-Control-Allow-Origin`  
이라는 값에 "이 리소스를 접근하는 것이 허용된 출처"를 내려주고 이 후 응답을 받은  
브라우저는 자신이 보냈던 요청의 `Origin`과 서버가 보내준 응답의 `Access-Control-  
Allow-Origin`을 비교해본 후 이 응답의 유효한 응답인지 아닌지를 결정한다.  
  
**기본적인 흐름은 이렇게 간단하지만, 사실 CORS가 동작하는 방식은 한 가지가 아니라  
세 가지의 시나리오에 따라 변경되기 때문에 여러분의 요청이 어떤 시나리오에 해당  
되는지 잘 파악한다면 CORS 정책 위반으로 인한 에러를 고치는 것이 한결 쉬울 것이다.**  

## Preflight Request
프리플라이트(Preflight) 방식은 일반적으로 우리가 웹 어플리케이션을 개발할 때  
가장 마주치는 시나리오이다. 이 시나리오에 해당하는 상황일 때 브라우저는 요청을  
한 번에 보내지 않고 예비 요청과 본 요청으로 나누어서 서버로 전송한다.  
  
**이 때 브라우저가 본 요청을 보내기 전에 보내는 예비 요청을 Preflight라고 부르는  
것**이며, 이 예비 요청에는 HTTP 메소드 중 OPTIONS 메소드가 사용된다. **예비 요청의  
역할은 본 요청을 보내기 전에 브라우저 스스로 이 요청을 보내는 것이 안전한지  
확인하는 것**이다.  
  
이 과정을 간단한 플로우 차트로 나타내보면 대략 이런 느낌이다.   
브라우저는 본 요청을 보내기 전에 예비 요청을 먼저 보내고, 요청의 유효성을 검사한다.  
<img src="https://user-images.githubusercontent.com/33191974/153131747-f3265d84-62c6-4095-bf94-385a8dd28776.png" width="50%" height="50%"/>    
우리가 자바스크립트의 fetch API를 사용하여 브라우저에게 리소스를 받아오라는  
명령을 내리면 브라우저는 서버에게 예비 요청을 먼저 보내고, 서버는 이 예비 요청  
에 대한 응답으로 현재 자신이 어떤 것들을 허용하고, 어떤 것들을 금지하고 있는지  
에 대한 정보를 응답 헤더에 감아서 브라우저에게 다시 보내주게 된다.  
  
이 후 브라우저는 자신이 보낸 예비 요청과 서버가 응답에 담아준 허용 정책을 비교  
한 후, 이 요청을 보내는 것이 안전하다고 판단되면 같은 엔드포인트로 다시 본  
요청을 보내게 된다. 이 후 서버가 이 본 요청에 대한 응답을 하면 브라우저는  
최종적으로 이 응답 데이터를 자바스크립트에게 넘겨준다.    
  
이 플로우는 브라우저의 개발자 도구 콘솔에서도 간단하게 재현해볼 수 있는데,  
필자의 블로그 환경에서 필자의 티스토리 블로그의 RSS 파일 리소스에 요청을 보내면  
브라우저가 본 요청을 보내기 전 OPTIONS 메서드를 사용하여 예비 요청을 보내는   
것을 확인할 수 있다.   
```
const headers = new Headers({
  'Content-Type': 'text/html',
});
//fetch('https://evan-moon.github.io', { headers });
fetch('https://evanmoon.tistory.com/rss', { headers });
```
```
//브라우저가 보낸 요청
OPTIONS https://evanmoon.tistory.com/rss

Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US, en; q=0.0, ko; q=0.8, ja; q=0.7, la; q=0.6
Access-Control-Request-Headers: content-type
Access-Control-Request-Method: GET
Connection: keep-alive
Host: evanmoon.tistory.com
Origin: https://evan-moon.github.io
Referer: https://evan-moon.github.io/2020/05/21/about-cors/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
```
실제로 브라우저가 보낸 요청을 보면, 단순히 Origin에 대한 정보뿐만 아니라  
자신이 예비 요청 이후에 보낼 본 요청에 대한 다른 정보들도 함께 포함되어 있는  
것을 볼 수 있다.  
  
이 예비 요청에서 브라우저는 Access-Control-Request-Headers를 사용하여 자신이  
본 요청에서 Content-Type 헤더를 사용할 것을 알려주거나, Access-Control-  
Reqeust-Method를 사용하여 이후 GET 메서드를 사용할 것을 서버에게 미리 알려주고  
있는 것이다.  
  
이렇게 티스토리 서버에 예비 요청을 보내면, 이제 티스토리 서버가 이 예비 요청에  
대한 응답을 보내준다.   
예비 요청에 대한 응답  
```
OPTIONS https://evanmoon.tistory.com/rss 200 OK

Access-Control-Allow-Origin: https://evanmoon.tistory.com
Content-Encoding: gzip
Content-Length: 699
Content-Type: text/html; charset=utf-8
Date: Sun, 24 May 2020 11:52:33 GMT
P3P: CP='ALL DSP MON LAW OUR LEG DEL'
Server: Apache
Vary: Accept-Encoding
X-UA-Compatible: IE-Edge
```
여기서 우리가 눈여겨 봐야할 것은 서버가 보내준 응답 헤더에 포함된 Access-  
Control-Allow-Origin: https://evanmoon.tistory.com라는 값이다.  
  
티스토리 서버는 이 리소스에 접근이 가능한 출처는 오직 https://evanmoon.  
tistory.com 뿐이라고 브라우저에게 이야기해준 것이고, 필자가 이 요청을 보낸  
출처는 https://evan-moon.github.io이므로 서버가 허용해준 출처와는 다른  
출처이다.  
  
결국 브라우저는 이 요청이 CORS 정책을 위반했다고 판단하고 다음과 같은 에러를  
뱉게 되는 것이다.   
```
Access to fetch at 'https://evanmoon.tistory.com/rss' from origin   
'https://evan-moon.github.io' has been blocked by CORS policy: Response to   
preflight request doesn't pass access control check: The 'Access-Control-Allow-Origin'  
header has a value 'https://evanmoon.tistory.com' that is not equal to the supplied   
origin. Have the server send the header with a valid value, or, if an opaque response  
serves your needs, set the request's mode to 'no-cors' to fetch the resource with   
CORS disabled.
```
이 때 예비 요청에 대한 응답에서 에러가 발생하지 않고 정상적으로 200이 떨어  
졌는데, 콘솔 창에는 빨갛게 에러가 표시되기 때문에 많은 사람들이 헷갈려하는데  
CORS 정책 위반으로 인한 에러는 예비 요청의 성공 여부와 별 상관이 없다.   
**브라우저가 CORS 정책 위반 여부를 판단하는 시점은 예비 요청에 대한 응답을 받은  
이후이기 때문이다.**  
  
물론 예비 요청 자체가 실패해도 똑같이 CORS 정책 위반으로 처리될 수 있지만,  
중요한 것은 예비 요청의 성공/실패 여부가 아니라 "응답 헤더에 유효한 Access-  
Control-Allow-Origin 값이 존재하는가"이다. 만약 예비 요청이 실패해서 200이  
아닌 상태코드가 내려오더라도 헤더에 저 값이 제대로 들어가 있다면 CORS 정책  
위반이 아니라는 의미이다.  
   
대부분의 경우 이렇게 예비 요청, 본 요청을 나누어 보내는 프리플라이트 방식을  
사용하기는 하지만, 모든 상황에서 이렇게 두 번씩 요청을 보내는 것은 아니다.  
조금 까다로운 조건이기는 하지만 어떤 경우에는 예비 요청없이 본 요청만으로  
CORS 정책 위반 여부를 검사하기도 한다.  
  
## Simple Request
이 시나리오에 대한 정식 명칭은 없지만 MDN의 CORS 문서에서는 이 시나리오를  
Simple Request라고 부르고 있으니, 필자도 그냥 단순 요청이라고 부를 것이다.   
  
단순 요청은 예비 요청을 보내지 않고 바로 서버에게 본 요청부터 때려박은 후,  
서버가 이에 대한 응답의 헤더에 Access-Control-Allow-Origin과 같은 값을  
보내주면 그 때 브라우저가 CORS 정책 위반 여부를 검사하는 방식이다. 즉, 프리  
플라이트와 단순 요청 시나리오는 전반적인 로직 자체는 같되, 예비 요청의 존재  
유무만 다른다.  
단순 요청은 예비 요청없이 바로 본 요청만 보내는 시나리오이다.   
<img src="https://user-images.githubusercontent.com/33191974/153139349-d3189e96-0374-434f-bfd2-b3c2b35ee6b0.png" width="50%" height="50%"/>  
하지만 아무때나 단순 요청을 사용할 수 있는 것은 아니고, 특정 조건을 만족하는  
경우에만 예비 요청을 생략할 수 있다. 이 조건이 조금 까다롭기 때문에 일반적인  
방법으로 웹 어플리케이션 아키텍처를 설계하게 되면 거의 충족시키기 어려운 조건  
들이라 필자도 이런 경우를 거의 경험하지는 못했다.  
1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 한다.
2. Accept, Accept-Language, Control-Language, Content-Type, DPR, Downlink,  
Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 된다.   
3. 만약 Content-Type를 사용하는 경우에는 application/x-www-form-urlencoded,  
multipart/form-data, text/plain만 허용된다.  
사실 1번 조건의 경우은 그냥 PUT이나, DELETE 같은 메서드를 사용하지 않으면  
되는 것 뿐이니 그렇게 보기 드문 상황은 아니지만, 2번이나 3번 조건같은 경우는   
조금 까다롭다.  
  
애초에 저 조건에 명시된 헤더들은 기본적인 헤더들이기 때문에, 복잡한 상용 웹  
어플리케이션에서 이 헤더들 외에 추가적인 헤더를 사용하지 않는 경우는 드물다.   
당장 사용자 인증에 사용되는 Authorization 헤더 조차 저 조건에는 포함되지  
않는다.   
   
게다가 대부분의 HTTP API는 text/xml이나 application/json 컨텐츠 타입을  
가지도록 설계되기 때문에 사실 상 이 조건들을 모두 만족시키는 상황을 만들기는  
그렇게 쉽지 않은 것이 현실이다.   

## Credentialed Request
**3번째 시나리오는 인증된 요청을 사용하는 방법이다.** 이 시나리오는 CORS의 기본적인  
방식이라기보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는  
방법이다.       
  
기본적으로 브라우저가 제공하는 비동기 리소스 요청 API인 XMLHttpRequest 객체나  
fetch API는 별도의 옵션없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로  
요청에 담지 않는다. 이 때 요청에 인증과 관련된 정보를 담을 수 있게 해주는   
옵션이 바로 credentials 옵션이다.   
  
이 옵션에는 총 3가지의 값을 사용할 수 있으며, 각 값들이 가지는 의미는 다음과   
같다.  
(옵션 | 설명)   
- same-origin(기본값) | 같은 출처 간 요청에만 인증 정보를 담을 수 있다.  
- include | 모든 요청에 인증 정보를 담을 수 있다.   
- omit | 모든 요청에 인증 정보를 담지 않는다.   
만약 여러분이 same-origin이나 include와 같은 옵션을 사용하여 리소스 요청에  
인증 정보가 포함된다면, 이제 브라우저는 다른 출처의 리소스를 요청할 때 단순히  
Access-Control-Allow-Origin만 확인하는 것이 아니라 좀 더 빡빡한 검사 조건을  
추가하게 된다.  
  
백문이불여일견이니 필자가 지금 이 포스팅을 작성하고 있는 로컬 환경과 필자의  
블로그를 호스팅하고 있는 Github 서버와의 통신을 통해, 어떤 제약이 추가되었는지  
직접 살펴보는 것이 좋을 것 같다.  
  
필자의 블로그는 Access-Control-Allow-Origin 값으로 모든 출처를 허용한다는   
의미인 \*가 설정되어 있기 때문에, 다른 출처에서 필자의 블로그로 리소스를 요청할  
때 CORS 정책 위반으로 인한 제약을 받지 않는다.   
  
그래서 http://localhost:8000과 같은 로컬의 개발 환경에서도 fetch API를    
사용하여 마음대로 리소스를 요청하고, 또 받아올 수 있다.   
```
fetch('https://evan-moon.github.io/feed.xml');
```
```
Request
GET https://evan-moon.github.io/feed.xml

Origin: http://localhost:8000
Referer: http://localhost:8000/2020/05/21/about-cors/
```
```
Response
GET https://evan-moon.github.io/feed.xml 200 OK

Access-Control-Allow-Origin: *
Content-Encoding: gzip
Content-Length: 1132748
Content-Type: application/xml
Server: GitHub.com
Status: 200
```
또한 구글 크롬 브라우저의 credentials 기본 값은 같은 출처 내에서만 인증 정보를  
사용하겠다는 same-origin이기 때문에, 필자의 로컬 환경에서 https://evan-moon.  
github.io로 보내는 리소스 요청에는 당연히 브라우저의 쿠키와 같은 인증 정보가  
포함되어 있지 않다.  
  
그렇기 때문에 브라우저는 단순히 `Access-Control-Allow-Origin: *`이라는 값만   
보고 "이 요청은 안전하구만"이라는 결론을 내리는 것이다. 그러나 필자가   
credentials 옵션을 모든 요청에 인증 정보를 포함하겠다는 의미를 가진 include로  
변경하고 같은 요청을 보내면 이번에는 상황이 조금 달라진다.   
```
fetch('https://evan-moon.github.io/feed.xml', {
  // Credentials 옵션 변경!
  credentials: 'include',
});
```
직접 브라우저 콘솔에서 실행해보면 알겠지만, 이번에는 credentials: include  
옵션을 사용하여 동일 출처 여부와 상관없이 무조건 요청에 인증 정보가 포함되도록   
설정했으므로, 이번 요청에는 브라우저의 쿠키 정보가 함께 담겨있는 것을 확인할  
수 있다.   
  
필자의 블로그를 호스팅하고 있는 Github 서버는 이번에도 동일한 응답을 보내  
주었지만, 브라우저의 반응은 다르다.   

```
Access to fetch at ’https://evan-moon.github.io/feed.xml’ from origin   
’http://localhost:8000’ has been blocked by CORS policy: The value of the   
‘Access-Control-Allow-Origin’ header in the response must not be the wildcard   
’*’ when the request’s credentials mode is ‘include’.
```
브라우저는 인증 모드가 include 일 경우, 모든 요청을 허용한다는 의미의 `*`를  
`Access-Control-Allow-Origin` 헤더에 사용하면 안된다고 이야기하고 있다.   
  
이처럼 요청에 인증 정보가 담겨 있는 상태에서 다른 출처의 리소스를 요청하게 되면  
브라우저는 CORS 정책 위반 여부를 검사하는 룰에 다음 두 가지를 추가하게 된다.   
1. `Access-Control-Allow-Origin(응답헤더에 있는 것)`에는 `*`를 사용할 수   
없으며, 명시적인 URL이어야 한다.    
2. 응답 헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야  
한다.  
인증까지 얽혀있는 이 시나리오는 다른 시나리오에 비해서 다소 복잡하게 느껴질 수  
있지만, 이렇게 CORS 정책에 대한 다양한 시나리오를 알아두면 실제 상황에서  
CORS 정책 위반으로 인한 문제가 발생했을 경우 삽질해야하는 시간을 크게 단축  
시킬 수 있으니 숙지해놓는 것을 추천한다(하라는 거 다 했는데 왜 안돼? 같은  
상황을 조금은 예방할 수 있다).  
  
# CORS를 해결할 수 있는 방법
지금까지 CORS가 무엇인지, 어떤 상황에서 CORS 정책이 적용되고 위반되는 것인지  
알아봤다면 이번 섹션에서는 실질적으로 CORS 정책 위반으로 인한 문제가 발생했을  
경우에 해결할 수 있는 방법을 알아보도록 하자.   

## Access-Control-Allow-Origin 세팅하기  
CORS 정책 위반으로 인한 문제를 해결하는 가장 대표적인 방법은 그냥 정석대로   
서버에서 `Access-Control-Allow-Origin` 헤더에 알맞은 값을 세팅해주는 것이다.  
  
이 때 와일드카드인 `*`을 사용하여 이 헤더를 세팅하게 되면 모든 출처에서 오는  
요청을 받아먹겠다는 의미이므로 당장은 편할 수 있겠지만, 바꿔서 생각하면 정체도  
모르는 이상한 출처에서 오는 요청까지 모두 받아먹겠다는 오픈 마인드와 다를 것  
없으므로 보안적으로 심각한 이슈가 발생할 수도 있다.  
  
그러니 가급적이면 귀찮더라도 `Access-Control-Allow-Origin: https://evan.git  
hub.io`와 같이 출처를 명시해주도록 하자.   
  
이 헤더는 Nginx나 Apache와 같은 서버 엔진의 설정에서 추가할 수도 있지만  
아무래도 복잡한 세팅을 하기는 불편하기 때문에 소스 코드 내에서 응답   
미들웨어 등을 사용하여 세팅하는 것을 추천한다. Spring, Express, Django와  
같이 이름있는 백엔드 프레임워크의 경우에는 모두 CORS관련 설정을 위한 세팅이나  
미들웨어 라이브러리를 제공하고 있으니 세팅 자체가 어렵지는 않을 것이다.   

## Webpack Dev Server로 리버스 프록싱하기([리버스 프록시](https://ko.wikipedia.org/wiki/%EB%A6%AC%EB%B2%84%EC%8A%A4_%ED%94%84%EB%A1%9D%EC%8B%9C))
사실 CORS를 가장 많이 마주치는 환경은 바로 로컬에서 프런트엔드(nodejs?)  
어플리케이션을 개발하는 경우라고 해도 과언이 아니다. 백엔드에는 이미 `Access-  
Control-Allow-Origin` 헤더가 세팅되어있겠지만, 이 중요한 헤더에다가   
`http://localhost:3000`은 범용적인 출처를 넣어주는 경우는 드물기 때문이다.   
  
프론트 엔드 개발자는 대부분 웹팩과 `webpack-dev-server`를 사용하여 자신의  
머신에 개발 환경을 구축하게 되는데, 이 라이브러리가 제공하는 프록시 기능을   
사용하여 아주 편하게 CORS 정책을 우회할 수 있다.   
```
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://api.evan.com',
        changeOrigin: true,
        pathRewrite: { '^/api': ''},
      },
    }
  }
}
```
이렇게 설정을 해놓으면 로컬 환경에서 `/api`로 시작하는 URL로 보내는 요청에  
대해 브라우저는 `localhost:8000/api`로 요청을 보낸 것으로 알고 있지만, 사실  
뒤에서 웹팩이 `https://api.evan.com`으로 요청을 프록싱해주기 때문에 마치 CORS   
정책을 지킨 것처럼 브라우저를 속이면서도 우리는 원하는 서버와 자유롭게 통신을  
할 수 있다. 즉, 프록싱을 통해 CORS 정책을 우회할 수 있는 것이다.   
  
혹시 `webpack-dev-middlware`와 Node 서버의 조합으로 개발 환경을 직접 구축  
했더라도 `http-proxy-middleware` 라이브러리를 사용하면 손쉽게 프록시 설정을  
할 수 있으니 걱정하지 말자.(`webpack-dev-server`도 내부적으로는 어차피 `
http-proxy-middleware`를 사용한다.)  
  
다만 이 방법은 실제 프로덕션 환경에서도 클라이언트 어플리케이션의 소스를   
서빙하는 출처(프록시)와 API 서버의 출처가 같은 경우에 사용하는 것이 좋다.  
물론 로컬 개발 환경에서야 웹팩이 요청을 프록싱해주니 아무 이상이 없겠지만,   
어플리케이션을 빌드하고 서버에 올리고 나면 더 이상 webpack-dev-server가  
구동하는 환경이 아니기 때문에 프록싱이 안되 이상한 곳으로 API 요청을 보내기   
때문이다.     
  
예를 들어 API 서버의 출처는 `https://api.evan.com`이고 클라이언트 어플리케이션  
을 서빙하는 서버의 출처는 `https://www.evan.com`이라면, 다음과 같은 상황이  
발생한다는 것이다.   
```
fetch('/api/me');
```
```
로컬환경에서는...
GET https://api.evan.com/me 200 OK

실제 서버에는 프록싱 로직이 없음.. 
GET https://www.evan.com/api/me 404 Not Found
```
물론 비즈니스 로직 내에서 `process.env.NODE_ENV`와 같은 빌드 환경 변수를 사용  
하여 분기 로직을 작성하는 방법도 있지만, 개인적으로 비즈니스 로직에 이런 개발  
환경 전용 소스가 포함되는 것은 별로 좋지 않다고 생각해서 피하는 편이다.  

## 요청을 img 태그에 넣으면 어떨까?  
필자는 앞서 SOP(Same-Origin Policy) 정책에는 다른 출처의 리소스에 접근할 수   
있는 몇 가지 예외조항이 존재하고, 그 중 하나가 CORS 정책을 지킨 요청이라고  
이야기했다. 그리고 CORS 정책을 지킨 요청을 제외한 SOP의 예외 조항은 실행 가능한  
스크립트, 렌더될 이미지, 스타일 시트 정도가 있다.  
  
그렇다면 다른 예외 조항이 적용된 요청을 보내면 CORS를 우회할 수 있지 않을까?  
```
<img src="https://evanmoon.tistory.com/rss">
<script src="https://evanmoon.tistory.com/rss"></script>
```
물론 이런 식으로 접근하면 CORS를 위반하지 않고 요청 자체는 성공한다.   
그리고 브라우저의 개발자 도구의 네트워크 탭에서 이 요청들의 헤더를 자세히  
살펴보면 `Sec-Fetch-Mode: no-cors`라는 값이 포함되어 있는 것을 볼 수 있다.  
  
`Sec-Fetch-Mode` 헤더는 요청 모드를 설정하는 필드인데, 브라우저는 이 필드의   
값이 no-cors인 경우에는 다른 출처라고 해도 CORS 정책 위반 여부를 검사하지  
않는다. 하지만 한 가지 슬픈 점은 브라우저가 이 헤더에 값이 포함된 요청의  
응답을 자바스크립트에게 알려주지 않는다는 것이다. 즉, 우리는 코드 레벨에서  
절대 이 응답에 담긴 내용에 접근할 수가 없다.   
  
결론은 CORS 정책을 지키도록 하자. 

# 정리
백엔드에서 CORS 설정을 하는 것이 CORS 정책 위반 에러에 근본적인 해결책이다.   







  
































[참고](https://evan-moon.github.io/2020/05/21/about-cors/)





  


























