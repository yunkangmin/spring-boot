# 커링(Curring)이란?
커링은 인자를 여러 개 받는 함수를 분리하여, 인자를 하나씩만 받는 함수의 체인으로   
만드는 방법이다.   
함수형 프로그래밍 기법 중 하나로 함수를 재사용하는데 유용하게 쓰일 수 있는 기법이다.  
자바스크립트 내부에는 커링이 내장되어 있지 않지만 자바스크립트로도 구현이 가능하다.   

## 예를 들어
```javascript
function greet(greeting, name) {
  console.log(greeting + ", " + name);
}
```
위의 함수가 제대로 동작하려면 greeting과 name을 전달받아야 한다.  
이 것을 커링 함수로 수정한다면  
```javascript
function greet(greeting) {
  return function(name) {
    console.log(greeting + ", " + name);
  }
}
```
첫 번째 함수가 greeing만을 인자로 받고  
그 안에 있는 반환되는 내부 함수가 name을 인자로 받는 형태로 수정을 했다.  
```javascript
var hello = greet('hello');
hello('world'); // 'hello, world'
hello('john'); // 'hello. john'
```
위의 코드들처럼 어떤 함수를 호출할 때 매개 변수가 항상 비슷할 때 유용하게 사용할  
수 있는 기법이다.  
매번 새로운 인자를 전달하지 않아도 바깥에 있는 함수가 인자를 전달하고, 새로운  
인자는 내부 함수가 받게 된다. 
주의할 게 있다면 커링 함수에서는 인자의 순서가 매우 중요하다.  
외부 함수 즉 가장 먼저 받는 인자일 수록 변하지 않아야 하고, 내부 함수 즉 가장  
나중에 받는 인자일수록 변할 가능성이 높다.  
이런 것들을 생각해가며 설계를 해야한다.  























