앞에서도 살펴봤지만 일단 뷰를 사용할 때 제이쿼리 사용은 지양해야 한다. 왜냐하면 뷰 프레임워크 자체만으로도  
이미 제이쿼리가 제공하는 기능을 다 구현할 수 있기 때문이다. 그럼에도 불구하고 실무에서는 기존에 구현해 놓은   
제이쿼리 UI(jQuery UI)같은 제이쿼리 기반의 라이브러리를 어쩔수 없이 사용해야 하는 상황이 있을 수 있다.   
만약 소프트웨어 개발을 의뢰한 고객의 요구 사항이 제이쿼리 UI 라이브러리라면 어떻게 해야 할까? 개발 기간이  
충분하지 않은 상태에서 뷰로 제이쿼리 UI와 유사한 라이브러리를 만드는게 더 오래 걸린다. 따라서 기존 라이브러리를   
안전하게 뷰에서 사용할 수 있도록 통합할 줄 알아야 한다.  
  
이번 팁에서는 제이쿼리 UI 라이브러리 중 날짜 선택(Date Picker)를 뷰 프로젝트에 통합해보자. 날짜 선택기 라이브러리는  
꽤 많은 곳에서 사용되고 있는 제이쿼리 플러그인이다. 모양은 아래와 같다. 참고로 날짜 선택기뿐만 아니라 다른 형태의  
자바스크립트 라이브러리도 지금부터 살펴보는 방식과 비슷하게 통합할 수 있다.    
![image](https://user-images.githubusercontent.com/33191974/149937792-9abd081b-a413-4b69-9658-dc8204d2d161.png)  
그럼 뷰 CLI로 webpack-simple 프로젝트를 구성하여 위 라이브러리를 추가해보겠다.

# 제이쿼리와 제이쿼리 UI 라이브러리 연동
뷰 CLI로 프로젝트 구성을 마쳤다면 이제 제이쿼리와 제이쿼리 UI 라이브러리를 연동해보자.   
webpack-simple 프로젝트에 제이쿼리 라이브러리를 연동하는 방법은 크게 2가지이다.   
- CDN을 이용하는 방법 : index.html 파일에 `<script>` 추가 
- 웹팩 추가 플러그인을 사용하는 방법 : 웹팩 설정 파일(webpack.config.js)에 ProviderPlugin 설정추가  

두 번째 방법이 첫 번째 방법보다 결합도는 더 높지만 웹팩의 동작 원리를 알고 있어야 사용할 수 있다.  
그럼 일단 간편하게 적용할 수 있는 첫 번째 방법으로 해보자. 여기서 결합도가 높다는 말은 웹팩으로 프로젝트  
구조를 만들 때 더 잘 통합된다는 의미이다.   
   
제이쿼리와 제이쿼리 UI의 CDN 주소는 검색 엔진에 각각 jquery cdn과 jquery ui cdn을 입력하여 검색한다.  
여기서는 빠른 진행을 위해 CDN을 가져올 수 있는 사이트 링크와 각 라이브러리 CDN 주소를 아래에 정리했다.   
(사이트 역할 | URL)   
- 제이쿼리의 CDN 목록 : https://jquery.com/download/#other-cdns
- 제이쿼리 UI의 CDN 목록 : https://code.jquery.com/ui/
- 제이쿼리 CDN 주소 : https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js
- 제이쿼리 UI의 CDN 주소 : https://code.jquery.com/ui/1.12.1/jquery-ui.js
- 제이쿼리 UI CSS의 CDN 주소 : https://code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css

1. 프로젝트의 index.html 파일에 위 제이쿼리 CDN 주소와 제이쿼리 UI CDN 주소로 `<script>` 태그를 추가한다.   
새로 추가하는 `<script>` 태그의 위치는 기존 `<script src="/dist/build.js">`의 바로 위이다.   
CDN으로 제이쿼리와 제이쿼리 UI 라이브러리를 추가한 코드  
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>vue-jquery-ui-datepicker</title>
    <!-- 제이쿼리 UI CSS의 CDN 주소 -->
    <link rel="stylesheet" href="https://code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
  </head>
  <body>
    <div id="app"></div>
    <!-- 제이쿼리 CDN 주소 -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <!-- 제이쿼리 UI의 CDN 주소 -->
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>
    <script src="/dist/build.js"></script>
  </body>
</html>
```
그리고 제이쿼리 UI 라이브러리의 스타일링을 위해 CSS CDN 주소를 사용한다. `<head>` 태그 내부의 맨 아래에  
`<link>` 태그를 추가하고 href 속성에 CDN 주소를 추가한다.   

2. 이제 CDN을 이용하여 로딩한 제이쿼리와 제이쿼리 UI 라이브러리가 정상적으로 동작하는지 확인해보자.   
src 폴더 밑에 App.vue 파일의 기존 내용을 다 지우고 아래와 같이 작성한다.   
라이브러리 로딩을 확인하기 위한 App.vue 코드  
```
<template>
  <div id="app">
    App 컴포넌트
  </div>
</template>

<script>
export default {
  created() {
    console.log($.fn.jquery);
    console.log($.ui.version);
  }
}
</script>
```
위 코드는 기존에 초기 화면을 구성하던 코드를 모두 삭제하고, 제이쿼리와 제이쿼리 UI 라이브러리가  
정상적으로 로딩되는지 확인하는 코드이다. App 컴포넌트의 인스턴스가 생성되고 나면 바로 created()에  
정의된 로직을 실행한다. 첫 번째 로그은 제이쿼리의 최신 버전인 3.2.1를 출력하고, 두 번째 로그는   
제이쿼리 UI 라이브러리의 최신 버전인 1.12.1을 출력한다.   
    
3. 변경된 파일 내용을 저장하고 다시 화면을 실행하면 개발자 도구의 콘솔에 다음과 같은 결과가 표시된다.   
![image](https://user-images.githubusercontent.com/33191974/149942272-f2d53b13-1e31-49c0-8f9b-5ca1bd06c18b.png)

# 제이쿼리 UI 라이브러리의 날짜 선택기 위젯 구현
프로젝트를 구성하고 필요한 라이브러리까지 CDN으로 연동했으니 이제 제이쿼리 UI 라이브러리로 날짜 선택기  
위젯을 구현해보자. 기존 제이쿼리 기반의 웹 앱에서 날짜 선택기를 구현하려면 인풋 박스를 하나 생성한 후   
클릭했을 때 .datepicker()를 호출해보자.  

4. App.vue 파일에 인풋 박스를 1개 추가하고, 클릭했을 때 .datepicker()를 호출하는 로직을 mounted() 안에  
추가한다. 기존 라이브러리 버전을 확인한느 created() 코드는 삭제한다.   
인풋 박스를 클릭했을 때 날짜 선택기가 표시되는 App.vue 코드  
```
<template>
  <div id="app">
    App 컴포넌트 <br/>
    <input id="calendar">
  </div>
</template>

<script>
export default {
  mounted() {
    $('#calendar').datepicker();
  }
}
</script>
```
앞 코드는 `<input>` 태그에 id 값으로 calendar를 주고, 제이쿼리 선택자 $()를 이용하여 datepicker()를 호출하는  
코드이다. 여기서 중요한 점은 인스턴스 라이프 사이클 훅 중 mounted에 제이쿼리 로직을 추가했다는 것이다.  
앞에서 라이브러리 버전을 확인할 때는 created()를 사용했는데, 제이쿼리 로직을 수행할 때는 mounted()를 사용한  
이유는 무엇일까?  
  
`<template>` 안의 HTML 태그에 접근할 수 있는 시점이 뷰의 가상 돔이 화면의 특정 요소에 부착되고 난 시점인   
mounted()이기 때문이다. [Tip 1]에서 다루었듯이 mounted의 이전 라이프 사이클 단계인 created에서는  
`<template>`에 정의한 돔 요소에 접근할 수 없다.   
  
5. 4번 코드를 실행하고 인풋 박스를 클릭하면 아래와 같이 날짜 선택기가 표시된다.  
![image](https://user-images.githubusercontent.com/33191974/149943826-8109e917-1e08-40ef-901a-d16b56021222.png)

# 날짜 선택기 위젯 컴포넌트화  
지금까지 제이쿼리 UI 라이브러리를 이용하여 날짜 선택기 위젯을 구현했다. 이렇게 뷰 로직에 제이쿼리 로직을 계속  
섞어서 개발하다 보면 나중에 코드를 추적하거나 관리하기가 어려워진다. 따라서 날짜 선택기 위젯 부분만 컴포넌트로  
분리하여 관리할 수 있게 코드를 구조화해보자.  

6. src 폴더 밑에 DatePicker.vue 파일을 하나 생성하고, 4번에서 제작한 날짜 선택기 코드 부분을 가져와 다음과 같이   
그대로 추가한다. 
```
<template>
  <input id="calendar">
</template>

<script>
export default {
  mounted() {
    $('#calendar').datepicker();
  }
}
</script>
```
7. DatePicker.vue 파일을 App.vue 파일의 지역 컴포넌트로 아래와 같이 등록한다.   
```
<template>
  <div id="app">
    App 컴포넌트 <br/>
    <!-- 날짜 선택기 표시 -->
    <DatePicker></DatePicker>
  </div>
</template>

<script>
import DatePicker from './DatePicker.vue'

export default {
  // 날짜 선택기 컴포넌트 등록
  components : {
    DatePicker
  }
}
</script>
```
여기서 components 속성 부분을 보면 기존 컴포넌트 정의 방식인 'DatePicker': DatePicker를 사용하지 않고   
DatePicker만 사용했다. 이는 ES6의 객체 리터럴에서 제공하는 약식 문법을 사용한 것이다. 이렇게 하면  
DatePicker가 'DatePicker': DatePicker와 동일한 동작을 한다. 그리고 `<DatePicker>` 컴포넌트 태그를  
추가한다.   

8. DatePicker 컴포넌트를 등록한 후 코드를 실행하면 다음과같이 동일한 결과가 나온다.  
![image](https://user-images.githubusercontent.com/33191974/149945189-56c08ecb-59ef-4066-aad0-d33f26194b58.png)  

이렇게 날짜 선택기 위젯을 컴포넌트로 분리하였기 때문에 App.vue에서 구현한 방식처럼 필요한 화면에서   
DatePicker 컴포넌트를 등록하여 사용할 수 있다.  
































