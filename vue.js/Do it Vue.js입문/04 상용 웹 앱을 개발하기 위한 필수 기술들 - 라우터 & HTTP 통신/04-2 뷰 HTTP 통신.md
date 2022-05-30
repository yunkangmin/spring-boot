# 웹 앱의 HTTP 통신 방법
요즘 웹 앱에서 서버에 데이터를 요청하는 HTTP(HyperText Transfer Protocol) 통신은 필수로  
구현해야 하는 기능이다. 과거의 웹 사이트가 정적인 텍스트나 간단한 이미지를 나타내는 데  
그쳤다면 이제는 사용자와의 상호 작용에 따라 데이터를 동적으로 화면에 표시해 줘야 하기  
때문이다. 
  
여기서 HTTP는 브라우저와 서버 간에 데이터를 주고 받는 통신 프로토콜(protocol)이다.  
프로토콜은 컴퓨터나 단말기 간에 통신하기 위해 상호간에 정의한 규칙이다.   
브라우저에서 특정 데이터를 보내달라고 요청(request)을 보내면 서버에서 응답(response)으로  
해당 데이터를 보내주는 방식으로 동작한다. 서버에 '해당 데이터를 보내주세요.'라는 메시지를  
보내는 게 바로 'HTTP 요청을 보낸다'와 같은 의미이다.    
<center><img src="https://user-images.githubusercontent.com/33191974/148934925-13aaf8f9-5424-4353-8538-21fd886beb63.png" width="600" height="300"/></center>
웹 앱 HTTP 통신의 대표적인 사례로는 제이쿼리(jQuery)의 ajax가 있다. ajax는 서버에서 받아온  
데이터를 표시할 때 화면 전체를 갱신하지 않고도 화면의 일부분만 변경할 수 있게 하는 자바스크립트  
기법이다. ajax가 대중화되면서 많은 웹 앱에서 ajax를 사용하고 있다. 리액트, 앵귤러 등에서도  
활발하게 사용하고 있다.   
  
뷰에서도 마찬가지로 ajax를 지원하기 위한 라이브러리를 제공한다. 뷰 프레임워크의 필수 라이브러리로  
관리하던 뷰 리소스와 요즘 가장 많이 사용하는 엑시오스(axios)가 바로 그것이다.  
그럼 각 라이브러리는 어떤 특징이 있는지 함께 살펴보자.   

# 뷰 리소스
뷰 리소스(resource)는 초기에 코어 팀에서 공식적으로 권하는 라이브러리였으나 2016년 말에  
에반이 공식적인 지원을 중단하기로 결정하면서 다시 기존에 관리했던 PageKit 팀의 라이브러리로  
돌아갔다. 그 이유는 HTTP 통신 관련 라이브러리는 뷰 라우팅, 상태 관리와 같은 라이브러리와는  
다르게 프레임워크에 필수적인 기능히 아니라고 판단했기 때문이다. 그럼에도 불구하고 뷰 리소스는  
아직 계속 사용할 수 있는 라이브러리이기 때문에 간단히 살펴보겠다.  
  
