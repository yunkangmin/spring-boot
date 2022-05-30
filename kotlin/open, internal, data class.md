## open
자바에서는 클래스에 final이 붙지 않으면 모두 다른 클래스에서 상속이 가능하다.
하지만 코틀린에서의 클래스와 메서드는 기본적으로 final이다.  
  
따라서 어떤 클래스의 상속을 허용하려면 해당 클래스 앞에 open 변경자를 붙여야 한다.  
그와 더불어 오버라이드를 허용하고 싶은 메서드나 프로퍼티의 앞에도 open 변경자를 붙여야 한다.  

```kotlin
open class Car {
  
  //이 메서드는 하위 클래스에서 override 불가능
  fun getNumberOfTires(): Int {
    return 4
  }
  
  //하위 클래스에서 override 가능
  open fun hasSunRoof(): Boolean {
    return false
  }
}

//open 클래스는 상속이 가능하다.
class Benz() : Car() {
  
  //getNumberOfTires 메서드는 override 불가능
  //hasSunRoof 메서드는 open 변경자가 붙어서 override 가능
  override fun hasSunRoof(): Boolean {
    return true
  }
}
```

위는 open 변경자를 붙인 클래스와 메서드이 예이다.  
주석으로 설명했으니 추가 설명은 생략한다.  
다음 abstract 클래스에 open 메서드의 예를 보자.

```kotlin
abstract class Animal {
  
  //추상 메서드는 반드시 override 해야함
  abstract fun bark()
  
  //이 메서드는 하위 클래스에 선택적으로 override 할 수 있다.
  open fun running() {
    println("animal running!")
  }
}

classc Dog() : Animal() {
  
  override fun bark() {
    println("멍멍")
  }
  
  //이 메서드는 override 하거나 하지 않거나 자유
  override fun running() {
    println("dog's running!")
  }
}
```

정리하자면 
open class : 다른 클래스에서 상속할 수 있음
open method : 해당 메서드를 하위 클래스에서 override 할 수 있음

## internal 가시성 변경자
internal 키워드에 대해 자세히 알아보기 전에 자바와 코틀린의 가시성 변경자에 대한 차이를 먼저  
알아보자.  
  
자바는 public, protect, private 변경자가 있다.  
하지만 코틀린에서 다른 점은 2가지가 있다.  
  
첫 번째로, 아무 변경자도 없는 경우 선언은 모두 공개(public)된다.
두 번째로, 자바의 기본 가시성인 패키지 전용은 코틀린에 없다.  
코틀린은 패키지를 네임 스페이스를 관리하기 위한 용도로만 사용한다.  
그래서 패키지를 가시성 제어에 사용하지 않는다. 
  
패키지 전용 가시성에 대안으로 코틀린에는 internal 이라는 새로운 가시성 변경자가  
도입됬다.  
자바에서 default는 패키지 이름을 보고 접근이 가능한지 판단하기 때문에 보안의 위험을 나타낼 수도  
있다.   
코틀린에서 자바의 단점을 보완하기 위해 internal 접근 제한자를 제공한다.  
자바에서 제한하는 패키지 단위와는 달리 동일한 모듈 내에 있는 클래스들의 접근을 제한한다.  
코틀린에서 제한하는 모듈의 범위는  
- IntelliJ IDEA 모듈
- Maven / Gradle 프로젝트
- 하나의 Ant 테스크 내에서 함께 컴파일되는 파일들  
  
범위를 제한함으로써, 보안을 높이고, 자바의 단점을 보완한 코틀린이다. 
internal은 같은 모듈 내에서만 볼 수 있으며, 모듈은 한꺼번에 컴파일되는 코틀린 파일들을  
의미한다.  
  
자바에서는 같은 패키지 안에서 protected 멤버에 접근할 수 있지만  
코틀린의 protected는 다르다.  
(자바에서 접근제어자가 protected로 설정되었다면 protected가 붙은 변수, 메서드는 동일 패키지 내의  
클래스 또는 해당 클래스를 상속받은 외부 패키지의 클래스에서 접근이 가능하다.)  
protected 멤버는 해당 클래스나 그 클래스를 상속한 클래스 안에서만 보인다.  

## data class
자바에서 대게 equlas, hashCode, toString을 오버라이드 해야하는 경우가 많았다.  
코틀린은 이런 불편함을 줄이기 위해 data라는 변경자를 클래스 앞에 붙이면 필요한 메서드를  
컴파일러가 자동으로 만들어준다.  
data 변경자가 붙은 클래스를 데이터 클래스라 부른다.  

```kotlin
class Person(val name:String, val age:Int) {
  
  override fun equlas(other: Any?): Boolean {
    if (this === other) return true
    if (javaClass != other?.javaClass) return false
    
    other as Person
    
    if (name != other.name) return false
    if (age != other.age) return false
    
    return true
  }
  
  override fun hashCode(): Int {
    var result = name.hashCode()
    result - 31 * result + age
    return result
  }
  
  override fun toString(): String {
    return super.toString()
  }
}
```

먼저 위는 일반 클래스에 IDE의 도움으로 equals, hashCode, toString을 재정의한 코드이다.  
위 코드의 결과는 다음과 같다.  

```
data.Person@5880f7ce
```

이제 Person 클래스를 데이터 클래스로 만들고, equals, hashCode, toString이 잘 작동하는지 알아보자.  


```kotlin
data class Person(val name:String, val age: Int)

fun main() {
  val p1 = Person("conatuseus", 30)
  var p2 = Person("conatuseus", 30)
  
  // == 은 동등성 검사
  println(p1 == p2)
  
  // === 은 같은 인스턴스인지 검사(동일성 확인)
  println(p1 === p2)
  
  println(p1.toString())
  
  val p3 = p1.copy()
  val p4 = p1.copy(name = "newName", age = 20)
}
```

기존 Person 클래스를 데이터 클래스로 변경했다.  
그리고 잘 작동하는지 확인하기 위해 동등성 검사, 동일성 검사, toString을 출력하도록 했다.  

```
true
false
Person(name=conatuseus, age=30)
```

먼저 동등성 검사를 보면 true가 나왔다.  
이는 동등성 검사가 잘 된다는 뜻이다.  
그리고 toString으로 출력된 것을 보면 매우 아름답다.  
기존에는 Person@1234 이런 식으로 출력되던 toString이 매우 아름답게 출력된 것을 확인할 수 있다.  
  
마지막으로 데이터 클래스는 copy 메서드도 컴파일러가 자동으로 만들어준다.  
copy 메서드가 왜 필요한지, 어떻게 사용하는지 알아보자.  
  
데이터 클래스의 프로퍼티가 꼭 val(불변 변수)일 필요는 없지만,  
데이터 클래스의 모든 프로퍼티를 읽기 전용으로 만들어서 데이터 클래스를 불변(immutable) 클래스로  
만들라고 권장한다.  
  
데이터 클래스 인스턴스를 불변 객체로 더 쉽게 활용할 수 있게 코틀린 컴파일러는 copy 메서드를  
제공한다.  
copy 메서드는 객체를 복사하면서 일부 프로퍼티를 바꿀 수 있게 해준다.  
복사본은 원본과 다른 생명주기를 가지며, 복사를 하면서 일부 프로퍼티 값을 바꾸거나 복사본을 제거해도  
프로그램에서 원본을 참조하는 다른 부분에 전혀 영향을 끼치지 않는다. 
  




























