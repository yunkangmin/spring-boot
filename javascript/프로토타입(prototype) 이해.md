# 정리  
- Prototype  
함수(생성자)만 갖고 있는 프로퍼티   

- 동일한 생성자 함수로 생성된 객체들은 내부적으로 `_proto_` 프로퍼  
티를 사용하여 생성자 함수에 존재하는 Prototype이라는 프로퍼티를   
참조하여 같은 공간을 공유하고 있다.  

- 함수(생성자)의 프로토타입 속성
- 프로토타입 객체(객체의 원형, 함수를 정의하면 생성됨)  
   - 같은 원형으로 생성된 객체가 공통으로 참조하는 공간  
   - 함수의 prototype 속성을 통해 접근가능

- 프로토타입   
1. 생성된 객체 안에 _proto_ 속성이 자신을 만들어낸 원형을 의미하는   
프로토타입 객체를 참조하는 숨겨진 링크인데 이 숨겨진 링크를  
프로토타입이라고 정의한다.
2. 함수의 프로토타입

- Javascript에서는 숨겨진 링크가 있어 프로토타입 객체 멤버에 접근할  
수 있다. 그래서 이 프로토타입 링크를 사용자가 정의한 객체에 링크가   
참조되도록 설정하면 코드의 재사용과 객체 지향적인 프로그래밍을   
할 수 있다.  

---

JavaScript는 클래스라는 개념이 없다.  
그래서 기존의 객체를 복사하여(cloning) 새로운 객체를 생성하는 프로토타입 기반의 언어이다.  
프로토타입 기반 언어는 객체 원형인 프로토 타입을 이용하여 새로운 객체를 만들어낸다.  
이렇게 생성된 객체 역시 또 다른 객체의 원형이 될 수 있다. 프로토타입은 객체를 확장하고 객체  
지향적인 프로그래밍을 할 수 있게 해준다. 프로토타입은 크게 두가지로 해석된다.  
프로토타입 객체를 참조하는 prototype 속성과 객체 멤버인 proto 속성이 참조하는 숨은 링크가 있다.  
이 둘의 차이점을 이해하기 위해서는 JavaScript 함수와 객체의 내부적인 구조를 이해해야 한다.  
이번 글에서는 JavaScript의 함수와 객체 내부 구조부터 시작하여 프로토타입에 대해 알아보자.  
  
