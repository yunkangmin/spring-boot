실무에서 하드 코딩을 하다가 배열 안에서 중복되는 값들을 count 해야 하는 상황이 있다.  
찾아보니 reduce()라는 기능을 가진 녀석이 있었는데 글을 적으며 이해하기 위해 노력해본다.  
ex) arr = {"김동수", "김동수", "김영희", "김영희"}라는 녀석이 있는데 중복값을 count해야 한다.  
  
얼핏 정리한 글들을 봤는데 자바스크립트 내장 메서드 중에서 제일 강력하다는 글을 보았다.  
먼저 map()을 살펴보자.   

## map()이란?
map의 기본원리는 간단한데  
반복문을 돌며 배열 안의 요소들을 1:1로 짝지어 주는 것이다.  
어떻게 짝지어줄 것인가 정의한 함수를 메서드의 인자로 넣어주면 된다.  
```javascript
//map 활용
const arr = [1, 2, 3];

let result = arr.map((result) => {
  console.log(result);
  return result;
});

//console = 1, 2, 3
arr; //[1, 2, 3]
result; //[1, 2, 3]

arr === result //false
```
반복문으로 요소를 순회하며 각 요소를 어떻게 짝지어줄지 알려주는데   
여기서는 return 값으로 result를 반환하였으니 값은 그대로 짝짓는다.  
중요한 점은, map을 실행하는 배열과 결과로 나오는 배열이 다른 객체라는 것이다.   
  
map은 규칙적인 배열만 반환하는 것이 아니라, 함수 안에 적어준 대로 반환할 수 있기 때문에   
자유도가 높다.  
```
abc = arr.map((result) => {
  return result + 1;
});
result; //[2, 3, 4]
```
이런 식으로 함수에서 사용자가 임의로 정의를 하면 result 값이 바뀌는 것을 볼 수 있다.  
  
한 가지 예시를 더 살펴보자.  
```
abc = arr.map((result) => {
  if (result % 2)
    return '홀수';
    
  return '짝수';
});

result //['홀수', '짝수', '홀수']
```
이렇게 map()은 배열을 1:1로 짝을 짓지만 절대 기존 객체를 수정하지 않는 메서드이다.  
(return 해서 나온느 값과 기존 arr은 다른 객체이다.)

## reduce()란?
reduce는 "줄이다"라는 의미를 가지고 있다.  
배열을 순회하며 인덱스 데이터를 줄여가며 어떠한 기능을 수행할 수 있다.  
reduce()란 덧셈 함수가 아니다.  
reduce() 메서드의 기본 문법을 먼저 알아보자.  
```
배열.reduce((누적값, 현재값, 인덱스, 요소) => {
  return 결과
}, 초기값);
```
이런 식으로 사용된다.  
누적값이라는 것을 잘 알고 있어야 한다.  
  
reduce()는 기본적으로 다른 메서드와 동일하게 첫 번째 인자로 함수를 받는다.  
단, 두번째 인자로 초기값을 세팅할 수 있다는 것에서 차이가 있다.  
물론 두번째 인자는 생략이 가능하며 생략 시에는 첫번째 값이 그 값을 담당한다.  
예제를 보자.   
```
abc = arr.reduce((acc(누적값), cur(현재값), i) => {
  console.log(acc, cur, i);
  return acc + cur;
}, 0(초기값));

//결과
(acc)   (cur)    (i)
누적값   현재값   인덱스
0        1        0
1        2        1
3        3        2

abc; //6
```
acc(누적값)이 초기값인 0부터 시작하여 return 하는 대로 누적되는 것을 볼 수 있다.   
(여기서 초기값이 0으로 설정되었기 때문에 처음 누적값은 0이다.)  
또한 초기값을 적어주지 않으면 자동으로 초기값이 0번째 인덱스의 값이 된다.  
  
초기값을 적지 않은 경우  
```
var abc;
const arr = [1, 2, 3];
//초기값 제거
abc = arr.reduce((acc(누적값), cur(현재값), i) => {
  console.log(acc, cur, i);
  return acc + cur;
}, );

//결과
(acc)  (cur)   (i)
누적값 현재값  인덱스
1      2      1
3      3      2

abc; //6
```
reduce() 메서드는 덧셈뿐만 아니라 초기값을 활용하여 무궁무진한 방법을 활용할 수 있게 해준다.   
또한 map, filter와 같은 함수형 메서드를 모두 reduce로 구현할 수 있다.  
  
map을 구현한 reduce()  
```
abc = arr.reduce((acc, cur) => {
  acc.push(cur % 2 ? '홀수': '짝수');
  return acc;
}, []); //초기값을 배열로 만듦

result; //['홀수', '짝수', '홀수']
```
배열에 값들을 push하면 위에서 사용한 map과 같아진다. 여기서 조건부로 push를 하면 filter와 같아진다.  
  
홀수만 필터링하는 code(조건부) filter와 같은 기능  
```
abc = arr.reduce((acc, cur)) => {
  if (cur % 2) acc.push(cur);
  return acc;
}, []);

//결과
result; //[1, 3]
```

제가 사용하고 싶었던 함수는 바로 이것을 활요했다.(대충 설명하자면 count 세는 로직)  
```
const name = ['홍길동', '홍길동', '김영희', '김영희', '바둑이', '김영희', '바둑이'];
const result = name.reduce((object, currentValue) => {
  if (!object[currentValue]) object[currentValue] = 0;
  object[currentValue]++;
  return object;
},{});
console.log(result);
=> {홍길동 : 2, 김영희: 3, 바둑이 : 2}라는 값이 출력된다.  
```
    
[더보기](https://2ham-s.tistory.com/289)


































