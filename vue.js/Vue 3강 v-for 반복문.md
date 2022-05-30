상단 메뉴를 만들어보자.  
코드를 짜다보면 반복적인 HTML 태그를 사용해야 할 때가 있다.  
이럴때는 Vue의 HTML 반복문을 사용하면 된다.  
```
<태그 v-for="작명 in 반복할횟수" :key="작명">  
```
:key="작명"은 꼭 사용해야 에러가 안난다.  
모든 html 요소에 사용가능하다.  
```
<template>
  //추가된 부분
  <div class="menu">
    <a v-for="작명 in 3" :key="작명">Home</a>
  </div>
  //추가된 부분

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

//추가된 부분
.menu {
  background: darkslateblue;
  padding: 15px;
  border-radius: 5px;
}
.menu a {
  color : white;
  padding: 10px;
}
 //추가된 부분
</style>

```
![image](https://user-images.githubusercontent.com/33191974/147875032-ececa7ea-dbf8-4c51-bbe3-145f6b67e661.png)    
하지만 Home만 3개 생겼다.  
아래와 같이 짜면 각각 다른 이름으로 생성할 수 있다.  
```
 <a v-for="작명 in 메뉴들" :key="작명">{{ 작명 }}</a>

//데이터에는 아래 '메뉴들'과 같이 세팅한다.  
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
    //html에서 <a v-for="작명 in 메뉴들" :key="작명">Home</a> 이런 식으로  
    //사용한다.  
    메뉴들 : ['Home', 'Shop', 'About'],
  }
},
```
최종본  
```
<template>
  <div class="menu">
    <!-- '메뉴들'은 아래 데이터에서 세팅한 '메뉴들' 
    '메뉴들' 자리에 숫자를 넣으면 해당 숫자만큼 반복되고 
    자료형을 넣으면 자료안의 데이터 개수만큼 반복된다. 그리고
    '작명'부분은 '메뉴들'을 순회할 때마다 '메뉴들'안에 있던 요소로 
    하나씩 변한다. 작명을 사용할 때는 데이터바인딩하듯이 사용하면 된다.
    반복문을 사용할 때는 :key=""를 꼭 사용해야 한다(사용하지 않으면  
    에러남). key는 작명을 구분하기 위한 속성이다. 반복문을 돌린 요소를  
    컴퓨터가 구분하기 위해 사용한다. 그래서 유니크한(중복되지 않는) 
    숫자같은 것을 넣어주면 된다. 작명부분은 아래와 같이 2개를 사용할 수 있는데 
    2개를 사용하면 첫번째는 메뉴들을 하나씩 순회하면 나온 요소이고  
    두번째는 0, 1, 2..이런식으로 1씩 증가하는 정수이다. 
    예)  <a v-for="(작명, i) in 메뉴들" :key="i">{{ i }}</a> -->
    <a v-for="작명 in 메뉴들" :key="작명">{{ 작명 }}</a>
  </div>

  <div>
    <h4 class="red" :style="스타일">XX 원룸</h4>
    <p>{{ price1 }}만원</p>
  </div>
   <div>
    <h4>XX 원룸</h4>
    <p>{{ price2 }}  만원</p>
  </div>
  <div v-for="(e, i) in products" :key="i">
    <h4>{{ e }}</h4>
    <p>{{ price3 }} 만원</p>
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
      //html에서 <a v-for="작명 in 메뉴들" :key="작명">Home</a> 이런 식으로  
      //사용한다.  
      메뉴들 : ['Home', 'Shop', 'About'],
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
}

.menu {
  background: darkslateblue;
  padding: 15px;
  border-radius: 5px;
}
.menu a {
  color : white;
  padding: 10px;
}
</style>
```
![image](https://user-images.githubusercontent.com/33191974/147875701-8ae7f4ae-50f4-4324-ad48-88ac6cebda1a.png)  













































