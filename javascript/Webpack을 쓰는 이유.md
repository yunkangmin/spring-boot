Webpack은 여러개 파일을 하나의 파일로 합쳐주는 모듈 번들러(Module bundler)이다.  
# Webpack을 쓰는 이유는 무엇일까?
표준화된 모듈화 기법이 등장한 것은 ES2015부터인데, Webpack이 필요한 이유를 알기 위해서는 [모듈화](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=knix008&logNo=220698639553)    
역사를 알아볼 필요가 있다.   

# 1. Intro
## import /export 구문이 없었던 모듈 이전 상황
자바스크립트는 script 태그를 사용하여 외부의 스크립트 파일을 가져올 수는 있지만, 파일마다 독립적인  
파일 스코프를 갖지 않고 하나의 전역 객체(Global Object)를 공유한다.   
  
예를들어 math.js와 app.js를 스크립트 태그에 넣는다고 하자. math.js를 먼저 로딩하고 app.js을 로딩한다.   
```html
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <script src="./src/math.js"></script>
    <script src="./src/app.js"></script>
  </body>
</html>
```
이 때 각각 같은 변수이름을 사용한다면 가장 뒤에 쓰인 스크립트를 기준으로 변수이름이 적용된다.  

> math.js
```
function sum(x, y) {
  return x + y;
}
```

> app.js
```
sum(1, 2)
console.log(sum(1, 2));
```

## 전역 스코프에 함수가 노출되면 생기는 문제점
여기서 문제는 sum이 전역스코프에 노출되는 것이다.   
```
sum = 1;
sum(3, 4) //Uncaught TypeError: sum is not a function
```
만약 sum에 1을 새롭게 할당한다면 이제는 sum이라는 함수를 호출할 수 없는 상황이 된다.  
즉, 다른 파일에서 같은 변수이름을 쓰면 충돌한다. 이렇게 스코프 문제가 발생하고, 의존성  
관리 문제도 해결되지 않으므로 모듈화를 구현하기 어렵다.   

# 2. IIFE
즉시 실행 함수 표현(IIFE, Immediately Invoked Function Expression)은 정의되자마자 즉시  
실행되는 Javascript Function을 말한다.  
```
(function() {
  statements
})()
```
IIFE 내부에서 정의된 변수는 외부 범위에서 접근이 불가능하다.  
```
(function() {
  const aName = "Mary"
})()

aName //Uncaught ReferenceError: aName is not defined
```
IIFE를 변수에 할당하면 IIFE 자체는 저장되지 않고, 함수가 실행된 결과만 저장된다.  
```
const result = (function() {
  const name = "Mary"
  return name
})()
result // "Mary"
```
IIFE는 스코프 문제를 해결했지만 바로 실행한다는 점에서 모듈화의 해결책은 아니다. 이렇게  
모듈 기능은 반드시 해결해야할 과제였다. 그래서 제안된 것이 AMD와 CommonJS이다.

# 3. CommonJS와 AMD
## CommonJS
CommonJS는 자바스크립트를 브라우저에서뿐만 아니라, 서버사이드 애플리케이션이나 데스크톱 애플리케이션  
에서도 사용하려고 조직한 자발적 워킹 그룹이다. 대표적으로 서버 사이드 플랫폼인 Node.js에서 이를  
사용한다.   
  
exports 키워드로 모듈을 만들고 require() 함수로 임포트하는 방식이다.   
1. 전역변수와 지역변수를 분리하여 모듈이 독립적인 실행 영역을 갖게 된다.
2. script 태그로 파일을 가져오는 것이 아니라 필요한 함수나 변수를 가져올 수 있다.
3. exports와 require를 이용하여 의존성 관리도 편해졌다.

이렇게 CommonJS는 모듈화의 조건을 충족시키지만 이 방식은 브라우저에서는 필요한 모듈을 모두 내려받을  
때까지 아무것도 할 수 없게 된다는 결정적인 단점이 있었다.  

## AMD
AMD(Asynchronous Module Definition)는 비동기로 로딩되는 환경에서 모듈을 사용하는 것이 목표다.  
이 방식은 define 함수 내에 코드를 작성함으로서 스코프 분리가 가능하다.   
```
define(['./foo.js', './boo.js'], function(foo, boo) {
//
})
```

