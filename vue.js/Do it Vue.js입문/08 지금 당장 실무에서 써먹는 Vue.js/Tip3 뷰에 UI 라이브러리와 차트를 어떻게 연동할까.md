앞에서 살펴본 제이쿼리 외에도 실무에서 뷰를 사용하기 위해 필요한 지식은 바로 UI 라이브러리나  
차트 라이브러리를 연동하는 방법이다.    
  
부트스트랩(Bootstrap)같은 UI 라이브러리는 뷰가 나오기 전부터 사용되던 라이브러리이기 때문에  
일반적으로 제이쿼리 기반의 CDN 방식으로 제공된다. 하지만 요즘에는 앵귤러, 리액트, 뷰와 같은  
프런트엔드 프레임워크의 사용이 대중화되면서 특정 프레임워크와 더 잘 결합할 수 있는 라이브러리  
형태로 제공된다. 예를 들면 '앵귤러-부트스트랩' 또는 '리액트-부트스트랩' 라이브러리같은 라이브러리가  
있다.   
  
이와 같은 형태의 라이브러리가 제공되는 이유는 대부분의 프런트엔드 프레임워크에서 실제 돔을 조작하지 않고   
프레임워크에서 제공하는 기능으로 돔을 간접적으로 조작하기 때문이다.  
  
예를 들면, 뷰는 특정 돔 요소에 접근하기 위해 ref라는 속성을 사용한다. 그러나 일반 부트스트랩 라이브러리에서  
제공하는 위젯 기능들은 직접 돔을 조작하는 데에 특화되어 있다. 따라서 프런트엔드 프레임워크에서 일반 UI 라이브러리를  
사용하려면 위젯을 동작시키기 위해 특정 코드를 직접 추가하여 구현해야 한다. 하지만 이러한 추가 구현은 프런트엔드  
프레임워크 및 UI 라이브러리의 동작 원리를 정확히 모르는 입문자들에게는 높은 장벽이다. 그래서 각 개발 프레임워크  
커뮤니티에서 잘 결합되어 있는 형태의 UI 라이브러리를 제공하고 있다.   
  
