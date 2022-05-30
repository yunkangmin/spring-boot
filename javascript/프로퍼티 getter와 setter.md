첫 번째 종류로는 데이터 프로퍼티(data property)이다. 지금까지 사용한 모든   
프로퍼티는 데이터 프로퍼티이다. 데이터 프로퍼티 조작 방법에 대해선 모두 알고  
있을 거라 생각한다.   

---

# 데이터 프로퍼티   
일반적으로 객체에 속해있는 프로퍼티를 데이터 프로퍼티라고 한다.   
```javascript
const obj = {
  id: 1, //데이터 프로퍼티
  name: 'Obj' //데이터 프로퍼티
```
obj 변수에 저장된 객체는 id와 name 프로퍼티를 가지고 두 프로퍼티는 모두    
데이터 프로퍼티다. 일반적으로 obj.id 혹은 obj.name과 같이 객체의 프로퍼티   
를 읽을 수 있다.  

# 접근자 프로퍼티
객체의 프로퍼티 종류 중 새로운 종류의 프로퍼티이다. 함수처럼 정의되며 호출  
은 데이터 프로퍼티에 접근하는 것처럼 호출할 수 있다.  
```javascript
const obj = {
  id: 1, //데이터 프로퍼티
  name: 'Obj', //데이터 프로퍼티  
  get idAndName() {
    return `${this.id}, ${this.name}`
  }, //접근자 프로퍼티
  set idAndName({ id, name }) {
    this.id = id,
    this.name = name
  }//접근자 프로퍼티
}

obj.idAndName //'1, Obj'
obj.idAndName = { id: 2, name: 'Obj2' }
obj.idAndName //'2, Obj2'
```
함수 앞에 get과 set 키워드가 붙으면 해당 함수는 접근자 프로퍼티로 동작  
한다. get은 객체의 어떠한 데이터를 읽어서 반환할 때 사용하며 set은 데이터  
를 쓸 때 사용한다.  

---
  
두 번째는 접근자 프로퍼티(accessor property)라 불리는 새로운 종류의 프로퍼티  
이다. 접근자 프로퍼티의 본질은 함수인데, 이 함수는 값을 획득(get)하고 설정(  
set)하는 역할을 담당한다. 그런데 외부 코드에서는 함수가 아닌 일반적인 프로퍼  
티처럼 보인다.  
  
# getter와 setter
접근자 프로퍼티는 'getter(획득자)'와 'setter(설정자)' 메서드로 표현된다. 객체   
리터럴 안에서 getter와 setter 메서드는 get과 set으로 나타낼 수 있다.   
```javascript
let obj = {
  get propName() {
    //getter, obj.propName을 실행할 때 실행되는 코드  
  },
  set propName(value) {
    //setter, obj.propName = value를 실행할 때 실행되는 코드  
  }
};
```
getter 메서드는 obj.propName을 사용해 프로퍼티를 읽으려고 할 때 실행되고,   
setter 메서드는 obj.propName = value으로 프로퍼티에 값을 할당하려 할 때   
실행된다.  
프로퍼티 name과 surname이 있는 객체 user를 만들어보자.  
```javascript
let user = {
  name: "John",
  surname: "Smith"
};
```
이 객체에 fullName이라는 프로퍼티를 추가해 fullName이 'John Smith'가 되도록  
해보자. 기존 값을 복사·붙여넣기 하지 않고 fullName이 John Simth가 되도록   
접근자 프로퍼티를 구현하면 된다.   
```javascript
let user = {
  name: "John",
  surname: "Smith",
  
  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

alert(user.fullname); //John Smith;
```
바깥 코드에선 접근자 프로퍼티를 일반 프로퍼티처럼 사용할 수 있다. 접근자  
프로퍼티는 이런 아이디어에서 출발했다. 접근자 프로퍼티를 사용하면 함수처럼   
호출하지 않고, 일반 프로퍼티에서 값에 접근하는 것처럼 평범하게 user.full  
Name을 사용해 프로퍼티 값을 얻을 수 있다. 나머지 작업은 getter 메서드가  
뒷단에서 처리해준다.   
  
