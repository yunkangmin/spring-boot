
# HTML JavaScript 로드 http:// vs //

## 결론
//의미는 현재 요청의 프로토콜을 따른다는 것이다.   
```javascript
<script src="//code.jquery.com/jquery-3.2.1.min.js"></script>
```

# 자세히 알아보기
웹 개발을 하면 외부 JavaScript 라이브러리를 많이 사용한다(라이브러리/  
프레임워크없이 순수 JavaScript만 사용하는 Vanila JS도 있다).   
  
아직 많은 개발 환경에서 라이브러리를 직접 로드하여 사용한다.   
이렇게 라이브러리를 직접 로드해 사용하려면 두 가지 방법이 있다.   
1. 서버에 라이브러리 소스 파일을 업로드 하여 사용
2. 라이브러리에서 제공하는 파일을 http 혹은 https를 이용하여 사용   
```javascript
<script src="js/jquery-3.2.1.min.js"></script>
<script src="http://code.jquery.com/jquery-3.2.1.min.js"></script>
```
1. `js/jquery-3.2.1.min.js` : 웹 서버에 js라는 디렉토리가 존재하고 그   
안에 `jquery-3.2.1.min.js` 파일이 있어야 한다.  
2. `http://code.jquery.com/jquery-3.2.1.min.js` : http://code.jquery.com  
에서 제공하는 파일을 웹 브라우저가 다운받아 사용한다(당연히 인터넷이 연결  
되어야 사용 가능하다)  
   - 이러한 방법을 CDN을 이용한다고 한다.  

# http:// vs //
오픈 소스 라이브러리의 Usage 혹은 Example을 보면 보통 `https:// ` 혹은   
http://로 시작하는 링크가 예제로 나온다.   
하지만, 드물게 `//`로 시작하는 링크를 예제로 사용한 경우도 있다.  
```javascript
<script src="https://d3js.org/d3.v5.min.js"></script>
<script src="//ajax.googleapis.com/ajax/libs/dojo/1.14.1/dojo/dojo.js"></script>
```
# http://
index.html  
```html
<html>
  <head></head>
  <body></body>
</html>
<script src="http://code.jquery.com/jquery-3.2.1.min.js"></script>
```

# //
index.html   
```html
<html>
  <head></head>
  <body></body>
</html>
<script src="//code.jquery.com/jquery-3.2.1.min.js"></script>
```

# 차이점  
`//code.jquery.com/jquery-3.2.1.min.js`의 의미를 알면 차이점은 확실히 알  
수 있다.  
과거의 필자도 `//`로 시작하는 정확한 뜻은 모르고 예제를 그대로 복사해서  
사용하려고 했었다.   
하지만, 이 방법은 제대로 동작하지 않는 경우도 있어 `http:` 혹은 `https:`  
추가해서 사용했다.  

`//`의미는 현재 요청 프로토콜을 따른다는 것이다.   
만약, 웹 페이지의 URL이 http://example.com이면 `//code.jquery.com/jquery-  
3.2.1.min.js`로 로드한 JavaScript 라이브러리는 `http://code.jquery.com/  
jquery-3.2.1.min.js`로 변환되어 브라우저가 해당 주소로 요청한다.  
- 자세히 알아보면, 웹 서비스 제공자가 http, https 환경 모두에서 example  
.com이라는 서비스를 운영하고 있을 때, 사용자가 브라우저(클라이언트)에  
`httP://example.com`이라고 입력하면 `//`로 로드한 라이브러리들은 자동으로  
앞에 `http:`를 붙인다(http로 요청 시 https로 전환하지 않는다고 가정).  
  
같은 원리로 웹 페이지의 URL이 https://example.com로 https 환경에서 운영  
되면 `//code.jquery.com/jquery-3.2.1.min.js`는 `https://code.jquery.com/  
jquery-3.2.1.min.js`로 변환된다.   

그러면 `//`로 JavaScript 라이브러리를 로드했을 때 제대로 동작하지 않은  
이유는 뭘까?   
  
