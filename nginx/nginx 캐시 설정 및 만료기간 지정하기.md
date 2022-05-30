## Nginx 캐시 설정 및 캐시 만료일 기간 설정
서버의 과다 트래픽 발생 방지를 위해 이미지나 웹 콘텐츠에 대해서 서버단에서의 캐시  
컨트롤을 활성화하는 것이 좋다.  
  
Nginx는 apache와는 다르게 .htaccess 파일보다는 주로 nginx.conf 파일과 같은 서버  
config 파일에서 액세스 설정을 하게 된다.  
(서버에 따라 nginx.conf라는 파일이 다른 이름으로 사용될 수 있음)
  
물론 브라우저의 캐시와 관련된 설정 또한 nginx.conf 파일에서 바로 지정할 수 있다.  
이는 .htaccess에서의 캐시 설정법과 비슷하면서도 다르다.  
  
여기서는 일반적인 정정 파일 캐시 설정만 작성하였다. 
  
아래 nginx.conf 예시를 살펴보자. 
```
#Enable Cache Control
location ~*\.(?:jpg|jpeg|png|gif|ico|gz|svg|ogg|mp4|webm|ogv|htc|cur)$ {
  expires 3M;
  access_log off;
  add_header Cache-Control "public";
}

location ~* \.(?:css|js)$ {
  expires 1M;
  access_log off;
  add_header Cache-Control "public";
}

location = /favicon.ico {
  expires max;
  access_log off;
  log_not_found off;
}
```
먼저 위 구문은 location 블록을 사용하므로, server 블록 내에 들어가야 한다.  
원하는 URL 또는 확장자 경로를 지정하여 어떤 확장자의 파일에 대해 캐시를 지정할 것인지를  
나타낼 수 있다. 대개 그림이나 음악, 영상 파일 또는 css, js 파일은 컨텐츠가 자주 변하는 일이  
없으므로 1 ~ 4개월 이상의 캐시 만료일을 지정한다.  
여기에서 html, php 확장자를 가진 웹 페이지를 구성하는 파일들은 페이지가 지속적으로 변하기 때문에   
따로 작성하지 않는 것을 권장한다.  

expires를 사용하여 캐시가 만료되는 일자를 설정할 수 있다. 
예를 들어 2개월 동안 캐시를 유지하고자 할 경우 '2M'을, 1년동안 유지하고자 할 때는  
'1y'와 같이 설정한다. (d=일/M=월/y=년)
  
expires는 일/월/년 단위이지만 이 대신에 add_header Cache-Control "public", max-age=[초];를  
사용하면 초 단위로도 표현할 수 있다.   
예를 들어 캐시 만료일을 30일로 설정하고 싶다면 아래와 같이 2592000(60*60*24*30 초)로 설정해주면 된다. 
```
add_header Cache-Control "public", max-age=2592000
```
access_log는 해당 파일을 읽어들일 때 로그를 남기는가에 대한 여부이다.  
위 컨텐츠에 대해서는 대부분 로그를 남길 필요성이 크지 않으므로 off로 설정했다.  
마찬가지로 log_not_found는 해당 파일이 없을 경우에 로그를 남길지에 대한 여부이다.  
off인 경우 로그를 남기지 않는다.  
  
이렇게 기본적으로 브라우저 캐시 만료에 대한 설정을 할 수 있다. 
  
해당 내용의 편집이 완료되었다면 nginx 서비스를 재기동해주어 적용을 완료한다.  
```
--서버 종류에 따라 아래 명령어 중 하나로 nginx 서비스를 재기동할 수 있다.
$sudo systemctl restart nginx(재시작0
$sudo systemctl reload nginx(리로드)

또는
$sudo service nginx reload(재시작)
$sudo service nginx restart(리로드)
```
































  
