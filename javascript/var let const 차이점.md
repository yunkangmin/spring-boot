# 정리
ES6에서 var, let, const를 모두 호이스팅하는데 var만 아래에서 변수선언을 해도   
위에서 선언단계와 초기화단계가 실행되기 때문에 변수 선언문 이전에 변수를   
참조할 수 있다.    
  
하지만 let과 const는 선언단계만 실행되기 때문에 var와 같이 변수 선언문   
이전에 참조하면 에러가 발생한다.   

---

JavaScript에서 변수 선언 방식인 var, let, const의 차이점에 대해 알아보자.  

## 1. 변수 선언 방식
우선 var는 변수 선언 방식에 있어서 큰 단점을 가지고 있다.  
```
var name = 'bathingape';
console.log(name); //bathingape
  
var name = 'javascript';
console.log(name) //javascript;
```
변수를 한 번 더 선언했음에도 불구하고, 에러가 나오지 않고 각기 다른 값이 출력되는 것을   
볼 수 있다. 
  
이는 유연한 변수 선언으로 테스트에는 편리할 수 있겠으나, 코드량이 많아 진다면 어디에서  
어떻게 사용될지도 파악하기 힘들뿐더러 값이 바뀔 우려가 있다.   
  
그래서 ES6 이후, 이를 보완하기 위해 추가된 변수 선언 방식이 let과 const이다.  
위의 코드에서 변수 선언 방식만 바꿔보자.   
```
let name = 'bathingape';
console.log(name); //bathingape'

let name = 'javascript';
console.log(name);
//Uncaught SyntaxError: Identifier 'name' has already been declared
```
name이 이미 선언되었다는 에러 메시지가 나온다.(const도 마찬가지)   
변수 재선언이 되지 않는다.   
그렇다면 let과 const의 차이점은 무엇일까?  
이 둘의 차이점은 immutable 여부이다.  
let은 변수에 재할당이 가능하다. 그렇지만,   
``` 
let name = 'bathingape';
console.log(name); //bathingape

let name= 'javascript';
console.log(name);
//Uncaught SyntaxError: Identifier 'name' has already been declared

name = 'react';
console.log(name); //react
```
const는 변수 재선언, 변수 재할당 모두 불가능하다.  


## 2. 호이스팅
**호이스팅(Hoisting)이란, var 선언문이나 function 선언문 등을 해당 [스코프](https://github.com/yunkangmin/spring-boot/blob/main/javascript/%EC%8A%A4%EC%BD%94%ED%94%84.md)의 선두로 옮긴 것처럼  
동작하는 특성을 말한다.**  
  
자바스크립트는 ES6에서 도입된 let, const를 포함하여 모든 선언(var, let, const, function, function*,  
class)을 호이스팅한다.  
  
하지만, var로 선언된 변수와는 달리 let로 선언된 변수를 선언문 이전에 참조하면 참조 에러(ReferenceError)  
가 발생한다.  
```
console.log(foo); //undefined
var foo;
  
console.log(bar); //Error: Uncaught ReferenceError: bar is not defined
let bar;
```
이는 let로 선언된 변수는 스코프의 시작에서 변수의 선언까지 일시적 사각지대(Temporal Dead Zone; TDZ)에  
빠지기 때문이다.  
  
참고로 변수는 선언단계 > 초기화 단계 > 할당 단계에 걸쳐 생성되는데   
var로 선언된 변수는 선언 단계와 초기화 단계가 한번에 이루어진다.  
하지만,  
```
//스코프의 선두에서 선언 단계와 초기화 단계가 실행된다.(아래에서 변수 선언을 해도(호이스팅 효과))  
//따라서 변수 선언문 이전에 변수를 참조할 수 있다.  

console.log(foo); //undefined

var foo;
console.log(foo); //undefined

foo = 1; //할당문에서 할당 단계가 실행된다.  
console.log(foo); //1
```
let로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.  
```
//스코프의 선두에서 선언 단계가 실행된다. 
//아직 변수가 초기화(메모리 공간 확보와 undefined로 초기화)되지 않았다.
//따라서 변수 선언문 이전에 변수를 참조할 수 없다.

//ReferenceError: foo is not defined
console.log(foo); 

//변수 선언문에서 초기화 단계가 실행된다.
//undefined
let foo; 

//할당문에서 할당 단계가 실행된다.
foo = 1;
console.log(foo); //1
```

## 정리
그렇다면 어떤 변수 선언 방식을 써야할까?  
변수 선언에는 기본적으로 const를 사용하고 재할당이 필요한 경우에 한정해 let을 사용하는 것이 좋다.  
그리고 객체를 재할당하는 경우는 생각보다 흔하지 않다. const를 사용하면 의도치 않은 재할당을 방지해  
주기 때문에 보다 안전하다.
1. 재할당이 필요한 경우에 한정해 let을 사용한다. 이 때 변수의 스코프는 최대한 좁게 만든다.
2. 재할당이 필요없는 상수와 객체에는 const를 사용한다.  


















































