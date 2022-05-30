## 자바스크립트 함수 호출하기, bind()
자바스크립트에서 함수를 호출할 경우 bind() 메소드를 사용할 수 있다.  
만약 어떤 함수가 있을 때 이 함수를 실행하기 위해서 bind()를 사용하면 어떻게 될까?  
이 경우 bind()는 새로운 함수를 반환하게 된다. 이는 함수를 실행하기 위해 () 소괄호를 사용하는 것과는  
차이점, 다른 특징을 가질 수 있다. 즉, call(), apply()처럼 this를 지정할 수도 있고 또한 원하는 인자를  
전달할 수도 있다. 이런 이유로 bind()가 사용된다. 그럼 간단한 사용 문법은 아래와 같다.  

```javascript
Function.bind(thisArg, [arg1, arg2, ...])
```
- thisArg //this가 가리킬 객체를 지정
- [arg1, arg2 ...] //함수의 인자로 전달할 값

이 때 첫번째 인자인 thisArg는 선택옵션으로 값을 전달하지 않아도 되며 null 등을 사용할 수도 있다.  
그 다음은 함수에 전달할 인자 값으로 배열 타입으로 전달하게 된다. 이처럼 인자값을 배열로 전달하는 것은  
apply()와 비슷하다. 

>call(), apply()와의 차이점은?  
>거의 동일하게 활용이 가능하지만 함수를 실행하지 않고 함수를 생성한 후 반환하는 점에서 call(), apply()  
>와의 차이점이다. 

## 자바스크립트 bind() 예제보기
다음으로 몇 가지 예제를 만들어 알아보자. 만약 아래와 같이 객체 mySite가 존재한다고 생각해보자.  
```javascript
mySite = {
  name: 'webisfree',
  getSite: function () {
    return this.name;
  }
}
```
이 객체는 getSite 이름의 메소드를 호출할 수 있다. 실행하면 아래와 같이 나타난다. 
```javascript
mySite.getSite();

//출력결과
"webisfree"
```

다음으로 getYourSite라는 변수를 하나 만들고 여기에 mySite.getSite 메소드를 선언한다.  
이제 이 함수를 실행하면 어떻게 될까?
```javascript
getYourSite = mySite.getSite;
getYourSite();

//출력결과
undefined
```
실행해보니 결과는 undefined가 나타나게 된다. 즉 함수의 this에는 name이라는 프로퍼티가 존재하지 
않으므로 window 객체를 참조하여 값을 반환하기 때문이다.  
  
이제 bind()를 사용하여 this 키워드가 mySite를 가리키도록 한 후 다시 실행하면 어떻게 될까?  
아래와 같이 입력 후 실행한다.  
```javascript
getMySite = getYourSite.bind(mySite);
getMySite();

//출력결과
'webisfree'
```
이번에는 this를 mySite로 가리키도록 bind()를 사용하였기 때문에 'webisfree'를 출력할 수 있다.  
이와 같이 bind()를 사용하면 this가 어떤 인스턴스를 가리킬 것인지 선택할 수 있다. 











































