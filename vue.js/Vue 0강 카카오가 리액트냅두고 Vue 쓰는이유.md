Vue.js 쓰는 이유는 Single Page Application, Web-app 만들 때 쓴다.  
[vue.js로 만든 사이트](https://vibe.naver.com/today)에 접속해서 
좌측 메뉴들을 클릭하면 화면이 새로고침되지 않고 변경되는 걸 확인할 수 있는데  
이런 사이트들을 Web-app이라고 한다. Web-app은 app처럼 쓸 수 있다고 해서 붙여진 이름이다.    
이러한 Web-app을 만들수 있는 도구로는 React, Augular, Vue가 있다.  
vue를 쓰는 이유는 제일 쉽기 때문이다. 어떻게 쉬운지 보자.

### 1. Vue를 선호하는 이유
실제 코드를 보자.
if 문  
```
//React의 if
function App() {
  
  const conditional = () => {
    //React의 if
    if (true) {
      return <p>이거보여주셈</p>
    } else {
      return <p>이거말고</p>
    }
  }
  
  returh (
    <div>{conditional()}</div>
  );
}

//Vue의 if
<template>
  <div>
    <p v-if="show">이거보여주셈</p>
    <p v-else>이거말고</p>
  </div>
</template>
```
for 문  
```
//React의 for
import React from 'react';

function List() {
  const todos = ['Eat', 'Sleep', 'Muyaho', 'Work'];
  return {
    //React의 for
    <ul>
      {
        todos.map(todo) =>
          <li key={todo}>{todo}</li>
        }
      }
    </ul>
  );
}
export default List;

//Vue의 for
<template>
  //Vue의 for
  <ul>
    <li v-for="todo" in todos" :keys="todo">{{ todo }}</li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      todos: [ 'Eat', 'Sleep', 'Muyaho', 'Work'],
    }
  },
}
</script>
```
state의 변경  
```
//React의 state 변경
const [human, setHuman] = React.useState(['Park', 18, 'Male'])

let humanCopy = [...human];
humanCopy[0] = 'Kim';
setHuman(humanCopy);

return {
  human: ['Park', 18, 'Male'],
}

//Vue의 state 변경
this.human[0] = 'Kim'
```

### 2. Vue를 선호하는 이유
코드를 짤 때 방법이 정해져 있다(right way가 있다).   
예를 들어보면, 
1. <HTML>을 여러개 만들고 싶다.
   - React  
      - {map}
      - forEach
      - for, for in, for of
      - 컴포넌트 render() 바깥에서 쓸지 안에서 쓸지
   - Vue  
      - v-for
2. 조건부로 html을 보여주고 싶을 때
   - React
      - tenary operator
      - &&, ||
      - if else
      - enum
      - ... 등
   - Vue
      - v-if, v-else

vue를 사용하면 코드스타일이 통일되어 개발자마다 제각각으로 코드를 짜는 것을 방지해준다.   
또한 방법이 하나이기 때문에 특히 초보일수록 좋다(이게 맞는 방법인지 고민할 필요가 없다).    

### 3. Vue를 선호하는 이유
React보다 HTML 렌더링이 더 빠르다.    
  
### 4. Vue를 선호하는 이유
업데이트가 잘 된다.    
  
































          
          
          
          
          
          
          
          
          
          
          
          
          
          
          