필자의 경우, HTML 파일을 개인 PC의 브라우저에서 열었기 때문이다.   
웹 서버의 프로토콜을 따라야 하는데 PC의 브라우저에서 HTML을 열었기 때문에  
http 혹은 https를 이용하지 않고 브라우저가 프로토콜을 이용했기 때문에   
제대로 동작하지 않았다.   
  
---

#### Unix(Linux, MacOS) 기준예시  
개인 PC에 Documents라는 디렉토리가 있고 그 안에 index.html 파일이 있다고   
가정했을 때, index.html을 더블 클릭하면 PC의 기본 브라우저에서 index.html  
이 열린다. 브라우저 주소창을 보면 Users/ryan/Documents/index.html이라고  
나올 것이다(브라우저에 따라 자동으로 프로토콜을 숨긴다).  
여기서 주소창의 URL을 복사하여 메모장에 붙여 넣으면 file:///Users/ryan/  
Documents/index.html이라고 나온다.  
브라우저가 파일 프로토콜을 이용해 /Users/ryan/Documents/index.html 파일을  
오픈한 것이다.  
http 혹은 https와 같은 웹 서버 환경이 아니다.  
- 이 경우 위에서 설명한 원리와 동일하게 `//`로 로드한 라이브러리는 현재  
요청 프로톨콜인 file://을 따른다.   
- 따라서 file:///code.jquery.com/jquery-3.2.1.min.js로 변환되고 현재 PC  
에 `/code.jquery.com/jquery-3.2.1.min.js`에 해당하는 디렉토리와 파일이   
존재하지 않기 때문에 오류가 발생하는 것이다.   

---

개인 PC 환경에서 `//`사용 시 웹 브라우저는 기본으로 파일 프로토콜을 이용  
한다. `file://` 그리고 해당 파일이 존재하지 않았기 때문에 다음과 같은  
오류가 발생했다.  
<img src="https://user-images.githubusercontent.com/33191974/162555395-81f3588e-5cb7-45ca-b17f-7d5c5ea39f41.png" width="50%" height="50%"/>    
문제 해결을 위해 아래와 같이 HTTP 서버 환경으로 구동한다면, `file://`이  
아닌 `http://`로 JavaScript 라이브러리를 로드한다(http://127.0.0.1:8000).  
```
$ ls
index.html

$ python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
즉, `//`로 시작하는 주소를 이용해 JavaScript 라이브러리를 로드하면 파일  
환경 혹은 웹 서버 환경`에 따라서 자동으로 붙여주는 프로토콜이 다르기 때문에  
제대로 동작하지 않는 경우도 있고 제대로 동작하는 경우도 있던 것이다.   
  
# 뭐가 더 좋을까?    
케바케지만, 필자의 상황에선 두 방법 모두 사용하지 않고 라이브러리 파일을  
서버에 업로드하여 사용하고 있다.   
> 필자의 상황은 망분리로 인해 내부망에선 인터넷이 안 되는 제약이 있다.   
> 내부망과 외부망 모두에서 정상 동작을 위해 웹 서버에 JavaScript 파일을  
> 업로드하여 사용한다.   
> 만약 필자의 상황이 네트워크 효율과 경제성을 고려해야 하는 등의 다른 상황  
> 이라면 CDN 방법을 택했을 수도 있다.  

---

그래도 둘 중 하나를 선택하라면, 프로토콜을 명시해서 쓸 것이다(`http://`).
   
개발 환경은 매우 다양하다.  
설명 중 언급한 것과 같이 로컬 PC 브라우저에서 HTML 파일을 열어 개발하는  
경우가 있다.    
또한, 개발 서버의 웹은 http인데 운영서버 웹은 https일 수 있다.   

> `http:// vs //` 둘만 비교하자면, `//`는 위에서 든 몇 가지 예시와 같이   
> 제대로 동작하지 않는 경우가 있을 수 있다.  
> 하지만, `http://`는 인터넷이 연결된 환경이라면 동작하지 않는 경우가 없다.   

또한, 예전엔 라이브러리 예제들이 `//` 방식을 사용했는데 요즘은 대부분은  
https를 우선으로 제공해서인지 `https://`로 시작하는 URL을 사용하고 있다.   

**방법은 안 되는 경우가 있지만, `http://` 방법은 안되는 경우가 없다.  





































