이번에는 AWS 콘솔이 아닌 검색 도메인의 엔드포인트 주소로 검색을 해보자.   
이 때 HTTP 메서드를 손쉽게 사용할 수 있는 cURL을 사용한다. 보통 Linux 배포판이나   
Mac OS X에는 cURL이 기본적으로 설치되어 있다. 설치되어 있지 않다면 다음   
명령으로 설치할 수 있다.  
Amazon Linux, CentOS  
```
sudo yum install curl
```
Ubuntu Linux  
```
sudo apt-get install curl
```
Windows용은 http://curl.haxx.se/download.html에서 받을 수 있다.  
  
다음 명령을 입력하여 검색 도메인에서 검색을 한다(Windows에서는 curl.exe를   
실행한다).  

- -X GET: HTTP GET 메서드를 사용한다.  
- search-<검색 도메인 이름>-<검색 도메인 ID>-ap-northeast-2.cloudsearch.  
amazonaws.com/2013-01-01/search: 검색용 엔드포인트 주소이다. 여러분들이   
생성한 검색 도메인의 엔드포인트 주소를 입력한다. 맨 마지막에 `/2013-01-01/  
search`에서 2013-01-01은 엔드포인트 API의 버전이다. 다른 날짜로 바꾸면   
동작이 되지 않으니 주의하기 바란다.   
- ?q=010-1234-5678: 검색어이다. 여기서는 핸드폰 번호가 010-1234-5678인 것을  
검색했다. 검색 방법에 대한 자세한 내용은 다음 링크를 참조하기 바란다.  
http://docs.aws.amazon.com/cloudsearch/latest/developerguide/search-api.html   

```
curl -X GET search-exampledomain2-6o4kmjqgddx4cdhz7jbpsuclbe.ap-northeast-2.cloudsearch.amazonaws.com/2013-01-01/search?q=010-1234-5678
{"status":{"rid":"zJitt/kviAEK1Dk8","time-ms":9},"hits":{"found":1,
"start":0,"hit":[{"id":"1","fields":{"name":"홍길동","age":"27","rank":
"대리","phone":"010-1234-5678","address":"서울시 종로구"}}]}}
```

> #### 터미널, 명령 프롬프트에서 UTF-8 한글 입력  
> Linux의 터미널, Windows의 명령 프롬프트에서 UTF-8로 인코딩된 한글을 입력하는   
> 것은 상당히 까다롭다. 그래서 숫자로 된 핸드폰 번호로 검색했다.   
> 실제로 검색용 엔드포인트 주소를 사용할 때는 프로그래밍 언어를 통해서 사용하게   
> 된다. 한글을 검색할 때는 꼭 UTF-8로 인코딩해야 한다.  
> 특히 검색처럼 엔드포인트 URL에 GET 메서드를 이용할 경우 UTF-8에 URL 인코딩    
> 을 해주어야 한다. 

HTTP GET 메서드는 우리가 인터넷을 할 때 흔히 사용하는 HTTP 메서드이다. 따라서  
웹 브라우저에서 엔드포인트 주소로 검색을 할 수 있다. 이 때는 한글을 그대로   
사용할 수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/158765426-703a9df6-89f8-4c75-ab9b-b138683a6d74.png" width="50%" height="50%"/>    
```
http://search-exampledomain2-6o4kmjqgddx4cdhz7jbpsuclbe.ap-northeast-2.cloudsearch.amazonaws.com/2013-01-01/search?q=홍길동
```