## 1. 함수와 객체의 내부 구조
JavaScript에서는 함수를 정의하고, 파싱단계에 들어가면, 내부적으로 수행되는 작업이 있다.  
함수 멤버로 prototype 속성이 있다. 이 속성은 다른 곳에 생성된 함수이름의 프로토타입 객체를  
참조한다. 프로토타입 객체의 멤버인 constructor 속성은 함수를 참조하는 내부구조를 가진다.  
아래 그림1과 같이 표현한다.  
![image](https://user-images.githubusercontent.com/33191974/145800416-b1fac8d0-5b2e-4fb5-bc1c-e15d742b2c3c.png)  
```javascript
function Person() {}
```
속성이 하나도 없는 Person이라는 함수가 정의되고, 파싱단계에 들어가면, Person 함수 Prototype 속성은  
프로토타입 객체를 참조한다. 프로토타입 객체 멤버인 constructor 속성은 Person 함수를 참조하는 구조를  
가진다. 여기서 알아야 하는 부분은 **Person 함수의 prototype 속성이 참조하는 프로토타입 객체는 new라는  
연산자와 Person 함수를 통해 생성된 모든 객체의 원형이 되는 객체이다.** 
생성된 모든 객체가 참조한다는 것을 기억해야 한다.  
![image](https://user-images.githubusercontent.com/33191974/145800984-6bb39556-6c88-4c30-84da-30747a4c003a.png)    
```javascript
function Person() {}

var joon = new Person();
var jisoo = new Person();
```
JavaScript에서는 기본 데이터 타입인 boolean, number, string, 그리고 특별한 값인 null, undefined  
빼고는 모두 객체이다. 사용자가 정의한 함수도 객체이고, new라는 연산자를 통해 생성된 것도 모두  
객체이다. 객체 안에는 proto(비표준) 속성이 있다. 이 속성은 객체가 만들어지기 위해 사용된 원형인  
프로토타입 객체를 숨은 링크로 참조하는 역할을 한다.  

## 2. 프로토타입 객체란?
함수를 정의하면 다른 곳에 생성되는 프로토타입 객체는 자신이 다른 객체의 원형이 되는 객체이다.  
모든 객체는 프로토타입 객체에 접근할 수 있다. 프로토타입 객체도 동적으로 런타임에 멤버를 추가할  
수 있다. 같은 원형을 복사로 생성된 모든 객체는 추가된 멤버를 사용할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/145802270-7017b47e-d85e-46ce-89d9-9df78468df74.png)  
```javascript
function Person(){}

var joon = new Person();
var jisoo = new Person();

Person.prototype.getType = function () {
  return "인간";
};

console.log(joon.getType()); //인간
console.log(jisoo.getType()); //인간
```

위 소스에서 함수 안의 prototype 속성을 이용하여 멤버를 추가했다.  
**프로토타입 객체에 getType()이라는 함수를 추가하면 멤버를 추가하기 전에 생성된 객체에서도 추가된  
멤버를 사용할 수 있다. 같은 프로토타입을 이용하여 생성된 joon과 jisoo 객체는 getType()을 사용할 수  
있다.**
  
**여기서 알아두어야 할 것은 프로토타입 객체에 멤버를 추가, 수정, 삭제할 때는 함수 안의 prototype 속성을  
사용해야 한다. 하지만 프로토타입 멤버를 읽을 때는 함수 안의 prototype 속성 또는 객체 이름으로 접근한다.**
![image](https://user-images.githubusercontent.com/33191974/145803006-57d6e174-260a-4432-9ac4-6d5f5bc15c9b.png)    
```javascript
joon.getType = function () {
  return "사람";
}

console.log(joon.getType()); //사람
console.log(jisoo.getType()); //인간

jisoo.age = 25;

console.log(joon.age); //undefined
console.log(jisoo.age); //25
```
위 소스에서 joon 객체를 이용하여 getType() 리턴 값을 사람으로 수정했다.  
그리고 joon과 jisoo에서 각각 getType()을 호출하면 joon 객체를 이용하여 호출한 결과는 사람으로  
출력되고, jisoo로 호출한 결과는 인간이 출력된다. **생성된 객체를 이용하여 프로토타입 객체의 멤버를  
멤버를 수정하면 프로토타입 객체에 있는 멤버를 수정하는 것이 아니라 자신의 객체에 멤버를 추가하는  
것이다. joon 객체를 사용하여 getType()을 호출하면 프로토타입 객체의 getType()을 호출한 것이 아니다.  
joon 객체에 추가된 getType()을 호출한 것이다. 프로토타입 객체의 멤버를 수정할 경우는 멤버 추가와  
같이 함수의 prototype 속성을 이용하여 수정한다.**  
![image](https://user-images.githubusercontent.com/33191974/145806406-1cdae14d-4dbb-48b4-8e89-372341a6747e.png)  
```javascript
Person.prototype.getType = function () {
  return "사람";
};

console.log(jisoo.getType()); //사람
```
함수의 prototype 속성을 이용하여 getType() 리턴 값을 사람으로 수정한다.  
그리고 jisoo 객체를 이용하여 호출한 결과 사람이 나온다.  
  
결론을 내리면, 프로토타입 객체는 새로운 객체가 생성되기 위한 원형이 되는 객체이다.  
같은 원형으로 생성된 객체가 공통으로 참조하는 공간이다. 프로토타입 객체의 멤버를 읽는  
경우에는 객체 또는 함수의 prototype 속성을 통해 접근할 수 있다. 하지만 추가, 수정, 삭제는  
함수의 prototype 속성을 통해 접근해야 한다(물론 객체이름으로도 할 수는 있지만 해당 객체에서만  
유효하다).

## 3. 프로토타입이란?
**JavaScript에서 기본 데이터 타입을 제외한 모든 것이 객체이다.**  
객체가 만들어지기 위해서는 자신을 만드는 데 사용된 원형인 프로토타입 객체를 이용하여  
객체를 만든다. 이 때 만들어진 객체 안에 _proto_(비표준) 속성이 자신을 만들어낸 원형을  
의미하는 프로토타입 객체를 참조하는 숨겨진 링크가 있다.  
이 숨겨진 링크를 프로토타입이라고 정의한다.  
![image](https://user-images.githubusercontent.com/33191974/145808063-1d330869-77c8-4497-98d5-6f08209cbaf7.png)  
```javascript
function Person() {}

var joon = new Person();
```
**위 그림 joon 객체의 멤버인 _proto_(비표준) 속성이 프로토타입 객체를 가리키는 숨은 링크가 프로토타입  
이라고 한다. 프로토타입을 크게 두 가지로 해석된다 했다. 함수의 멤버인 prototype 속성은 프로토타입  
객체를 참조하는 속성이다. 그리고 함수와 new 연산자가 만나 생성한 객체의 프로토타입 객체를 지정해주는  
역할을 한다(_proto_). 객체 안의 _proto_(비표준) 속성은 자신을 만들어낸 원형인 프로토타입 객체를  
참조하는 숨겨진 링크로서 프로토타입을 의미한다.**  
  
JavaScript에서는 숨겨진 링크가 있어 프로토타입 객체 멤버에 접근할 수 있다. 그래서 이 프로토타입 링크를  
사용자가 정의한 객체에 링크가 참조되도록 설정하면 코드의 재사용과 객체 지향적인 프로그래밍을 할 수 있다.  

## 4. 코드의 재사용
코드의 재사용하면 떠오르는 단어는 바로 상속이다. 클래스라는 개념이 있는 Java에서는 중복된 코드를 상속받아  
코드 재활용을 할 수 있다. 하지만 JavaScript에서는 클래스가 없는, 프로토타입 기반 언어이다.  
그래서 프로토타입을 이용하여 코드 재사용을 할 수 있다.  
  
이 방법에도 크게 두 가지로 분류할 수 있다. classical 방식과 prototypal 방식이있다.  
classical 방식은 new 연산자를 통해 생성한 객체를 사용하여 코드를 재사용하는 방법이다.  
마치 Java에서 객체를 생성하는 방법과 유사하여 classical 방식이라고 한다. prototypal 방식은  
리터럴 또는 Object.create()를 이용하여 객체를 생성하고 확장해 가는 방식이다. 두 가지 방법 중  
JavaScript에서는 prototypal 방식을 선호한다. 그 이유는 classical 방식보다 간결하게 구현할 수  
있기 때문이다. 밑의 예제1 ~ 4번까지는 classical 방식의 코드 재사용 방법이고, 5번은 prototypal 방식인  
Object.creat()를 사용하여 코드의 재사용을 보여준다.  
  
### (1) 기본 방법  
부모에 해당하는 함수를 이용하여 객체를 생성한다.  
자식에 해당하는 함수의 prototype 속성을 부모 함수를 이용하여 생성한 객체를 참조하는 방법이다.  
![image](https://user-images.githubusercontent.com/33191974/145815067-6da85a80-0c51-4186-bb43-04aca2cbfaa1.png)    
```javascript
function Person(name) {
  this.name = name || "혁준";
}

Person.prototype.getName = function () {
  return this.name;
}

function Korean(name) {}
Korean.prototype = new Person();

var kor1 = new Korean();
console.log(kor1.getName()); //혁준

var kor2 = new Korean("지수");
console.log(kor2.getName()); //혁준
```
위 소스를 보면 부모에 해당하는 함수는 Person이다.  
Korean.prototype = new Person();에서 자식 함수인 Korean 함수 안의 prototype 속성을 부모함수로  
생성된 객체로 바꿨습니다. 이제 Korean 함수와 new 연산자를 이용하여 생성된 kor 객체의 _proto_ 속성이  
부모 함수를 이용하여 생성된 객체를 참조한다. 이 객체가 Korean 함수를 이용하여 생성된 모든 객체의  
프로토타입 객체가 된다. kor1에는 name과 getName()이라는 속성이 없지만, 부모에 해당하는 프로토타입  
객체에 name이 있다. 이 프로토타입 객체의 부모에 getName()을 가지고 있어 kor1에서 사용할 수 있다.   
이 방법에도 단점이 있다. 부모 객체의 속성과 부모 객체의 프로토타입 속성을 모두 물려받게 된다.  
대부분의 경우 객체 자신의 속성은 특정 인스턴스에 한정되어 재사용할 수 없어 필요가 없다.   
또한 자식 객체를 생성할 때 인자를 넘겨도 부모 객체를 생성할 때 인자를 넘겨주지 못한다.  
kor2 객체를 생성할 때 Korean 함수의 인자로 지수라고 주었다. 객체를 생성한 후 getName()을 호출하면  
지수라고 출력될 것 같지만, 부모 생성자에 인자를 넘겨주지 않았기 때문에 name에는 default 값인 혁준이  
들어있다. 객체를 생성할 때마다 부모의 함수를 호출할 수도 있다. 하지만 매우 비효율적이다.  
그래서 다음 방법은 이 방법의 문제점을 해결하는 방법을 알아본다.  
  
### (2) 생성자 빌려 쓰기
이 방법은 기본 방법의 문제점인 자식 함수에서 받은 인자를 부모 함수로 인자를 전달하지 못했던 부분을  
해결한다. 부모 함수의 this에 자식 객체를 바인딩하는 방식이다.  
![image](https://user-images.githubusercontent.com/33191974/145817729-687cae37-fffe-428a-a647-7e57833bd063.png)  
```javascript
function Person(name) {
  this.name = name || "혁준";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  Person.apply(this, arguments);
}

var kor1 = new Korean("지수");
console.log(kor1.name); //지수
```
위 소스에서 Person.apply(this, arguments);를 보면 Korean 함수 내부에서 apply 함수를 이용한다.  
부모객체인 Person 함수 영역의 this를 Korean 함수 안의 this로 바인딩한다.  
이 것은 부모의 속성을 자식 함수 안에 모두 복사한다. 그 다음 코드를 보면 객체를 생성하고, 
name을 출력한다. 객체를 생성할 때 넘겨준 인자를 출력하는 것을 볼 수 있다. 기본 방법에서는  
부모객체의 멤버를 참조를 통해 물려받았다. 하지만 생성자 빌려 쓰기는 부모객체 멤버를 복사하여  
자신의 것으로 만들어 버린다는 차이점이 있다. 하지만 이 방법은 부모객체의 this로 된 멤버들만 물려받게  
되는 단점이 있다. 그래서 부모객체의 프로토타입 객체의 멤버들을 물려받지 못한다. 위 그림을 보면  
kor1 객체에서 부모객체의 프로토타입 객체에 대한 링크가 없다는 것을 볼 수 있다.  

### (3) 생성자 빌려 쓰고 프로토타입 지정해주기
이 방법은 방법 1과 방법 2 문제점들을 보완하면서 Java에서 예상할 수 있는 동작 방식과 유사하다.  
![image](https://user-images.githubusercontent.com/33191974/145820132-77b05ede-c2a4-40d5-9d68-a09046de32f6.png)    
```javascript
function Person(name) {
  this.name = name || "혁준";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  Person.apply(this, arguments);
}

Korean.prototype = new Person();

var kor1 = new Korean("지수");
console.log(kor1.getName()); //지수
```
위 소스에서 Person.apply(this, arguments);에서 부모 함수 this를 자식 함수 this로 바인딩한다.  
Korean.prototype = new Person();에서 자식 함수 prototype 속성을 부모 함수를 사용하여 생성된 객체로  
지정했다. 부모객체 속성에 대한 참조를 가지는 것이 아닌 복사본을 통해 내것으로 만든다. 동시에 부모객체의  
프로토타입 객체에 대한 링크도 참조된다. 부모객체의 프로토타입 객체 멤버도 사용할 수 있다.  
기본방법에 나온 그림과 비교했을 때 kor1 객체에 name 멤버가 없는 반면 위 그림에서는 name 멤버를 가지고  
있는 것을 확인할 수 있다. 생성자 빌려 쓰기에 나온 그림과 비교했을 때는 프로토타입 링크가 부모 함수로  
생성한 객체에 대해 참조도 하고 있다. 그리고 부모 객체의 프로토타입 객체도 링크로 연결된 것을 볼 수 있다.  
이 방법에도 문제점이 있다. 부모 생성자를 두 번 호출한다. 생성자 빌려 쓰기 방법과 달리 getName()은  
제대로 상속되었지만, name에 대해서는 kor1 객체와 부모 함수를 이용하여 생성한 객체에도 있는 것을  
볼 수 있다. 

### (4) 프로토타입 공유
이번 방법은 부모 생성자를 한 번도 호출하지 않으면서 프로토타입 객체를 공유하는 방법이다.  
![image](https://user-images.githubusercontent.com/33191974/145824809-237a8f53-9436-4d7a-a8d3-c2719256160d.png)  
```javascript
Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  this.name = name;
}
Korean.prototype = Person.prototype;

var kor1 = new Korean("지수");
console.log(kor1.getName()); //지수
```
위 소스 Korean.prototype = Person.prototype에서 자식 함수의 prototype 속성을 부모 함수의 prototype  
속성이 참조하는 객체로 설정했다. 자식 함수를 통해 생성된 객체는 부모 함수를 통해 생성된 객체를 거치지  
않고 부모 함수의 프로토타입 객체를 부모로 지정하여 객체를 생성한다. 부모 함수의 내용을 상속받지 못하므로  
상속받으려는 부분을 부모 함수의 프로토타입 객체에 작성해야 사용자가 원하는 결과를 얻게 된다.  
생성자 빌려 쓰고 프로토타입 지정해주기에서 나온 그림과 비교했을 때 중간에 부모 함수로 생성한 객체가  
없고 부모 함수의 프로토타입 객체로 링크가 참조되는 것을 볼 수 있다.  

### (5) prototypal한 방식의 재사용
이 방법은 Object.create()를 사용하여 객체를 생성과 동시에 프로토타입 객체를 지정한다.  
이 함수는 첫번째 매개변수는 부모객체로 사용할 객체를 넘겨주고, 두 번째 매개변수는 선택적 매개변수로써  
반환되는 자식 객체의 속성에 추가되는 부분이다. 이 함수를 사용함으로써 객체 생성과 동시에 부모객체를  
지정하여 코드의 재활용을 간단하게 구현할 수 있다. 
```javascript
var person = {
  type : "인간",
  getType : function() {
    return this.type;
  },
  getName : function() {
    return this.name;
  }
};

var joon = Object.create(person);
joon.name = "혁준";

console.log(joon.getType()); //인간
console.log(joon.getName()); //혁준
```
위 소스에서 부모 객체에 해당하는 person을 객체 리터럴 방식으로 생성했다.  
그리고 var joon = Object.create(person);에서 자식 객체 joon은 Object.create() 함수를  
이용하여 첫 번째 매개변수로 person을 넘겨받아 joon 객체를 생성했다.  
한 줄로 객체를 생성함과 동시에 부모객체의 속성도 모두 물려받았다. 위의 1 ~ 4번에 해당하는  
classical 방식보다 간단하면서 여러 가지 상황을 생각할 필요도 없다.  
JavaScript에서는 new 연산자와 함수를 통해 생성한 객체를 사용하는 classical 방식보다  
prototypal 방식을 더 선호한다. 

## 마치며
지금까지 JavaScript 프로토타입에 대해 정리했다. 처음 프로토타입을 공부할 땐, 자바 OOP 관점에서  
접근하여 혼란스러웠다. 하지만 함수의 내부구조부터 차근차근 접근하였더니 쉽게 이해할 수 있었다.  
그리고 코드의 재사용 방식에 대해 공부하였던 것도 JavaScript 언어 자체를 이해하는데 많은 도움이  
되었다.

---

# 정리
Javascript는 클래스라는 개념이 없다.  
그래서 기존의 객체를 복사하여(cloning) 새로운 객체를 생성하는 프로토타입  
기반의 언어이다.  
프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를   
만들어낸다.   
이렇게 생성된 객체 역시 또 다른 객체의 원형이 될 수 있다. 프로토타입은  
객체를 확장하고 객체지향적인 프로그래밍을 할 수 있게 해준다. 프로토타입은   
크게 두 가지로 해석된다.   
프로토타입 객체를 참조하는 prototype 속성과 객체 멤버인 proto 속성이 참조  
하는 숨은 링크가 있다.   
이 둘의 차이점을 이해하기 위해서는 Javascript 함수와 객체의 내부적인  
구조를 이해해야 한다.   
이번 글에서는 Javascript의 함수와 객체 내부 구조부터 시작하여 프로토타입에  
대해 알아보자.   

# 1. 함수와 객체의 내부 구조  
Javascript에서는 함수(생성자포함)를 정의하고, 파싱단계에 들어가면,   
내부적으로 수행되는 작업이 있다.     
함수멤버로 prototype 속성이 있다. 이 속성은 다른 곳에서 생성된 `함수이름의   
프로토타입 객체`를 참조한다. 프로토타입 객체의 멤버인 constructor 속성은   
함수를 참조하는 내부구조를 가진다.  
아래 그림1과 같이 표현한다.   
```javascript
function Person() {}
```
속성이 하나도 없는 Person이라는 함수(생성자 포함)가 정의되고, 파싱 단계에  
들어가면, Person 함수 Prototype 속성은 프로토타입 객체를 참조한다.   
프로토타입 객체멤버인 constructor 속성은 Person 함수를 참조하는 구조를   
가진다. **여기서 알아야 하는 부분은 Person 함수의 prototype 속성이 참조하는  
프로토타입 객체는 new라는 연산자와 Person 함수를 통해 생성된 모든 객체의  
원형이 되는 객체이다. 생성된 모든 객체가 참조한다는 것을 기억해야 한다.** <img src="https://user-images.githubusercontent.com/33191974/163667734-c872aa08-2fa9-4040-ad91-53a967e60bd1.png" width="50%" height="50%"/>    

```javascript
function Person() {}

var joon = new Person();
var jisoo = new Person();
```
Javascript에서는 기본 데이터 타입인 boolean, number, string, 그리고 특별  
한 값인 null, undefined 빼고는 모두 객체이다. 사용자가 정의한 함수도 객체  
이고, new라는 연산자를 통해 생성된 것도 모두 객체이다. 객체 안에는 proto(  
비표준) 속성이 있다. 이 속성은 객체가 만들어지기 위해 사용된 원형인 프로토  
타입 객체를 숨은 링크로 참조하는 역할을 한다.  

# 2. 프로토타입 객체란?   
**함수를 정의하면 다른 곳에 생성되는 프로토타입 객체는 자신의 다른 객체의  
원형이 되는 객체이다.** 모든 객체는 프로토타입 객체에 접근할 수 있다.  
프로토타입 객체도 동적으로 런타임에 멤버를 추가할 수 있다. 같은 원형을   
복사로 생성된 모든 객체는 추가된 멤버를 사용할 수 있다.  
<img src="https://user-images.githubusercontent.com/33191974/163667931-c8fa198b-c102-48aa-8e1f-a3dab659ba88.png" width="50%" height="50%"/>  
```javascript
function Person() {}

var joon = new Person();
var jisoo = new Person();

Person.prototype.getType = function () {
  return "인간";
};

console.log(joon.getType()); //인간
console.log(jisoo.getType()); //인간
```
위 소스에서 함수 안의 prototype 속성을 이용하여 멤버를 추가했다.   
프로토타입 객체에 getType()이라는 함수를 추가하면 멤버를 추가하기 전에  
생성된 객체에서도 추가된 멤버를 사용할 수 있다. 같은 프로토타입을 이용하여   
생성된 joon과 jisoo 객체는 getType()을 사용할 수 있다.  
  
**여기서 알아두어야 할 것은 프로토타입 객체에 멤버를 추가, 수정, 삭제할 때는  
함수 안의 prototype 속성을 사용해야 한다.** 하지만 프로토타입 멤버를 읽을  
때는 함수 안의 prototype 속성 또는 객체 이름으로 접근한다.   
<img src="https://user-images.githubusercontent.com/33191974/163668159-5c6f3e04-20c3-4f76-9e3d-dc825baaa775.png" width="50%" height="50%"/>   
```javascript
joon.getType = function () {
  return "사람";
}

console.log(joon.getType()); //사람
console.log(jisoo.getType()); //인간

jisoo.age = 25;

console.log(joon.age); //undefined
console.log(jisoo.age); //25
```
위 소스에서 joon 객체를 이용하여 getType() 리턴값을 사람으로 수정했다.  
그리고 joon과 jisoo에서 각각 getType()을 호출하면 joon 객체를 이용하여   
호출한 결과는 사람으로 출력되고, jisoo로 호출한 결과는 인간이 출력된다.  
생성된 객체를 이용하여 프로토타입 객체의 멤버를 수정하면 자신의 객체에   
멤버를 추가하는 것이다. joon 객체를 사용하여 getType()을 호출하면 프로토  
타입 객체의 getType()을 호출한 것이 아니다.   
joon 객체에 추가된 getType()을 호출한 것이다. 프로토타입 객체의 멤버를   
수정할 경우는 멤버 추가와 같이 함수의 prototype 속성을 이용하여 수정한다.   
<img src="https://user-images.githubusercontent.com/33191974/163668283-a0963d88-f77a-48b5-8115-04c696a96d06.png" width="50%" height="50%"/>  
```javascript
Person.prototype.getType = function () {
  return "사람";
}

console.log(jisoo.getType()); //사람   
```
함수의 prototype 속성을 이용하여 getType() 리턴값을 사람으로 수정한다.   
그리고 jisoo 객체를 이용하여 호출한 결과 사람이 나온다.   
  
결론을 내리면, 프로토타입 객체는 새로운 객체가 생성되기 위한 원형이 되는   
객체이다. 같은 원형으로 생성된 객체가 공통으로 참조하는 공간이다. 프로토   
타입 객체의 멤버를 읽는 경우에는 객체 또는 함수의 prototype 속성을 통해  
접근할 수 있다. 하지만 추가, 수정, 삭제는 함수의 prototype 속성을 통해   
접근해야 한다(물론 객체이름으로도 할 수는 있지만 해당 객체에서만 유효  
하다).    

# 3. 프로토타입이란?   
Javascript에서 기본 데이터 타입을 제외한 모든 것이 객체이다.   
객체가 만들어지기 위해서는 자신을 만드는데 사용된 원형인 프로토타입 객체를   
이용하여 객체를 만든다. 이 때 만들어진 객체 안에 proto(비표준) 속성이 자신  
을 만들어낸 원형을 의미하는 프로토타입 객체를 참조하는 숨겨진 링크가 있다.  
이 숨겨진 링크를 프로토타입이라고 정의한다.  
<img src="https://user-images.githubusercontent.com/33191974/163668556-f3f7e3d4-dcb0-4860-974d-49a6ac72d258.png" width="50%" height="50%"/>  
```javascript
function Person() {}
var joon = new Person();
```
위 그림 joon 객체의 멤버인 proto(비표준) 속성이 프로토타입 객체를 가리키는   
숨은 링크가 프로토타입이라고 한다. 프로토타입은 크게 두가지로 해석된다.  
함수의 멤버인 prototype 속성은 프로토타입 객체를 참조하는 속성이다.  
그리고 함수와 new 연산자가 만나 생성한 객체의 프로토타입 객체를 지정해주는  
역할을 한다(proto). 객체 안의 proto(비표준) 속성은 자신을 만들어낸 원형인   
프로토타입 객체를 참조하는 숨겨진 링크로서 프로토타입을 의미한다.   
  
Javascript에서는 숨겨진 링크가 있어 프로토타입 객체 멤버에 접근할 수 있다.   
그래서 이 프로토타입 링크를 사용자가 정의한 객체에 링크가 참조되도록 설정  
하면 코드의 재사용과 객체 지향적인 프로그래밍을 할 수 있다.  
  
# 4. 코드의 재사용  
코드의 재사용하면 떠오르는 단어는 바로 상속이다. 클래스라는 개념이 있는   
Java에서는 중복된 코드를 상속받아 코드 재활용을 할 수 있다. 하지만 Java  
script에서는 클래스가 없는, 프로토타입 기반 언어이다.   
그래서 프로토타입을 이용하여 코드 재사용을 할 수 있다.   
  
이 방법에도 크게 두 가지로 분류할 수 있다. classical 방식과 prototypal  
방식이 있다. classical 방식은 new 연산자를 통해 생성한 객체를 사용하여   
코드를 재사용하는 방법이다. 마치 Java에서 객체를 생성하는 방법과 유사하여   
classical 방식이라고 한다. prototypal 방식은 리터럴 또는 Object.create()   
를 이용하여 객체를 생성하고 확장해 가는 방식이다. 두 가지 방법 중 Java  
Script에서는 prototypal 방식을 선호한다. 그 이유는 claasical 방식보다   
간결하게 구현할 수 있기 때문이다. 밑의 예제 1 ~ 4번까지는 classical 방식의  
코드 재사용 방법이고 5번은 prototypal 방식인 Object.create()를 사용하여   
코드의 재사용을 보여준다.  

## (1) 기본 방법
부모에 해당하는 함수를 이용하여 객체를 생성한다.   
자식에 해당하는 함수의 prototype 속성을 부모 함수를 이용하여 생성한 객체를   
참조하는 방법이다.   
<img src="https://user-images.githubusercontent.com/33191974/163669080-fe5d375d-ded2-4ce9-85f1-32cc6eb66dd8.png" width="50%" height="50%"/>   
```javascript
function Person(name) {
  this.name = name || "혁준";
}

Person.prototype.getName = function () {
  return this.name;
}

function Korean(name) {}
Korean.prototype = new Person();

var kor1 = new Korean();
console.log(kor1.getName()); //혁준

var kor2 = new Korean("지수");
console.log(kor2.getName()); //혁준   
```
위 소스르 보면 부모에 해당하는 함수는 Person이다.   
Korean.prototype = new Person();에서 자식 함수인 Korean 함수 안의 proto  
type 속성을 부모함수로 생성된 객체로 바꿨다. 이제 Korean 함수와 new 연산  
자를 이용하여 생성된 kor 객체의 proto 속성이 부모함수를 이용하여 생성된  
객체를 참조한다. 이 객체가 Korean 함수를 이용하여 생성된 모든 객체의   
프로토타입 객체가 된다. kor1에는 name과 getName()이라는 속성이 없지만,  
부모에 해당하는 프로토타입 객체에 name이 있다. 이 프로토타입 객체의 부모에   
getName()을 가지고 있어 kor1에서 사용할 수 있다.   
이 방법에도 단점이 있다. 부모 객체의 속성과 부모 객체의 프로토타입 속성을   
모두 물려받게 된다. 대부분의 경우 객체 자신의 속성은 특정 인스턴스에 한정  
되어 재사용할 수 없어 필요가 없다. 또한 자식 객체를 생성할 때 인자를   
넘겨도 부모 객체를 생성할 때 인자를 넘겨주지 못한다. kor2 객체를 생성할   
때 Korean 함수의 인자로 지수라고 주었다. 객체를 생성한 후 getName()을  
호출하면 지수라고 출력될 것 같지만, 부모 생성자에 인자를 넘겨주지 않았기  
때문에 name에는 default 값인 혁준이 들어있다. 객체를 생성할 때마다 부모의   
함수를 호출할 수도 있다. 하지만 매우 비효율적이다. 그래서 다음 방법은   
이 방법의 문제점을 해결하는 방법을 알아본다.   
  
## (2) 생성자 빌려쓰기   
이 방법은 기본 방법의 문제점인 자식 함수에서 받은 인자를 부모 함수로 인자  
를 전달하지 못했던 부분을 해결한다. 부모 함수의 this에 자식 객체를 바인딩  
하는 방식이다.  
<img src="https://user-images.githubusercontent.com/33191974/163669488-874a4ace-51de-413a-9f24-17d11195f2e1.png" width="50%" height="50%"/>  
```javascript
function Person(name) {
  this.name = name || "혁준";
}

Person.prototype.getName = function () {
  return this.name;
};

function Korean(name) {
  Person.apply(this, arguments);
}

var kor1 = new Korean("지수");
console.log(kor1.name); //지수
```
위 소스에서 Person.apply(this, arguments);를 보면 Korean 함수 내부에서  
apply 함수를 이용한다. 부모객체인 Person 함수 영역의 this를 Korean 함수  
안의 this로 바인딩한다. 이 것은 부모의 속성을 자식 함수 안에 모두 복사  
한다. 그 다음 코드를 보면 객체를 생성하고, name을 출력한다. 객체를 생성할  
때 넘겨준 인자를 출력하는 것을 볼 수 있다. 기본 방법에서는 부모객체의   
멤버를 참조를 통해 물려받았다. 하지만 생성자 빌려쓰기는 부모객체 멤버를   
복사하여 자신의 것으로 만들어버린다는 차이점이 있다. 하지만 이 방법은   
부모 객체의 this로 된 멤버들만 물려받게 되는 단점이 있다. 그래서 부모객  
체의 프로토타입 객체의 멤버들을 물려받지 못한다. 위 그림을 보면 kor1 객체  
에서 부모객체의 프로토타입 객체에 대한 링크가 없다는 것을 볼 수 있다.   

## (3) 생성자 빌려쓰고 프로토타입 지정해주기   
이 방법은 방법 1과 방법 2 문제점들을 보완하면서 Java에서 예상할 수 있는  
동작 방식과 유사하다.    
<img src="https://user-images.githubusercontent.com/33191974/163669893-f3e4b7b9-0f59-4c21-9595-55ba585b52e7.png" width="50%" height="50%"/>  
```javascript
function Person(name) {
  this.name = name || "혁준";
}

Person.prototype.getName = function () {
  return this.name;
}

function Korean(name) {
  Person.apply(this, arguments);
}

Korean.prototype = new Person();

var kor1 = new Korean("지수");
console.log(kor1.getName()); //지수
```
위 소스에서 Person.apply(this, arguments);에서 부모 함수 this를 자식함수  
this로 바인딩한다. Korean.prototype = new Person();에서 자식함수 proto  
type 속성을 부모 함수를 사용하여 생성된 객체로 지정했다. 부모객체 속성에   
대한 참조를 가지는 것이 아닌 복사본을 통해 내것으로 만든다. 동시에 부모객  
체의 프로토타입 객체에 대한 링크도 참조된다. 부모객체의 프로토타입 객체  
멤버도 사용할 수 있다. 기본방법에 나온 그림과 비교했을 때 kor1 객체에   
name 멤버가 없는 반면 위 그림에서는 name 멤버를 가지고 있는 것을 확인할   
수 있다. 생성자 빌려 쓰기에 나온 그림과 비교했을 때는 프로토타입 링크가   
부모함수로 생성한 객체에 대해 참조도 하고 있다. 그리고 부모 객체의 프로토  
타입 객체도 링크로 연결된 것을 볼 수 있다. 이 방법에도 문제점이 있다.   
부모 생성자를 두 번 호출한다. 생성자 빌려쓰기 방법과 달리 getName()은  
제대로 상속되었지만, name에 대해서는 kor1 객체와 부모 함수를 이용하여   
생성한 객체에도 있는 것을 볼 수 있다.  

## (4) 프로토타입 공유  
이번 방법은 부모 생성자를 한 번도 호출하지 않으면서 프로토타입 객체를  
공유하는 방법이다.  
<img src="https://user-images.githubusercontent.com/33191974/163670394-f5c1cb00-ab57-46e9-b483-bfdf8e4484c0.png" width="50%" height="50%"/>     
```javascript
Person.prototype.getName = function () {
  return this.name;
}

function Korean(name) {
  this.name = name;
}
Korean.prototype= Person.prototype;

var kor1 = new Korean("지수");
console.log(kor1.getName()); //지수
```
위 소스 Korean.prototype = Person.prototype에서 자식 함수의 prototype  
속성을 부모 함수의 prototype 속성이 참조하는 객체로 설정했다. 자식 함수를  
통해 생성된 객체는 부모 함수를 통해 생성된 객체를 거치지 않고 부모 함수의   
프로토타입 객체를 부모로 지정하여 객체를 생성한다. 부모 함수의 내용을   
상속받지 못하므로 상속받으려는 부분을 부모함수의 프로토타입 객체에 작성  
해야 사용자가 원하는 결과를 얻게 된다. 생성자 빌려쓰고 프로토타입 지정  
해주기에서 나온 그림과 비교했을 때 중간에 부모 함수로 생성한 객체가 없고   
부모 함수의 프로토타입 객체로 링크가 참조되는 것을 볼 수 있다.   

## (5) prototypal한 방식의 재사용   
이 방법은 Object.create()를 사용하여 객체를 생성과 동시에 프로토타입 객체  
를 지정한다. 이 함수는 첫번째 매개변수는 부모객체롤 사용할 객체를 넘겨  
주고, 두번째 매개변수는 선택적 매개변수로써 반환되는 자식 객체의 속성에   
추가되는 부분이다. 이 함수를 사용함으로써 객체 생성과 동시에 부모객체를  
지정하여 코드의 재활용을 간단하게 구현할 수 있다.   
```javascript
var person = {
  type : "인간",
  getType : function() {
    return this.type;
  }, 
  getName : function() {
    return this.name;
  }
};

var joon = Object.create(person);
joon.name = "혁준";

console.log(joon.getType()); //인간
console.log(joon.getName()); //혁준
```
위 소스에서 부모 객체에 해당하는 person을 객체리터럴 방식으로 생성했다.   
그리고 var joon = Object.create(person);에서 자식 객체 joon은 Object.  
create() 함수를 이용하여 첫 번째 매개변수로 person을 넘겨받아 joon 객체를  
생성했다. 한 줄로 객체를 생성함과 동시에 부모 객체의 속성도 모두 물려받았  
다. 위의 1 ~ 4번에 해당하는 classical 방식보다 간단하면서 여러 가지 상황을  
생각할 필요도 없다. Javascript에서는 new 연산자와 함수를 통해 생성한 객  
체를 사용하는 classical 방식보다 prototypal 방식을 더 선호한다.   

---

# Object.create() 메소드를 이용한 객체의 생성  
Object.create() 메소드를 이용하여 객체를 생성할 수도 있다.   
Object.create() 메소드는 지정된 프로토타입(prototype) 객체와 프로퍼티를     
가지고 새로운 객체를 만들어준다.  
따라서 이 메소드를 이용하면 사용자가 프로토타입객체를 직접 명시할 수 있으  
므로, 상당히 유용하게 사용된다.  
```
Object.create(프로토타입객체, [새로운객체의 프로퍼티1, 2, 3...]);
```
Object.create() 메소드의 첫번째 인수로는 프로토타입으로 사용할 객체를   
전달한다. 두 번째 인수로는 새로운 객체의 추가할 프로퍼티 정보를 전달한다.   
```
//null 프로토타입을 사용하여 새로운 객체를 만들고
var obj = Object.create(null, { 
             //x좌표를 나타내는 열거할 수 있는 프로퍼티 추가
             x: { value: 100, enumerable: true },
             //y좌표를 나타내는 열거할 수 있는 프로퍼티 추가
             y: { value: 200, enumerable: true }
          });
obj.x;
obj.y;
//객체의 프로토타입을 반환해줌
Object.getPrototypeOf(obj);
```


  

  

















  






























































