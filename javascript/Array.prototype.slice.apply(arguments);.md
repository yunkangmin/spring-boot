```javascript
function argTest() {
  console.log(arguments);// {"0" : 4 }
  Array.prototype.slice.apply(arguments); // 4
  
  Array.prototype.slice.apply({'0': 4, '1': 5, length: 2}) // 4, 5
  Array.prototype.slice.apply({'0': 4, '1': 5, length: 2}, [1]) // 5
  Array.prototype.slice.call({'0': 4, '1': 5, length: 2}, 1) // 5
}
argTest(4);
```
```javascript
Array.prototype.slice.apply(arguments);
```
arguments는 배열이 아니다. 콘솔로 찍어보면 `{"0": 4}` 이렇게 생긴 오브젝트가  
찍힌다.  
그런데 slice로 자를 수 있다니?  
그래서 똑같이 생긴 오브젝트를 만들어서 해봤는데 안된다.  
검색해보니, length property가 있어야 한다.  
그래서 length property를 추가했더니 잘 된다. 
  
그런데 이게 다 argument가 배열이 아닌데 배열처럼 쓰려고 하는 짓이 아닌가.    
그래서 es6에서는 이를 개선한 rest parameter가 등장했다.  
  
```javascript
function argTest(...es6args) {
  Array.prototype.slice.apply(arguments); // 4
  console.log(es6args); // 4
}
argTest(4);
```
이렇게 하면 Array.prototype.slice.apply(arguments);와 동일한 결과를 보여준다.  
앞으로는 이걸 쓰도록 권고하고 있으니 알아두자. 

























