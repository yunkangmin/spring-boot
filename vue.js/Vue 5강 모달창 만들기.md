vue로 어떤 것을 만들 때 데이터 중심적으로 사고하는 게 중요하다.  
즉, 데이터를 잘 기획해두어야 한다.   
데이터를 어떻게 만들어놔야 편할지를 생각해야 한다.  

## <img>를 넣어보자  
img 폴더에서 room0 ~ 2까지 이미지를 아래 경로에 넣자.  
![image](https://user-images.githubusercontent.com/33191974/147922548-6731e54d-6a45-4df3-b47e-24d2b9596a7b.png)   
```

...

<div>
  <!-- https:// 이런식으로 절대경로는 그냥 넣으면 되고 
  상대경로 예를들어 같은 폴더에 있는 이미지를 넣고 싶다면 
  src/assets 폴더에 미리 준비해둬야 한다. 
  src 경로는 ./부터 시작한다. -->
  <img src="./assets/room0.jpg" class="room-img"/>
  <h4>{{ products[0] }}</h4>
  <p>{{ price1 }}  만원</p>
  <!-- 신고수라는 변수에 1을 더한다. -->
  <button @click="increase(0)">허위매물신고</button> <span>신고수 : {{ 신고수[0] }}</span>
</div>
 <div>
  <img src="./assets/room1.jpg" class="room-img"/>
  <h4>{{ products[1] }}</h4>
  <p>{{ price1 }}  만원</p>
  <button @click="increase(1)">허위매물신고</button> <span>신고수 : {{ 신고수[1] }}</span>
</div>
 <div>
  <img src="./assets/room2.jpg" class="room-img"/>
  <h4>{{ products[2] }}</h4>
  <p>{{ price1 }}  만원</p>
  <button @click="increase(2)">허위매물신고</button> <span>신고수 : {{ 신고수[2] }}</span>
</div> 

...

.room-img {
  width: 100%;
  margin-top: 40px;
}

...
```
![image](https://user-images.githubusercontent.com/33191974/147923162-7e21b4ec-4b9a-4303-803e-e7cae301e2d2.png) 

## Vue에서 모달창 만들기   
이미지나 글씨를 누르면 모달창(상세페이지)이 뜨게 한다.  
모달창 디자인부터 하자.   
```
<template>
  <div class="black-bg">
    <div class="white-bg">
      <h4>상세페이지</h4>
      <p>상세페이지 내용</p>
    </div>
  </div>
  
  ...

</template>

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

...

</style>
```
UI 만드는 법칙이 있다.  
- 동적이 UI 만드는 법
   1. HTML CSS로 미리 디자인해둔다.
   2. UI의 현재 상태를 데이터로 저장해둠
   3. 데이터에 따라 UI가 어떻게 보일지 작성

코드를 직접 짜면서 알아보자.  

### 1. UI의 현재 상태를 데이터로 저장해둠
```
data() {
  return {
    //모달창 상태를 보관하는 데이터
    //모달창은 닫힌 상태와 열린 상태 2가지가 존재한다. 
    //예 0: 닫힘, 1: 열림
    모달창열렸니 : false, 
 
    ...
 
  }
},
```

## 2. 데이터에 따라 UI가 어떻게 보일지 작성
```
<template>
  <!-- 데이터(모달창열렸니)가 true이면 모달창이 보이게  
  v-if="조건식"는 조건식이 참일때만 UI를 보여준다.-->
  <div class="black-bg" v-if="모달창열렸니 == true">
    <div class="white-bg">
      <h4>상세페이지</h4>
      <p>상세페이지 내용</p>
    </div>
  </div>
  
  ...
  
</template>
```

위에 2가지를 하면 UI 켜고 끄기를 쉽게 할 수 있게 된다.  
제목을 누르면 창이 열리게 해보자.  
```
<div>
  <img src="./assets/room0.jpg" class="room-img"/>
  <h4 @click="모달창열렸니 = true">{{ products[0] }}</h4>
  <p>{{ price1 }}  만원</p>
  <!-- 신고수라는 변수에 1을 더한다. -->
  <button @click="increase(0)">허위매물신고</button> <span>신고수 : {{ 신고수[0] }}</span>
</div>
```
![image](https://user-images.githubusercontent.com/33191974/147926329-1eefdfc4-c178-48b2-94a6-e90f154f0aef.png)  

## 정리
동적인 UI를 만들려면  
1. UI 디자인을 해두고  
2. UI의 현재 상태를 데이터로 만들어둔다.  
3. 데이터에 따라 UI가 어떻게 보일지 작성한다.  

위 방법대로 하면 UI를 쉽게 키고 끌수 있다.  

데이터 저장공간을 React에서는 state(상태)라고 부른다.    
UI의 상태를 저장하는 공간이기도 하기 때문이다. 
```
export default { 
  name: 'App',
  data() {
    return {
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
```
모달창 닫기 버튼을 만들어서 닫기기능을 구현해보자.  
```
<template>
  <div class="black-bg" v-if="모달창열렸니 == true">
    <div class="white-bg">
      <h4>상세페이지</h4>
      <p>상세페이지 내용</p>
      <button @click="모달창열렸니 = false">닫기</button>
    </div>
  </div>
  
  ...
</template>
```

전체코드  
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
  <div>
    <!-- https:// 이런식으로 절대경로는 그냥 넣으면 되고 
    상대경로 예를들어 같은 폴더에 있는 이미지를 넣고 싶다면 
    src/assets 폴더에 미리 준비해둬야 한다. 
    src 경로는 ./부터 시작한다. -->
    <img src="./assets/room0.jpg" class="room-img"/>
    <h4 @click="모달창열렸니 = true">{{ products[0] }}</h4>
    <p>{{ price1 }}  만원</p>
    <!-- 신고수라는 변수에 1을 더한다. -->
    <button @click="increase(0)">허위매물신고</button> <span>신고수 : {{ 신고수[0] }}</span>
  </div>
   <div>
    <img src="./assets/room1.jpg" class="room-img"/>
    <h4>{{ products[1] }}</h4>
    <p>{{ price1 }}  만원</p>
    <button @click="increase(1)">허위매물신고</button> <span>신고수 : {{ 신고수[1] }}</span>
  </div>
   <div>
    <img src="./assets/room2.jpg" class="room-img"/>
    <h4>{{ products[2] }}</h4>
    <p>{{ price1 }}  만원</p>
    <button @click="increase(2)">허위매물신고</button> <span>신고수 : {{ 신고수[2] }}</span>
  </div> 
</template>

<script>

export default { 
  name: 'App',
  data() {
    return {
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





































