## 1. 생성자 함수란?   
- "객체"를 생성할 때 사용하는 함수이다.   

## 2. 생성자 함수의 필요성?  
- 여러 개의 동일한 프로퍼티를 가지는 객체를 생성하기 위해서 필요하다.   
- Prototype을 이용하여 메모리 절감을 위해서도 필요하다.   

## 3. 생성자 함수의 형태   
- 생성자명 : 대문자로 시작 (일반 함수와 구별을 하기 위한 관례이다)   
- 내부의 식별자 선언(var 식별자)없음
- 내부의 메소드 선언(var 식별자 = function () {})없음   
- "this.프로퍼티"를 통해서 프로퍼티 명시만 가능   
```javascript
function <생성자명>() {
  this.프로퍼티
}
```

## 4. 생성자 함수의 사용법  
### 1. new 키워드를 통하여 생성자 함수를 호출한다.  
```javascript
var 생성자 = new 생성자명();
```
### 2. 생성자 함수 생성 과정 분석  
```javascript
var student = new Student("영수", 88, 92);
```
- student -> 객체 할당   
- new -> 빈 객체 생성
- Student -> 객체 Property정의   
- "영수", 88, 92 -> 객체 Property 내용 입력
```
//this는 생성된 객체 자체를 의미한다.  
//중괄호와 this -> new
//name, math, eng -> Student
//"영수", 88, 92 -> "영수", 88, 92
{
  this.name : "영수",
  this.math : 88,
  this.eng : 92
}
```

## 5. 생성자 함수에서 사용되는 함수 정의   
- 객체에는 프로퍼티를 활용하기 위한 함수를 정의할 수 있다.   
- 정의방법은 2가지가 있다.   
### 1. 생성자 내부에 함수 정의  
```javascript
function Student(_name, _math, _ent) {
  this.name = _name,
  this.math = _math,
  this.eng = _eng,
  this.score = function allinfo() {
    return "Student name :" + 
      this.name + "score math : " + this.math + "eng : " + this.eng;
  }
}
//출력결과 : Student name :영수score math: 93 eng : 11
```
### 2. Prototype을 사용한 함수 정의  
```javascript
function Student(_name, _math, _eng) {
  this.name = _name,
  this.math = _math,
  this.eng = _eng
}
Student.prototype.score = function allInfo() {
  return "Student name : " + this.name + 
  "score math : " + this.math + "eng : " + this.eng;
}
//출력결과 : Student name :영수score math: 93 eng : 11
```
## 6. Prototype이란?
### 1. 나온 배경  
일반적으로 객체를 만들어서 해당 객체를 복사하여 사용하게 되면, 객체에 들어   
있는 프로터피와 함수가 복사한 객체 개수만큼 생성된다. 그러나 이는 매우 비효  
율적이다. 왜냐하면 객체에 들어있는 함수는 모두 동일할테니 굳이 여러개를   
생성하여 메모리를 잡아 먹을 필요가 없다. 그래서 이를 해결하기 위해 나온  
것이 프로토타입(Prototype)이다.   

### 2. Prototype의 정의   
- 생성자 함수로 생성한 객체들이 프로퍼티와 메서드를 공유하기 위해 사용하는   
객체이다.  
- 함수만 갖고 있는 프로퍼티이다.   
자바스크립트는 모든 것이 객체임으로 함수도 프로퍼티를 가질 수 있다.  
- Prototype은 사용자가 만들어 주는 것이 아니고 javascript에서 자동으로   
만들어 준다.   

### 구조   
- 동일한 생성자 함수로 생성된 객체들은 내부적으로 `_proto_`프로퍼티를 사용  
하여 생성자 함수에 존재하는 Prototype이라는 프로퍼티를 참조하여 같은  
공간을 공유하고 있다.  
- 함수에는 Prototype이 프로퍼티로 존재한다.  
<img src="https://user-images.githubusercontent.com/33191974/163666124-88f02854-7283-4f51-88a5-64fb6f5cd98b.png" width="50%" height="50%"/>    
<img src="https://user-images.githubusercontent.com/33191974/163666165-8c500ba0-f254-407d-b1a8-fd4f938f7126.png" width="50%" height="50%"/>  

## 7. Prototype을 이용한 객체 상속   
- 생성자 함수의 Prototype에 객체를 생성함으로써 객체를 상속할 수 있다.  
### 1. 예제코드  
- 증조 할아버지의 눈  
- 할아버지의 키  
- 아버지의 잘생김을 나는 모두 상속받아 사용할 수 있다.   
```javascript
//증조 할아버지
function great_grandfather() {
  this.eyes = "파란눈";
}
//할아버지
function grandfather() {
  this.height = "장신";
}
grandfather.prototype = new great_grandfather();

//아빠  
function father() {
  this.face = "잘생김";
}
father.prototype = new grandfather();

//나
function me() {
  this.age = "18";
}
me.prototype = new father();

//내가 태어났다.  
var iam = new me();

//나는 증조 할아버지의 자식인가??
if (me.prototype.isPrototypeOf(great_grandfather)) {
  //나는 모든 것을 물려 받았다.   
  console.log(iam.eyes);
  console.log(iam.height);
  console.log(iam.face);
  console.log(iam.age);
}
console.dir(iam);
```
### 2. 객체의 상속관계   
<img src="https://user-images.githubusercontent.com/33191974/163666461-40f09f5d-33b3-4f33-9f81-1742ff6ac969.png" width="50%" height="50%"/>  























