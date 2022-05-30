# 범위 지정 함수(Scope function)란?
범위 지정 함수는 특정 객체에 대한 작업을 블록 안에 넣어 실행할 수 있도록  
하는 함수이다. 블록은 특정 객체에 대해 할 작업의 범위가 되며, 따라서 범위  
지정 함수라 부른다. 특정 객체에 대한 작업을 블록 안에 넣게 되면 가독성이   
증가하여 유지 보수가 쉬워진다. 코틀린에서는 let, run, apply, also, with   
총 5가지 기본적인 범위 지정함수를 지원한다.   

> # 코틀린의 범위 지정 함수   
> 1. apply
> 2. run
> 3. with
> 4. let
> 5. also

# 범위 지정함수와 수신객체 지정 람다(함수)      
범위 지정함수는 다른 말로 수신객체 지정 람다(함수)라고도 부른다. 이유는 수신  
객체를 명시하지 않거나 it을 호출하는 것만으로 람다 안에서 수신객체의 메서들  
호출할 수 있도록 해주기 때문이다. 이 것이 가능한 이유는 블록(block) 람다  
식에서 수신객체를 람다의 입력 파라미터 혹은 수신객체로 사용하였기 때문이다.   
  
이렇게 말하면 어떤 말인지 이해하기 어려우니 실제 사용 예제를 통해 이 것이  
무슨 말인지 알아보자.  
  
also에서의 block은 람다식의 입력 파라미터로 also의 수신객체(T)를 지정한다.   
```kotlin
public inline fun <T> T.also(block: (T) -> Unit): T
```
<img src="https://user-images.githubusercontent.com/33191974/162375883-54325ec2-f0da-4956-9f83-8e726c74fd7a.png" width="50%" height="50%"/>  
apply에서의 block은 람다식의 수신객체로 apply의 수신객체(T)를 지정한다.   
```kotlin
public inline fun <T> T apply(block: T () -> Unit): T
```
<img src="https://user-images.githubusercontent.com/33191974/162376092-fb31e9b2-7346-4f0b-bc33-37db6fa22654.png" width="50%" height="50%"/>  
위 두 가지를 활용하면 람다 블록에서 수신객체 지정함수의 수신객체를 명시하지  
않고 접근 가능하거나 it로 접근할 수 있게 된다. 이에 대해 분류를 하면 다음  
과 같다.  
<img src="https://user-images.githubusercontent.com/33191974/162377169-27575318-0311-4c89-a2fb-3049d7beeacc.png" width="50%" height="50%"/>  
  
우리는 이 글에서 다음의 data class를 활용하여 각각이 어떻게 적용되는지  
살펴볼 것이다.   
```kotlin  
data class Person(
  var name: String = "",
  var age: Int = 0,
  var temperature: Float = 36.5f
)
```

# apply   
apply는 수신객체 내부 프로퍼티를 변경한 다음 수신 객체 자체를 반환하기  
위해 사용되는 함수이다. 따라서 객체 생성 시에 다양한 프로퍼티를 설정해야  
하는 경우 자주 사용된다.   
  
apply에서의 block은 람다식의 수신객체로 apply의 수신객체(T)를 지정하기  
때문에 람다식 내부에서 수신객체에 대한 명시를 하지 않고 함수를 호출할 수  
있게 된다.   
```kotlin  
public inline fun <T> T.apply(block: T.() -> Unit): T
```

<img src="https://user-images.githubusercontent.com/33191974/162378133-2bbd0cf5-7125-4b4b-8203-391e6020cdd0.png" width="50%" height="50%"/>  

apply를 활용하면 다음 방법으로 수신객체의 프로퍼티 지정이 가능하다. 람다식  
의 수신 객체가 apply의 수신 객체이기 때문에 수신객체에 대한 명시를 생략하  
는 것이 가능하다.    
```kotlin  
val person = Person().apply {
  name = "DevCho"
  age = 29
  temperature = 36.2f
}
```
프로퍼티 설정 시마다 person을 쓰지 않아도 돼서 가독성이 좋은 것을 볼 수   
있다.   

※ 기존에는 위의 PersonClass를 초기화하고 프로퍼티를 설정하기 위해서는   
다음의 방법을 사용했다.   
```kotlin  
val person = Person()
person.name = "DevCho"
person.age = 29
person.temperature = 36.2f
```

