# 뷰-부트스트랩
뷰 역시 마찬가지로 뷰-부트스트랩 라이브러리가 있다.   
![image](https://user-images.githubusercontent.com/33191974/150053976-3e68cdce-d544-4e0c-81c2-e1d7a392e40a.png)  
홈페이지 메인화면만 봤을 때는 기존 부트스트랩 라이브러리와 큰 차이가 없다. 그러나 [Get Started] 버튼을 클릭하여  
Getting Started 페이지로 가면 뷰에 특화된 설치 방법이 나온다.   
![image](https://user-images.githubusercontent.com/33191974/150055691-c20a166c-fa3b-4ea8-86e0-b5d95f004279.png)

  
뷰 CLI로 프로젝트를 생성하는 게 익숙한 사용자에게는 npm 명령어를 이용한 라이브러리 설치가 유용하게 느껴질 것이다.    
그리고 npm 명령어로 설치한 라이브러리를 main.js 파일에서 import로 불러와 Vue.use()에 담은 것 역시 뷰 개발자에게는  
익숙한 패턴이다. 이렇게 뷰 개발자가 좀 더 편하게 부트스트랩을 사용할 수 있도록 라이브러리가 제공되고 있다.   
  
하지만 여기서 주의할 점이 있다. 뷰의 경우 나온 지 얼마 되지 않았기 때문에 인기가 많은 몇 개의 라이브러리를 제외하고는  
이런 형태의 라이브러리를 찾기 어렵다는 점이다. 그런 경우 이런 라이브러리가 나오길 기다리는 것보다 기존의 일반 라이브러리를  
뷰 프레임워크에 잘 결합할 줄 아는 것이 훨씬 도움이 된다. 그럼 일반 부트스트랩 라이브러리와 차트 라이브러리를 연동하는  
방법에 대해서 살펴보자.   

# 뷰와 일반 부트스트랩 라이브러리 연동하기 
먼저 외부 라이브러리를 연동하기 위한 뷰 프로젝트를 생성한다. 그리고 부트스트랩 라이브러리는 CDN으로 로딩한다.   
부트스트랩 라이브러리 CDN 주소는 https://getbootstrap.com/docs/4.0/getting-started/introduction/에서 복사해 사용하거나   
다음 표를 참고하자.   
(CDN 용도 | URL 이름)  
- 부트스트랩 CSS : https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css
- 부트스트랩 자바스크립트 : https://code.jquery.com/jquery-3.2.1.slim.min.js  
https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.3/umd/popper.min.js   
https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/js/bootstrap.min.js

1. index.html에 부트스트랩 CSS CDN, 자바스크립트 CDN을 각각 아래와 같이 추가한다.     
`<link>`와 `<script>` 태그의 위치와 순서에 주의하자.   
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>vue-ui-chart</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css">
  </head>
  <body>
    <div id="app"></div>
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.3/umd/popper.min.js"></script>
    <script src="https:/maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/js/bootstrap.min.js"></script>
    <script src="/dist/build.js"></script>
  </body>
</html>
```
2. App.vue에 있는 기존 코드를 모두 정리하고 부트스트랩의 간단한 버튼을 추가하는 코드를 아래와 같이 삽입하자.   
```
<template>
  <div id="app">
    <button tyupe="button" class="btn btn-primary btn-lg">Large button</button>
  </div>
</template>

<script>
export default {
  name : 'app',
  data () {
    return {
      msg : 'Welcome to Your Vue.js App'
    }
  }
}
</script>
```
3. 이 애플리케이션을 실행하면 아래와 같은 결과 화면이 나온다.   
![image](https://user-images.githubusercontent.com/33191974/150063607-c2a56909-2f5a-4435-b13d-fca61b3f4245.png)  
부트스트랩 라이브러리는 [Tip 2]에서 다룬 제이쿼리 UI 플러그인과 다르게 index.html 파일에 라이브러리 로딩 스크립트를  
추가하여 간단하게 연동할 수 있다. 

# 뷰와 차트 라이브러리 연동하기
차트 라이브러리 또한 뷰 - 부트스트랩 라이브러리와 마찬가지로 뷰의 렌더링 방식에 맞춰 지원하는 VueChartJS와 같은  
라이브러리가 있다. 하지만 아직은 D3, AmChart와 같은 대중적인 차트들은 뷰와 쉽게 결합될 수 있는 형태로 제공되지  
않는다. 따라서 기존의 차트 라이브러리 역시 뷰와 함께 사용할 줄 알아야 실무 레벨에서의 애플리케이션 개발이 가능하다.   
  
앞에서 연동한 부트스트랩 라이브러리에 이어서 실제로 기업용 애플리케이션에서 자주 사용하는 차트 라이브러리 1개를  
같이 연동해 보자.  차트 라이브러리는 하이 차트(HighChart)를 사용한다.   
  
4. 앞에서 구축한 프로젝트의 index.html에 아래와 같이 하이 차트의 CDN 주소를 추가한다.  
```
<body>
  <div id="app"></div>
  <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.3/umd/popper.min.js"></script>
  <script src="https:/maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/js/bootstrap.min.js"></script>
  <!-- 이 부분 -->
  <script src="https://code.highcharts.com/highcharts.js"></script>
  <script src="/dist/build.js"></script>
</body>
```
5. src 폴더 밑에 Chart.vue 파일을 새로 생성하고 아래와 같이 코딩한다.     
``` 
<template>
    <div id="container" style="width:100%; height:400px;"></div>
</template>

<script>
export default {
    mounted() {
        Highcharts.chart('container', {
            chart : {
                type : 'bar'
            },
            title : {
                text : 'Fruit Consumption'
            },
            xAxis : {
                categories : ['Apples', 'Bananas', 'Oranges']
            },
            yAxis : {
                title : {
                    text : 'Fruit eaten'
                }
            },
            series : [{
                name : 'Jane',
                data : [1, 0, 4]
            }, {
                name : 'John',
                data : [5, 7, 3]
            }]
        });
    }
}
</script>
```  
위 코드는 하이 챠트 공식 사이트에 나와 있는 막대(bar) 차트의 샘플을 이용한 코드로, 두 사람의 과일 소비량을   
막대 그래프로 나타낸 것이다. 차트를 연동하기 위해 `<template>` 안에는 차트를 표시할 간단한 `<div>` 태그를  
지정하고 너비와 높이를 지정한다. 그리고 `<script>`에는 mounted()를 추가하고 하이 차트에서 제공하는 막대 챠트  
예제 코드를 추가한다. 하이 챠트 예제 코드는 https://www.highcharts.com/docs/getting-started/your-first-chart  
에서 가져온다. 
  
6. App.vue에서 Chart.vue를 컴포넌트로 등록하고 `<template>`에 컴포넌트 태그를 추가한다.   
```
<template>
  <div id="app">
    <button tyupe="button" class="btn btn-primary btn-lg">Large button</button>
    <!-- 차트 표시 -->
    <Chart></Chart>
  </div>
</template>

<script>
import Chart from './Chart.vue'

export default {
  name : 'app',
  data () {
    return {
      msg : 'Welcome to Your Vue.js App'
    }
  },
  // 차트 컴포넌트 등록
  components : {
    Chart
  }

}
</script>
```
[Tip 2]의 7번과 같은 방법으로 Chart 컴포넌트를 import하고, components 속성에 등록한다.  
등록한 컴포넌트는 `<Chart>`로 화면에 표시한다.   
  
7. 변경된 코드를 저장하고 다시 브라우저를 실행하면 아래와 같은 화면이 나온다.  
![image](https://user-images.githubusercontent.com/33191974/150065930-2f4c8894-feb1-4391-9943-485d598b22ad.png)  
  
지금까지 뷰를 실무에서 바로 활용할 수 있도록 대중적인 제이쿼리를 비롯한 외부 라이브러리 연동 방법에 대해   
알아봤다.  





  



















