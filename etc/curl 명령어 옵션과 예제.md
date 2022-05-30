curl은 오픈소스로 개발되어 윈도우와 리눅스에 기본 설치되고 있는 웹 개발 툴로써  
http, https, ftp, sftps, smtp, telnet등의 다양한 프로토콜과 Proxy, Header, Cookie등의  
세부옵션까지 쉽게 설정할 수 있다.   
  
이러한 장점때문에 Client를 코딩을 시작하기 전에 curl 명령어로 서버 동작을 먼저 확인함으로서  
좀 더 빠르게 개발을 진행할 수 있다.  
  
본 포스팅은 curl 옵션 중 자주 사용하는 옵션 위주로 예제와 설명을 정리했다.  
  
## 연속된 URL로 요청하고 결과 파일로 지정하기 
네이버나 카카오의 HLS 동영상을 다운로드할 때 연속된 URL을 하나의 명령어로 처리할 수 있다.  
  
```
$curl "https://example_url/[000001-000188].m4s" -o "#1.m4s"
```
https://example_url/000001.m4s~000188.m4s까지 연속적으로 접속해서 결과를 000001.m4s ~  
000184.m4s로 저장. curl로 네이버나 카카오 TV의 동영상을 받을 때 유용하다.  
(-o는 output의 줄임말)  

## Proxy 서버 지정
Proxy 서버는 -x 옵션으로 지정 가능하다.  
Win10에서는 https proxy는 미지원한다.  

```
curl -x http://proxy_server:proxy_port --proxy-user username:password -L http://url.example
```

## Http Header에서 Bearer token을 포함하여 POST 명령어
http://example.com/api로 Bearer Token을 Authorization 값으로 실어서 data.json 내용으로  
POST하는 명령어이다.  
OAuth 2.0 명령어 테스트 시 유용하다.  
```
curl -X POST\
-H Content-Type:application/json\
-H Authorization: Bearer abcdbdg\
-d @data.json\

http://example.com/api
```

### curl 옵션 설명
1. -X, --request -> <command> 사용할 요청 명령어 지정(GET, POST등)
2. -H, --header HTTP Header에 추가. 위 예제에서는 Content-Type:application/json과  
Authorization: Bearer abcdbdg을 추가함
3. -d, --data HTTP POST data
4. --data-ascii HTTP POST ASCII data
5. --data-binary HTTP POST binary data

...
[curl](https://kibua20.tistory.com/148)  
[curl 옵션](https://www.lesstif.com/software-architect/curl-http-get-post-rest-api-14745703.html)






































