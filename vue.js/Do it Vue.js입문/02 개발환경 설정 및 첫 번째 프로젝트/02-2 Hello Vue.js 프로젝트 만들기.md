# 뷰 시작하기
뷰를 사용하여 간단한 메시지를 출력해 보자.  
뷰를 사용하는 방법이 얼마나 쉽고 간편한지 직접 확인할 수 있다.  

```
HTML 파일 생성 -> 뷰 소스코드 추가 -> 브라우저로 실행
```
  
VSCode를 실행하고 `C:\vue02` 경로에 index.html 파일을 생성하고 아래와 같이 작성하자.    
```
<html>
    <head>
        <title>Vue Sample</title>
    </head>
    <body>
        <div id="app">
            {{ message }}
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    message: 'Hello Vue.js!'
                }
            });
        </script>
    </body>
</html>
```
  
위 코드는 html 기본 구조에 <div> 태그를 하나 추가하고, 뷰 라이브러리를 로딩한 후 뷰로 Hello Vue.js!라는  
간단한 메세지를 출력하는 코드이다. 뷰 인스턴스를 만들고 인스턴스에 정의된 데이터 객체의 메시지 프로퍼티  
(property)를 화면에 출력한다.  
   
여기서 중요한 건 이렇게 HTML 문서에서 코드 몇 줄로 바로 실행할 수 있다는 사실이다.   
이 때문에 웹 개발자 커뮤니티에서는 종종 '뷰가 제이쿼리보다 적용하기 쉽다.'는 말이 나온다.   
   
위의 코드를 브라우저에서 열어보자.  
![image](https://user-images.githubusercontent.com/33191974/148215216-4acbeebc-47c6-475b-b9c7-9a9bff3cb91d.png)  

# 크롬 개발자 도구로 코드 확인하기
앞에서 실행한 예제가 뷰 라이브러리를 정상적으로 로딩하였는지 확인하기 위해 크롬 개발자 도구의 Console 패널을  
확인해보자.  
![image](https://user-images.githubusercontent.com/33191974/148215734-8f2eeebf-29f3-42ef-a1b9-94f9f2258892.png)  
이 로그를 통해 뷰 라이브러리가 정상적으로 로딩이 되었고, 현재 개발자 모드로 뷰가 실행되고 있다는 것도 파악하였다.  
  
# 뷰 개발자 도구로 코드 확인하기
이번에는 뷰 어플리케이션에만 활용할 수 있는 **뷰 개발자 도구**로 작성한 예제를 분석해보자.  
뷰 개발자 도구는 컴포넌트로 구성된 애플리케이션의 구조를 한눈에 확인할 수 있다.  
그리고 각 컴포넌트별로 정의된 속성의 변화를 실시간으로 확인할 수 있어 뷰로 제작한 웹 앱을 분석하거나   
디버깅할 때 유용하게 사용할 수 있다.  
  
## 첫 번째 로그 해결 방법
앞에서 나온 로그 중 첫 번째 로그은 아이러니하게도 뷰 개발자 도구를 설치한 사용자에게도 표시된다.   
왜냐하면 현재 예제를 서버에서 띄운 것이 아니라 파일 시스템에서 접근하여 브라우저로 실행했기 때문이다.  
쉽게 말하면 브라우저 주소 창에 file://형태로 접근한 파일과 http://로 접근한 파일에 대해서 뷰 개발자  
도구가 각기 다른 설정을 적용하기 때문이다.  
이 문제를 해결하기 위해서는 크롬 확장 플러그인 설정을 변경해야 한다.  
![image](https://user-images.githubusercontent.com/33191974/148216783-b1705af3-84cf-4408-90f3-bde4b8ffc484.png)  
![image](https://user-images.githubusercontent.com/33191974/148216933-4aa66ace-c68f-484d-9e6d-f4833b360ecc.png)  
![image](https://user-images.githubusercontent.com/33191974/148217043-049925b1-31b5-4d9d-b3eb-0bde431c1e8d.png)
그리고 다시 페이지를 실행하면 아래와 같이 첫 번째 로그가 사라진 것을 확인할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/148217143-8a74f823-b6cd-47d6-bd4a-2edc089dbb98.png)

# 뷰 개발자 도구 사용 방법
이제 뷰 개발자 도구를 활성화하였으니 한번 사용해 보겠다. 크롬 개발자 도구를 열고 'Vue'탭을 선택한다.    
![image](https://user-images.githubusercontent.com/33191974/148516669-3cafb12e-19f9-4dd2-91c6-42cd2301a803.png)  
'Vue' 탭을 열고 페이지 가운데에 보이는 '<Root> ==$vm()'을 클릭하면 다음과 같은 화면이 나타난다.  
![image](https://user-images.githubusercontent.com/33191974/148517216-ec97efa7-c1b2-49c8-bd96-527a9712dace.png)  
'<Root> ==$vm()'을 클릭하면 왼쪽의 'Hello Vue.js!' 텍스트가 강조되면서 오른쪽에 루트 컴포넌트에  
대한 상세 내용이 표시된다. 그 이외에 'Vuex', 'Events', Refresh' 탭을 선택하여 해당 기능들에 대한  
상태를 쉽게 확인할 수 있다. 여기서 루트 컴포넌트란 뷰 애플리케이션을 실행할 때 가장 근간이 되는  
컴포넌트이자 최상위 컴포넌트를 의미한다. 여기서는 최상위 컴포넌트라고 부른다.  
    
결론적으로 화면상으로 표시된 'Hello Vue.js!' 텍스트는 최상위 컴포넌트의 data 속성인 message의  
값이다. 어떻게 message의 텍스트 값이 화면에 표시된 건지는 다음글에서 살펴보자.  
  
  
  


  

  

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
