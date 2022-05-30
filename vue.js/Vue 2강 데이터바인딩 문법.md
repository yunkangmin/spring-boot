부동산 소개 사이트를 만들 것이다.   
실시간 미리보기(npm run serve)를 해보자.  
만약 제대로 뜨지 않는다면 vue 프로젝트 폴더를 에디터에서 오픈을 제대로 해보자.   
App.vue 파일을  열자.  
아래와 같이 HelloWorld로 표시된 부분은 지우자.  
```
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  -> <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
-> import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
   ->  HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```
<template> 부분에 html css 짜던 대로 짜면 된다.  
쇼핑몰 상품을 진열해보자.  
데이터 바인딩을 해보자. 자바스크립트 데이터를 HTML에 넣는 문법이다.  
기존에 자바스크립트에서는 아래와 같이 데이터 세팅을 했을 것이다.  
```
document.getElementById().innerHTML = ??;
```
vue에서는 위와 같이 길게 코드를 짜지 않고 간단히 데이터를 세팅할 수 있다.  
먼저 데이터를 어딘가에 저장해야 한다.   
```
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div>
    <h4>XX 원룸</h4>
    <p>{{ price1 }}만원</p>
  </div>
   <div>
    <h4>XX 원룸</h4>
    <p>{{ price2 }}  만원</p>
  </div>
</template>

<script>

export default {
  name: 'App',
  //데이터 보관함
  data() {
    //return 문안에 데이터를 보관한다. 
    return {
      //데이터는 object 자료로 저장하면 된다.  
      //{ 자료이름 : 자료내용 }
      //설정한 데이터를 화면에 표시하고 싶다면 아래와 같이 사용한다.
      //{{데이터이름}}
      price1 : 60,
      price2 : 70,
    }
  },
  components: {
   
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

## {{데이터바인딩}}하는 이유
1.  html에 하드코딩을 하면 나중에 변경하기 어렵다.  
쇼핑몰 상품들은 매일 가격이 변동된다.  
가변적인 데이터는 데이터바인딩을 사용하는게 옳은 방법이다.  
2. vue가 제공하는 실시간 자동렌더링 기능을 이용하기 위해서이다.   
vue는 data(데이터 보관함에 설정한 가격)을 변경하면 html에도 실시간으로  
반영됨. 실시간 자동 렌더링이 되면 웹앱같은 사이트(새로고침되지 않는)를 만들 수 있다.  
자주 변경되지 않는 로고(예) 쇼핑몰 이름)는 데이터바인딩을 할 필요가 없다.   
그래서 html에 하드코딩해도 된다(예) 만원(가격단위).    
이렇게 되면 어떤 것을 데이터바인딩하면되는지 안되는지 판단이 된다.   
  
## {{데이터바인딩}}하는 이유 정리
1. 자주 변경될 데이터들은 데이터보관함에 저장해놨다가 html에서 사용한다.  
  
데이터바인딩은 HTML 속성(예)class명) 같은 것도 가능하다.  
html에서 사용할 때는 {{데이터명}} 이런 식으로 사용하는게 아니라 :style 이런식으로  
속성 앞에 콜론을 사용하고 그안에 데이터 명을 표시한다.   
:속성="데이터이름"  
```
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div>
    원룸샵
    <h4 class="red" :style="스타일">XX 원룸</h4>
    <p>{{ price1 }}만원</p>
  </div>
   <div>
    <h4>XX 원룸</h4>
    <p>{{ price2 }}  만원</p>
  </div>
</template>

<script>

export default {
  name: 'App',
  //데이터 보관함
  data() {
    //return 문안에 데이터를 보관한다. 
    return {
      //데이터는 object 자료로 저장하면 된다.  
      //{ 자료이름 : 자료내용 }
      //설정한 데이터를 화면에 표시하고 싶다면 아래와 같이 사용한다.
      //{{데이터이름}}
      price1 : 60,
      price2 : 70,
      스타일 : 'color : blue',
    }
  },
  components: {
   
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```  

## 데이터바인딩에서 배열 사용법
```
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <div>
    원룸샵
    <h4 class="red" :style="스타일">XX 원룸</h4>
    <p>{{ price1 }}만원</p>
  </div>
   <div>
    <h4>XX 원룸</h4>
    <p>{{ price2 }}  만원</p>
  </div>
  <div>
    <h4>{{products[0]}}</h4>
    <p>{{ price3 }}  만원</p>
  </div>
   <div>
    <h4>{{products[1]}}</h4>
    <p>{{ price3 }}  만원</p>
  </div>
   <div>
    <h4>{{products[2]}}</h4>
    <p>{{ price3 }}  만원</p>
  </div>
</template>

<script>

export default {
  name: 'App',
  //데이터 보관함
  data() {
    //return 문안에 데이터를 보관한다. 
    return {
      //데이터는 object 자료로 저장하면 된다.  
      //{ 자료이름 : 자료내용 }
      //설정한 데이터를 화면에 표시하고 싶다면 아래와 같이 사용한다.
      //{{데이터이름}}
      price1 : 60,
      price2 : 70,
      스타일 : 'color : blue',
      products : ['역삼동원룸', '천호동원룸', '마포구원룸'],
      price3 : 100,
    }
  },
  components: {
   
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```  
  
  