뷰 리소스를 사용하는 방법은 CDN을 이용해서 라이브러리를 로딩하는 방식과 NPM으로 라이브러리를  
설치하는 방법(ES6 기준)이 있다([ES6 설치방법 참고](https://github.com/pagekit/vue-resource#installation)). CDN 설치 방법을 이용하여 간단히 뷰 리소스로 서버에서 특정  
데이터를 받아와 로그로 출력하는 실습을 해보자.
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Vue Named View Sample</title>
    </head>
    <body>
       
        <div id="app">
            <!-- 
                1. 먼저 버튼은 인스턴스 영역 안인 <div> 태그 안에 <button> 태그로 
                추가한다. 그리고 v-on:click을 이용하여 버튼을 클릭했을 때 getData()가
                호출되도록 클릭 이벤트를 설정한다.
            -->
            <button v-on:click="getData">프레임워크 목록 가져오기</button>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <!-- vue resource cdn -->
        <script src="https://cdn.jsdelivr.net/npm/vue-resource@1.3.4"></script>
        <script>
            new Vue({
                el : '#app',
                methods : {
                    getData : function() {
                        //2. getData()에는 뷰 리소스에서 제공하는 API인 this.$http.get()을
                        //사용하여 해당 URL에서 제공하는 데이터를 받아온다.
                        //API 이름에서 유츄할 수 있듯이 this.$http.get()은 HTTP GET 요청을  
                        //서버에 보내고 특정 데이터를 받아온다.
                        this.$http.get('https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json')
                        .then(function(response) {
                            //3. 그리고 버튼을 클릭하여 해당 URL로 HTTP GET 요청을 보내고 나면,
                            //then() 안에서 응답을 받은 데이터 response를 콘솔에 출력한다.
                            //참고로, v-on:click은 다음 장 뷰 템플릿에서 자세히 소개한다. 
                            console.log(response);
                            console.log(JSON.parse(response.data));
                        });
                    }
                }
            });
        </script>
    </body>
</html>    
```
<center><img src="https://user-images.githubusercontent.com/33191974/148937909-05cbffb0-05dc-49ee-9b60-5f8a90ebd160.png" width="500" height="500"/></center> 
화면에서 [프레임워크 목록 가져오기] 버튼을 클릭하면 크롬 개발자 도구의 'Console' 패널에 response 객체의 값과  
프레임워크 목록을 객체에 담아 출력한다. 여기서 첫 번째 로그는 response의 내용이다. url 속성 값에는 HTTP GET  
요청을 할 때 넣었던 사이트의 URL이 들어있다. 두 번째 로그는 응답 데이터의 body 값이 문자열이기 때문에  
JSON.parse() 자바 스크립트 API를 이용하여 자바스크립트 객체로 보기 쉽게 변환하였다.  
  
이처럼 뷰 리소스 라이브러리를 이용하면 서버로부터 데이터를 받아와 화면에 나타낼 수 있다. 여기서는 단순히 받아온  
데이터를 로그로만 출력했지만 05장에서 살펴볼 데이터 바인딩과 템플릿을 익히고 나면 받아온 데이터를 화면에 쉽게  
나타낼 수 있을 것이다.  
  
# 액시오스
액시오스(Axios)는 현재 뷰 커뮤니티에서 가장 많이 사용되는 HTTP 통신 라이브러리이다. 에반도 뷰 리소스 라이브러리를  
공식 라이브러리에서 제외하면서 액시오스를 언급했다. 액시오스는 깃허브 리포지토리의 별이 3만개가 넘는데, 이는 뷰  
리소스의 8천 개에 비해 압도적으로 많다. 그만큼 많은 개발자들이 관심을 갖고 이용하고 있다는 증거이다. 일반적으로  
오픈 소스 라이브러리의 장래성은 깃허브 리포지토리가 얼마나 활성화되어 있느냐로 판단할 수 있는데, 액시오스가  
그런 면에서는 뷰 리소스보다 더 안정적으로 지원되는 라이브러리라고 할 수 있다.   
  
또한 액시오스는 Promise 기반의 API 형식이 다양하게 제공되어 별도의 로직을 구현할 필요 없이 주어진 API만으로도  
간편하게 원하는 로직을 구현할 수 있다.   

> #### Promise 기반의 API 형식이란 무엇일까?
> Promise란 서버에 데이터를 요청하여 받아오는 동작과 같은 비동기 로직 처리에 유용한 자바스크립트 객체이다.  
> 자바스크립트는 단일 스레드(thread)로 코드를 처리하기 때문에 특정 로직의 처리가 끝날 때까지 기다려주지 않는다.  
> 따라서 데이터를 요청하고 받아올 때까지 기다렸다가 화면에 나타내는 로직을 실행해야 할 때 주로 Promise를  
> 활용한다(?). 그리고 데이터를 받아왔을 때 Promise로 데이터를 화면에 표시하거나 연산을 수행하는 등 특정 로직을  
> 수행한다. 데이터 통신과 관련한 여러 라이브러리 대부분에서 Promise를 활요하고 있으며, 액시오스에서도 Promise  
> 기반의 API를 지원한다.  

액시오스 공식 깃허브 리포지토리(https://github.com/axios/axios)에서 안내하는 문서 역시 뷰 리소스보다 더 상세하게  
기술되어 있다. 따라서 원하는 기능에 대해 손쉽게 API 형식과 코드 예제를 참고할 수 있다.  

## 액시오스 설치 및 사용하기
액시오스 설치와 사용법은 뷰 리소스와 거의 동일하다. CDN을 이용하여 설치하는 방법과 NPM을 이용하여 설치하는 방법
(ES6 기준)이 있다. 여기서는 CDN을 이용하는 방법을 살펴본다. NPM을 이용한 설치 방법은 다음 링크를 참고한다(
https://github.com/axios/axios#installing).

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

위 코드를 HTML 파일에 추가하면 라이브러리를 로딩하여 사용할 수 있는 상태가 된다.   
  
액시오스는 뷰 리소스처럼 HTTP 통신에 대해 간단하고 직관적인 API를 제공한다. 그리고 API 형식이 다양하여 단순한  
호출 이외에도 여러 설정값을 추가하여 함께 호출할 수 있다.   

```
//HTTP GET 요청
axios.get('URL 주소').then().catch();
```
```
//HTTP POST 요청
axios.post('URL 주소').then().catch();
```
```
//HTTP 요청에 대한 옵션 속성 정의
axios({
  method : 'get',
  url : 'URL 주소',
  ...
});
```

위 API 형식을 간단히 살펴보자
(API 유형 | 처리결과)  
- axios.get('URL 주소').then().catch() : 해당 URL 주소에 대해 HTTP GET 요청을 보낸다. 서버에서 보낸 데이터를   
정상적으로 받아오면 then() 안에 정의한 로직이 실행되고, 데이터를 받아올 때 오류가 발생하면 catch()에 정의한 로직이   
수행된다.  
- axios.post('URL 주소').then().catch() : 해당 URL 주소에 대해 HTTP POST 요청을 보낸다. then()과 catch()의 동작은  
위에서 살펴본 내용과 동일하다.  
- axios({ 옵션 속성 }) : HTTP 요청에 대한 자세한 속성들을 직접 정의하여 보낼 수 있다. 데이터 요청을 보낼 URL, HTTP  
요청 방식, 보내는 데이터 유형, 기타 등등.  

앞에서 설명한 사용 방법 중 GET 요청을 하는 API로 간단히 데이터를 요청해서 받아와 콘솔에 출력하는 실습을 해보자.  
코드 처리 흐름은 앞의 뷰 리소스 코드 예제와 동일하다.   

```
<html>
    <head>
        <title>Vue with Axios Sample</title>
    </head>
    <body>
        <div id="app">
            <button v-on:click="getData">프레임워크 목록 가져오기</button>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <!-- 액시오스 라이브러리 로딩 -->
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el : '#app',
                methods : {
                    getData : function() {
                        //액시오스 GET 요청 API
                        axios.get('https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json')
                        .then(function(response) {
                            console.log(response);
                        });
                    }
                }
            });
        </script>
    </body>