## ES6 모듈
ES6에서는 export를 이용해 모듈로 만들고 import로 가져온다.  

> math.js
```
export function sum(x, y) {
  return x + y;
}
```
> app.js
```
import * as math from "./math.js"
// import {sum} from "./math.js"
// sum만 가져오고 싶다면 이렇게 사용할 수도 있다.
console.log(math.sum(1, 2));
```
import * as name은 모든 export를 가져오고 name 매개변수는 모듈 객체의 이름으로 export를 참조하기 위한  
일종의 네임 스페이스로 사용된다.   
  
ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가했다.  
script 태그에 type="module" 속성을 추가하면 모듈로 사용할 수 있다.  
```
<script type="module" src="./src/app.js"></script>
```
이 때는 app.js에서 math를 가져오기 때문에 math.js은 따로 로드하지 않아도 된다.  
  
그러나 아직까지는 모든 브라우저에서 지원하지 않기 때문에 브라우저와 무관하게 사용할 수 있는 모듈이  
필요하다. 

# 4. Webpack  
![image](https://user-images.githubusercontent.com/33191974/148634933-7b2c2cbf-0c39-41d9-9be9-f9b908ecc5ba.png)

그래서 웹팩을 사용한다. 웹팩은 하나의 시작점(Entry point)으로부터 의존적인 모듈을 전부 찾아내서  
하나의 파일로 만든다. 이 결과물을 Output이라고 한다.   

## 4-1 설치 
Webpack4 이후 버전부터는 cli를 같이 설치해야 커맨드라인에서 사용할 수 있다.   
```
$ npm install --save-dev webpack

# 특정 버전 설치
$ npm install --save-dev webpack@<version>

$ npm install --save-dev webpack-cli
```
--save-dev 옵션이나 -D 옵션을 추가하여 설치하면 package.json 파일 안에서 dependencies가 아니라   
devDependencies에 기록이 되는데, 라이브러리를 설치할 때 어플리케이션에서 직접쓰이는 것과 개발환경에  
쓰이는 것을 이렇게 분리하여 설치하는 것이 좋다.   
```
// ex)
  
"dependencies" : {
  "react-switch" : "^5.0.1"
),
"devDependencies" : {
  "prettier" : "^1.19.1",
    "webpack" : "^4.41.6",
    "webpack-cli" : "^3.3.11"
},
```
> cf) npm install (plugin) --save는 빌드시 플러그인이 포함되지만, npm install (plugin) --save-dev  
> 로 설치한 플러그인은 --production 빌드시 해당 플러그인이 포함되지 않는다.
  
## 4-2. webpack.config.js
webpack.config.js은 Webpack이 실행될 때 참조하는 설정 파일이다. 최상단에 webpack.config.js 파일을  
만들면 이를 웹팩에서 자동으로 사용한다.   
```
const path = require("path")
  
module.exports = {
  mode : "development",
  entry : {
    main : "./src/app.js",
  },
  output : {
    filename : "[name].js",
    path : path.resolve("./dist"),
  },
}
```
첫줄의 path는 경로를 만들어주며 Node.js에 기본으로 깔려있는 패키지이다. 웹팩자체가 노드에서 돌아가기  
때문에 이 모듈도 노드형 모듈을 사용한 것이다. import path from "path"와 같다.  
(webpack.config.js는 모던자바스크립트 파일이 아니라서, import를 쓸 수 없다.)
![image](https://user-images.githubusercontent.com/33191974/148635278-4f29bfc2-5bc7-4bc7-899b-d9fc89ed7b92.png)  
이제 package.json의 script에 build 명령어로 webpack을 추가하고 npm run build를 실행하면 결과물이  
main.js이며 dist 디렉토리가 생성되는 것을 확인할 수 있다. dist 디렉토리 안에는 코드들이 합쳐진  
main.js가 하위 파일로 생성된다.   

> index.html
```
<script src="./dist/main.js"></script>
```
따라서 index.html에서는 ./src/app.js가 아니라 ./dist/main.js를 로드하면 된다.  
이제 모듈을 지원하지 않는 브라우저에서도 동작하는 코드가 됬다. 또한 브라우저가 모듈시스템을  
지원하지 않아도 되기때문에 type을 명시하지 않아도 된다.   

...
  
  
    
    
  












































