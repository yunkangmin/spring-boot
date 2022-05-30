이번 시간에는 실제 상품데이터를 가져와서 HTML에서 보여줄 것이다.  
실제 쇼핑몰에서는 서버에서 데이터를 가져와서 보여준다.  

상품데이터  
```
[{
  id : 0,
  title: "Sinrim station 30 meters away",
  image: "https://codingapple1.github.io/vue/room0.jpg",
  content: "18년 신축공사한 남향 원룸 ☀️, 공기청정기 제공",
  price: 340000
  },
  {
  id : 1,
  title: "Changdong Aurora Bedroom(Queen-size)",
  image: "https://codingapple1.github.io/vue/room1.jpg",
  content: "침실만 따로 있는 공용 셰어하우스입니다. 최대 2인 가능",
  price: 450000
  },
  {
  id : 2,
  title: "Geumsan Apartment Flat",
  image: "https://codingapple1.github.io/vue/room2.jpg",
  content: "금산오거리 역세권 아파트입니다. 애완동물 불가능 ?",
  price: 780000
  },
  {
  id : 3,
  title: "Double styled beds Studio Apt",
  image: "https://codingapple1.github.io/vue/room3.jpg",
  content: "무암동인근 2인용 원룸입니다. 전세 전환가능",
  price: 550000
  },
  {
  id : 4,
  title: "MyeongIl Apartment flat",
  image: "https://codingapple1.github.io/vue/room4.jpg",
  content: "탄천동 아파트 월세, 남향, 역 5분거리, 허위매물아님",
  price: 680000
  },
  {
  id : 5,
  title: "Banziha One Room",
  image: "https://codingapple1.github.io/vue/room5.jpg",
  content: "반지하 원룸입니다. 비올 때 물가끔 새는거 빼면 좋아요",
  price: 370000
}];
```
위 상품 데이터를 데이터보관함에 붙여넣기하기에는 많고 실수를 할 수 있기 때문에  
따로 파일로 만든다.  
아래와 같이 src/assets에 만들어보자.   
![image](https://user-images.githubusercontent.com/33191974/148029459-50cb7265-b832-4ee3-a083-c80713e3939e.png)      

## 변수 1개 사용할 때 export/import 문법
oneroom.js 파일에서 App.vue 파일로 변수를 보내고 싶을 때는 아래와 같이 하면 된다.   
보낼 파일에서 'export default 변수명'을 쓴다. 변수명이 export되는 것이다.  
보통 페이지 최하단에 적는다. export를 해야 다른 파일에서 사용할 수 있다.  
  
oneroom.js   
```
var apple = 10;

export default apple
```
  
App.vue  
```
<script>
//import 작명(마음대로 작명해도 됨) from 경로
//import를 하고 변수(작명한 이름으로 사용)를 사용안하면 에러가 난다.  
import apple from './assets/oneroom.js';

...
</script>
```

## 변수 2개이상 사용할 때 export/import 문법
이번에는 변수 2개이상을 사용할 때를 알아보자.   
oneroom.js  
```
var apple = 10;
var apple = 100;

export {apple, apple2}
```
  
App.vue  
```
<script>
//import {변수명(oneroom.js에서 사용한 변수명 그대로)} from '경로';
import {apple} from './assets/oneroom.js';
apple;

...

</script>
```

이제 상품 데이터를 oneroom.js에 붙여넣자.  
  
oneroom.js  
```
export default [{
    id : 0,
    title: "Sinrim station 30 meters away",
    image: "https://codingapple1.github.io/vue/room0.jpg",
    content: "18년 신축공사한 남향 원룸 ☀️, 공기청정기 제공",
    price: 340000
    },
    
    ...
]};    
```
  
App.vue  
```
<script>
import data from './assets/oneroom.js';

...

export default {
  name: 'App';
  data() {
    return {
      원룸들 : data,
      
      ...
    }
  },
  
  ...
  
}
```

oneroom.js 파일 안에 데이터들은 구조가 [{}, {}, {}...] 이런 식이다.  
배열 안에 object가 들어있는 형식이다. 주로 상품데이터를 사용할 때 많이 쓰는 형식이다.  
'{'로 시작하면 무조건 object이다.  
   
App.vue  
```
<template>
  
  ...
  <!-- 
       1. 속성에 데이터바인딩은 :속성명
       2. HTML 태그 안에 데이터바인딩은 {{ 데이터명 }}
   -->
  <div>
    <img :src="원룸들[0].image" class="room-img"/>
    <h4>{{ 원룸들[0].title }}</h4>
    <p>{{ 원룸들[0].price }}원</p>
  </div>

  ...

</template>

...
```
  
상품을 반복문을 사용해서 구현해보자.  
```
<template>
  
  ...
  
  <div v-for="(e, i) in 원룸들" :key="i">
    <img :src="원룸들[i].image" class="room-img"/>
    <h4>{{ 원룸들[i].title }}</h4>
    <p>{{ 원룸들[i].price }}원</p>
  </div>
  
  ...

</template>
```
  
최종코드  
```
<template>
  <!-- 데이터(모달창열렸니)가 true이면 모달창이 보이게  
  v-if="조건식"는 조건식이 참일때만 UI를 보여준다.-->
  <div class="black-bg" v-if="모달창열렸니 == true">
    <div class="white-bg">
      <h4>상세페이지</h4>
      <p>상세페이지 내용</p>
      <button @click="모달창열렸니 = false">닫기</button>
    </div>
  </div>
  <div class="menu">
    <a v-for="작명 in 메뉴들" :key="작명">{{ 작명 }}</a>
  </div>
  <div v-for="(e, i) in 원룸들" :key="i">
    <img :src="원룸들[i].image" class="room-img"/>
    <h4>{{ 원룸들[i].title }}</h4>
    <p>{{ 원룸들[i].price }}원</p>
  </div>
</template>

<script>
import data from './assets/oneroom.js';


export default { 
  name: 'App',
  data() {
    return {
      원룸들 : data, 
      //모달창 상태를 보관하는 데이터
      //모달창은 닫힌 상태와 열린 상태 2가지가 존재한다. 
      //예 0: 닫힘, 1: 열림
      모달창열렸니 : false, 
      price1 : 60,      
      products : ['역삼동원룸', '천호동원룸', '마포구원룸'],
      메뉴들 : ['Home', 'Shop', 'About'],
      신고수 : [0, 0, 0],
    }
  },
  methods : {
    increase(i) {
      this.신고수[i]++;
    }
  },
  components: {
   
  }
}
</script>

<style>
body {
  margin : 0
}
div {
  box-sizing: border-box;
}
.black-bg {
  width: 100%; height: 100%;
  background: rgba(0, 0, 0, 0.5);
  position: fixed; padding: 20px;
}
.white-bg {
  width: 100%; background: white;
  border-radius: 8px;
  padding: 20px;
}

.room-img {
  width: 100%;
  margin-top: 40px;
}

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