</html>
```
위 코드는 뷰 리소스 실습 예제와 마찬가지로 [프레임워크 목록 가져오기] 버튼을 클릭해서 HTTP GET 요청을 보내고 데이터를 받아와서  
로그에 출력하는 예제이다. 이전 예제 코드와 비교하면 라이브러리를 로딩해 오는 CDN의 주소와 GET 요청을 보내는 API 형식 부분만  
다르다.  
![image](https://user-images.githubusercontent.com/33191974/149094477-04e5799b-2202-493f-b8c1-9fe834bcb183.png)  
response 객체를 확인해 보면 data 속성이 일반 문자열 형식이 아니라 객체 형태이기 때문에 별도로 JSON.parse()를 사용하여   
객체로 변환할 필요가 없다. 이런 부분들이 뷰 액시오스가 뷰 리소스보다 사용성이 좋다는 것을 증명해준다.  
   
 지금까지 뷰의 HTTP 통신 라이브러리인 뷰 리소스와 액시오스에 대해 살펴봤다. 라이브러리에서 제공하는 API를 이용하여 간편하게   
 HTTP 통신을 구현할 수 있고, 원하는 데이터를 서버에서 끌어다가 화면으로 가져올 수 있다는 것을 확인했다. 라이브러리에 대한   
 더 자세한 사용 방법과 가이드는 앞에서 소개한 각 링크 주소를 참고하자.  







  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  

