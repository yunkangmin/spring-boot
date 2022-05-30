## 이벤트 핸들러 
vue에서 어떤 요소를 클릭하면 특정 기능을 실행하는 법을 배운다.  
UI를 만들 때 유용하다.  
허위매물신고버튼을 누르면 신고수가 1증가하는 이벤트를 만들어보자.   
![image](https://user-images.githubusercontent.com/33191974/147911735-133aa2d8-3dc6-4a11-86f0-3bcc89d3fad9.png)  
예전에는 아래와 같이 onclick을 사용했다.  
```
<div>
  <h4>{{ products[0] }}</h4>
  <p>{{ price2 }}  만원</p>
  <button onclick="">허위매물신고</button> <span>신고수 : 0</span>
</div>
 <div>
  <h4>{{ products[1] }}</h4>
  <p>{{ price2 }}  만원</p>
</div>
 <div>
  <h4>{{ products[2] }}</h4>
  <p>{{ price2 }}  만원</p>
</div>
```
vue에서는 v-on:click=""처럼 사용하면 된다.   
""안에는 자바스크립트 코드를 작성하면 된다.   
입력할 코드가 길면 함수를 작성해도 된다.  
@click=""을 사용해도 된다.(v-on의 약자 @)
```
<div>
  <h4>{{ products[0] }}</h4>
  <p>{{ price2 }}  만원</p>
  <button v-on:click="">허위매물신고</button> <span>신고수 : 0</span>
</div>
 <div>
  <h4>{{ products[1] }}</h4>
  <p>{{ price2 }}  만원</p>
</div>
 <div>
  <h4>{{ products[2] }}</h4>
  <p>{{ price2 }}  만원</p>
</div>
```
만약 허위매물신고버튼을 누르면 신고수가 1씩 증가하는 것을 자바스크립트로 구현한다면  
아래와 같은 것이다.  
1. 버튼 누르면 숫자 찾아서 + 1
2. +1된 걸 HTML에 반영
  
하지만 vue에서는 실시간 렌더링이 되서 데이터가 변경되면 알아서 관련된 html에 반영을 한다.    
그래서 우리가 조작할 건 {{ 신고수 }} 밖에 없다.  
즉, 허위매물신고버튼을 누르면 데이터만 +1해주면 된다. 
```
<div>
  <h4>{{ products[0] }}</h4>
  <p>{{ price2 }}  만원</p>
  <!-- 신고수라는 변수에 1을 더한다. -->
  <button @click="신고수++">허위매물신고</button> 
  <span>신고수 : {{ 신고수 }}</span>
</div>
 <div>
  <h4>{{ products[1] }}</h4>
  <p>{{ price2 }}  만원</p>
</div>
 <div>
  <h4>{{ products[2] }}</h4>
  <p>{{ price2 }}  만원</p>
</div>

...

export default {
  name: 'App',
  //데이터 보관함
  data() {
    //return 문안에 데이터를 보관한다. 
    return {
    
      ...
      //추가된 부분
      신고수 : 0,
    }
  },
  components: {
   
  }
}
</script>
```
## 여러가지 이벤트 종류
1. @mouseover 
...
ctrl + space를 누르면 여러가지 이벤트가 나온다.  

## 함수만드는 법
함수를 만들고 싶으면 mothods : {}를 만든다. 
```
...

<div>
  <h4>{{ products[0] }}</h4>
  <p>{{ price2 }}  만원</p>
  <!-- 함수명만 사용한다. -->
  <button @click="increase">허위매물신고</button> 
  <span>신고수 : {{ 신고수 }}</span>
</div>

...

export default {
  name: 'App',
  //데이터 보관함
  data() {
    //return 문안에 데이터를 보관한다. 
    return {
    
      ...
      
      신고수 : 0,
    }
  },
  methods : {
    increase() {
      //this를 꼭 붙여줘야 한다.  
      //안붙이면 에러.
      //전체 object를 가리킨다. 
      this.신고수++;
    }
  },
  components: {
   
  }
}
```
아래 코드는 상품 3개를 만들고 각각의 버튼을 누르면 각각의 신고수가 증가하는 코드이다.
```
<template>

  ...
  
  <div>
    <h4>{{ products[0] }}</h4>
    <p>{{ price2 }}  만원</p>
    <!-- 신고수라는 변수에 1을 더한다. -->
    <button @click="increase(0)">허위매물신고</button> <span>신고수 : {{ 신고수[0] }}</span>
  </div>
   <div>
    <h4>{{ products[1] }}</h4>
    <p>{{ price2 }}  만원</p>
    <button @click="increase(1)">허위매물신고</button> <span>신고수 : {{ 신고수[1] }}</span>
  </div>
   <div>
    <h4>{{ products[2] }}</h4>
    <p>{{ price2 }}  만원</p>
    <button @click="increase(2)">허위매물신고</button> <span>신고수 : {{ 신고수[2] }}</span>
  </div>
</template>

<script>

export default {
  name: 'App',
  //데이터 보관함
  data() {
    //return 문안에 데이터를 보관한다. 
    return {
    
      ...
      
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

```
상품 3개가 반복되니 상품부분을 반복문으로 변경해보자.  
```
...

<div v-for="i in 3" :key="i">
  <h4>{{ products[i - 1] }}</h4>
  <p>{{ price2 }}  만원</p>
  <!-- 신고수라는 변수에 1을 더한다. -->
  <button @click="increase(i - 1)">허위매물신고</button> <span>신고수 : {{ 신고수[i - 1] }}</span>
</div>

...

```

최종코드    
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

  <!-- <div>
    <h4 class="red" :style="스타일">XX 원룸</h4>
    <p>{{ price1 }}만원</p>
  </div>
  <div>
    <h4>XX 원룸</h4>
    <p>{{ price2 }}  만원</p>
  </div> -->
  <div v-for="i in 3" :key="i">
    <h4>{{ products[i - 1] }}</h4>
    <p>{{ price2 }}  만원</p>
    <!-- 신고수라는 변수에 1을 더한다. -->
    <button @click="increase(i - 1)">허위매물신고</button> <span>신고수 : {{ 신고수[i - 1] }}</span>
  </div>
  <!-- <div>
    <h4>{{ products[0] }}</h4>
    <p>{{ price2 }}  만원</p> -->
    <!-- 신고수라는 변수에 1을 더한다. -->
    <!-- <button @click="increase(0)">허위매물신고</button> <span>신고수 : {{ 신고수[0] }}</span>
  </div>
   <div>
    <h4>{{ products[1] }}</h4>
    <p>{{ price2 }}  만원</p>
    <button @click="increase(1)">허위매물신고</button> <span>신고수 : {{ 신고수[1] }}</span>
  </div>
   <div>
    <h4>{{ products[2] }}</h4>
    <p>{{ price2 }}  만원</p>
    <button @click="increase(2)">허위매물신고</button> <span>신고수 : {{ 신고수[2] }}</span>
  </div> --> 


  <!-- <div v-for="(e, i) in products" :key="i">
    <h4>{{ e }}</h4>
    <p>{{ price3 }} 만원</p>
  </div> -->
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

























