# 현재 아래 내용 확인안됨

이제 Signed URL을 직접 생성해보자. Signed URL은 보통 애플리케이션에서 자체적으로   
생성하여 사용한다. 따라서 Java, PHP, C# 등의 프로그래밍 언어로 생성하는 것이   
편리하다. 하지만 프로그래밍 언어로 하나하나 설명하기에는 내용이 너무 길어지므로  
Linux에서 커맨드라인 명령으로 간단하게 생성하는 방법을 알아본다.   
  
먼저 Canned Policy를 사용한 Signed URL 생성 방법을 알아보자. Canned Policy에서   
Canned는 미리 준비되어 있는 상태를 뜻한다. 따라서 CloudFront 배포 서버에서 정책을    
이미 알고 있기 때문에 URL에 정책 내용이 포함되지 않는다.   
   
EC2 인스턴스 혹은 Linux가 설치된 컴퓨터에 SSH로 접속한다. 그리고 텍스트 편집기를   
열고 아래와 같이 작성한 뒤 canned_policy.json로 저장한다.   
- Resource: 필자가 생성한 CloudFront 배포의 도메인은 d3qad395nskve.cloudfront.net  
이다. http://d3qad395nskve.cloudfront.net/test.jpg와 같이 http://를 포함한  
CloudFront 배포 도메인과 파일의 경로를 포함한 파일명을 입력한다.   
- DateLessThan -> AWS: EpochTime: Signed URL에서는 이 만료날짜 값이 필수이다.   
그리고 UTC 형식을 사용해야 한다. 위의 정책 파일에서 1399206886값은 이미 지나가  
버린 시간 값이므로 그대로 사용하면 Signed URL이 동작하지 않는다. 꼭 미래의   
시간 값을 사용하기 바란다.   

canned_policy.json
```
{ 
  "Statement": [{
    "Resource": "http://d3qad395nskve.cloudfront.net/test.jpg",
    "Condition": {
      "DateLessThan": {
        "AWS:EpochTime": 1646462092
      }
    }
  }]
}   
```
> #### 웹에서 UTC 값 생성하기  
> UTC 시간 값을 사람이 직접 계산하는 것은 무리가 있다. 이 값은 프로그래밍 언어를   
> 통하지 않고도 인터넷에서 쉽게 생성할 수 있다.   
> http://www.epochconverter.com에 접속한다.  
> <img src="https://user-images.githubusercontent.com/33191974/156712246-65e703dd-52de-418f-b69c-3a48a254468f.png" width="50%" height="50%"/>  
> 원하는 날짜와 시간을 입력한 뒤 Human date to Timestamp 버튼을 클릭하면 Epoch   
> timestamp에 UTC 형식의 값을 얻을 수 있다. 이 사이트 이외에도 구글에서 utc  
> converter라고 검색하면 같은 기능을 가진 비슷한 사이트가 많이 있다.   

정책 파일의 공백을 모두 제거하고 한 줄로 만든다. 공배과 개행문자(새줄)이 있으면   
Signed URL이 동작하지 않는다. 특히 파일 맨 끝의 개행문자는 꼭 제거해야 한다.   
canned_policy.json   
```
{"Statement":[{"Resource":"http://d3qad395nskve.cloudfront.net/test.jpg","Condition":{"DateLessThan":{"AWS:EpochTime":1646462092}}}]}
```
---

#### Vim에서 공백 및 개행문자 제거하기   

- 공백 삭제하기    
```
:%s/ //g
```
- 개행문자 제거하기(Windows에서는 `\r\n`)  
```
:%s/\n//g
```
- 파일 맨 끝의(EOL) 개행문자 제거하기   
```
:set binary
:set noeol
```

---
공백 개행문자를 제거했으면 `:wq`를 입력하여 파일을 저장하고 Vim을 종료한다.   
다음과 같이 명령을 입력하여 서명값(Signature)을 생성한다.  
```
[ec2-user@ip-172-26-14-145 ~]$ cat canned_policy.js | openssl sha1
 -sign pk.pem | openssl base64 | tr '+=/' '-_~'
RUaMo8Z2EZrVeM-FwihLOTS-Y9wWjkN3WGfQF8tn5eNByVKhTPHdIH4-CTimYJ4E
VkORbl-o7UZ0wBX8Q~XKnX~Q5SJtNKwGkpcPu4KJGZyWBVmzkTe9SF8slIWTJsXO
sEGS7WERiYjYXXCkPMrXSGM6VYEP9u2pDAh0wwjoRWtmnd-MNzu6xVX4iKYXzaP~
A0vIo4Um2xKz03ecYgh7TUKkt0IFIES~LG0AL7tWQiSqr7RgrSsFtBtFlPGrxz~C
3jKxh-uC5w3kMlHvSPPHxRgxOm7ftpZ1oMB9n3fZCiba3CKFRAPcStdnQqo8d2RA
vN2Xo9HSKv~8NRKTBpd21Q__
```
- cat canned_policy.js : 방금 작성한 canned_policy.json 파일이다(json으로  
생성해야 하지만 여기서는 js로 잘못생성함).  
- openssl sha1 -sign pk.pem : cloudfront 키에서 개인키이다. 여러분이 생성한  
개인키 파일을 지정하면 된다.   
- openssl base64 : 서명 값(Signature)를 BASE64로 인코딩한다.   
- `tr '+=/' '-_~'` :BASE64로 인코딩된 값 중에서 URL로 인식될 수 있는 문자를   
다른 것으로 바꾼다.   
  
명령을 입력하면 canned_policy.json의 내용을 서명한 데이터가 출력된다.   
메모장이나 기타 텍스트 편집기에서 아래와 같이 Signed URL을 직접 생성한다.  
- http://d3qad395nskve.cloudfront.net/test.jpg : 접속할 CloudFront 도메인과   
파일명이다. 이 도메인은 여러분들이 생성한 CloudFront 배포 도메인을 입력한다.   
- Expires: Signed URL이 만료되는 날짜와 시간값이다. 여러분이 canned_policy.json에  
지정한 DateLessThan -> AWS:EpochTime 값과 동일해야 한다.   
- Signature: 생성한 서명값을 이 곳에 붙인다. 생성할 때는 여러 줄로 출력이 되지만   
URL로 사용할 때는 한 줄로 만들어서 붙여야 한다.  
- Key-Pair-Id: CloudFront 액세스 키 값이다. 여러분들이 생성한 액세스 키 아이디를  
입력한다.  
```
http://d3qad395nskve.cloudfront.net/test.jpg?Expires=1646462092&Signature=
RUaMo8Z2EZrVeM-FwihLOTS-Y9wWjkN3WGfQF8tn5eNByVKhTPHdIH4-CTimYJ4EVkORbl-o7UZ
0wBX8Q~XKnX~Q5SJtNKwGkpcPu4KJGZyWBVmzkTe9SF8slIWTJsXOsEGS7WERiYjYXXCkPMrXSG
M6VYEP9u2pDAh0wwjoRWtmnd-MNzu6xVX4iKYXzaP~A0vIo4Um2xKz03ecYgh7TUKkt0IFIES~L
G0AL7tWQiSqr7RgrSsFtBtFlPGrxz~C3jKxh-uC5w3kMlHvSPPHxRgxOm7ftpZ1oMB9n3fZCiba
3CKFRAPcStdnQqo8d2RAvN2Xo9HSKv~8NRKTBpd21Q__&Key-Pair_Id=APKA5K5N3XSUOHQCE64E
```
이제 Canned Polic를 사용한 Signed URL이 완성되었다. 웹 브라우저에서 이 URL로   
접속해보자.   





