한편, 위 예시의 fullName은 getter 메서드만 가지고 있기 때문에 user.full  
Name=을 사용해 값을 할당하려고 하면 에러가 발생한다.  
```javascript
let user = {
  get fullName() {
    return `...`;
  }
};

//Error(프로퍼티에 getter 메서드만 있어서 에러가 발생한다.)
user.fullName = "Test"; 
```
user.fullName에 setter 메서드를 추가해 에러가 발생하지 않도록 고쳐보자.  
```javascript
let user = {
  name: "John",
  surname: "Smith",
  
  get fullName() {
    return `${this.name} ${this.surname}`;
  },
  
  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
};

//주어진 값을 사용해 set fullName이 실행된다.  
user.fullName = "Alice Cooper";

//Alice
alert(user.name);
//Cooper
alert(user.surname);
```
이렇게 getter와 setter 메서드를 구현하면 객체엔 fullName 이라는 '가상'의  
프로퍼티가 생긴다. 가상의 프로퍼티는 읽고 쓸 순 있지만 실제로는 존재하지  
않는다.  

# 접근자 프로퍼티 설명자   
데이터 프로퍼티의 설명자와 접근자 프로퍼티의 설명자는 다르다.   
접근자 프로퍼티엔 설명자 value와 writable가 없는 대신에 get과 set이라는   
함수가 있다.   
따라서 접근자 프로퍼티는 다음과 같은 설명자를 갖는다.   
- get: 인수가 없는 함수로 프로퍼티를 읽을 때 동작함


# getter와 setter 똑똑하게 활용하기   
getter와 setter를 '실제' 프로퍼티 값을 감싸는 래퍼(wrapper)처럼 사용하면,  
프로퍼티 값을 원하는 대로 통제할 수 있다.   
아래 예시에선 name을 위한 setter를 만들어 user의 이름이 너무 짧아지는 걸  
방지하고 있다. 실제값은 별도의 프로퍼티 `_name`에 저장된다.  
```javascript
let user = {
  get name() {
    return this._name;
  }
  
  set name(value) {
    if (value.length < 4) {
      alert("입력하신 값이 너무 짧습니다. 네 글자 이상으로 구성된 이름을   
      입력하세요.");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); //Pete

user.name = ""; //너무 짧은 이름을 할당하려 함
```
user의 이름은 `_name`에 저장되고, 프로퍼티에 접근하는 것은 getter와 setter  
을 통해 이루어진다.   
기술적으로 외부코드에서 `user._name`을 사용해 이름에 바로 접근할 수 있다.  
그러나 밑줄 `_`로 시작하는 프로퍼티는 객체 내부에서만 활용하고, 외부에서는  
건드리지 않은 것이 관습이다.    
  
# 호환성을 위해 사용하기   
접근자 프로퍼티는 언제 어느 때나 getter와 setter를 사용해 데이터 프로퍼티의  
행동과 값을 원하는 대로 조정할 수 있게 해준다는 점에서 유용하다.   
데이터 프로퍼티 name과 age를 사용해서 사용자를 나타내는 객체를 구현한다고  
가정해보자.   
```javascript
function User(name, age) {
  this.name = name;
  this.age = age;
}

let john = new User("John", 25);

alert(john.age); //25
```
그런데 곧 요구사항이 바뀌어서 age 대신에 birthday를 저장해야 한다고 해보자.   
age보다는 birthday가 더 정확하고 편리하기 때문이다.    
```javascript
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;
}

let john = new User("John", new Date(1992, 6, 1));
```
이렇게 생성자 함수를 수정하면 기존 코드 중 프로퍼티 age를 사용하고 있는   
코드도 수정해줘야 한다.  
  
age가 사용되는 부분을 모두 찾아서 수정하는 것도 가능하지만, 시간이 오래  
걸린다. 게다가 여러 사람이 age를 사용하고 있다면 모두 찾아서 수정하는 것  
자체가 힘들다. 게다가 age는 user 안에 있어도 나쁠 것이 없는 프로퍼티이기도  
하다.   
  
기존 코드들은 그대로 두도록 하자.  
대신 age를 위한 getter를 추가해서 문제를 해결해보자.   
```javascript
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;
  
  //age는 현재 날짜와 생일을 기준으로 계산한다.   
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear = this.birthday.getFullYear();
    }
  });
}

let john = new User("John" new Date(1992, 6, 1));

alert(john.birthday); //birthday를 사용할 수 있다.  
alert(john.age); //age 역시 사용할 수 있다.   
```
이제 기존 코드도 잘 동작하고, 멋진 프로퍼티도 생겼다.   













