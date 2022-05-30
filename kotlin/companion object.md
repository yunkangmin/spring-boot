오늘은 companion object에 대해 알아본다.  
Java 코드를 살펴보고 Kolin으로 바꿔보도록 하자.   
  
# Java에서의 상수   
```java
public class Person {
  public static final int MAX_AGE = 500;
}

public static void main() {
  System.out.println(Person.MAX_AGE);
}
```
간단한 코드이다.  
Person 클래스 내부에 MAX_AGE라는 상수가 public 접근 제한자로 존재하고, 
이는 외부에서 Person.MAX_AGE로 접근할 수 있다.    
만약 Person 클래스 내부에서만 접근하게 하고 싶다면 public을 private으로  
바꿀 수 있다.   
클래스와 인스턴스 관점에서 바라보자면, static 필드인 MAX_AGE는 Person의   
인스턴스가 생길 때마다 메모리가 추가로 할당되는 것이 아니고, Person 클래스  
설계도 자체에 함께 존재하게 된다.   
  
# Kotlin에서의 companion object   
위의 코드를 Kolin으로 똑같이 바꿔보자.   
```kotlin
class Person {
  companion object {
    const val MAX_AGE: Int = 500
  }
}

fun main() {
  printf(Person.MAX_AGE)
}
```
class Person 내부에 companion object라는 구문이 존재하고 해당 객체 내에  
const val이 붙은 변수가 존재하게 된다.   
const가 붙은 이유는, MAX_AGE는 런 타임이 아니라 컴파일 타임에 500이란   
값으로 초기화되기 때문이다.   
또한, 코틀린에서는 기본 접근 제한자가 public이기 때문에 class와 const val  
앞에 public을 붙이지 않아도 된다.   

# Java에서의 static method  
자바에는 static 필드 외에도 static method가 존재한다.   
```java
public class Person {
  public static void sayHello() {
    System.out.println("안녕하세요");
  }
}

public static void main() {
  System.out.println(Person.sayHello());
}    
```
코틀린에서도 비슷한 구현이 가능하다.   

# Kotlin에서의 companion object  
```kotlin
class Person {
  companion object {
    fun sayHello() {
      println("안녕하세요")
    }
  }
}

fun main() {
  Person.sayHello()
}
```
하지만 흥미롭게도 코틀린의 companion object에는 다른 점이 두 가지 존재한다. 

# companion object에 이름 붙이기  
companion object도 엄연히 하나의 객체이기 때문에 이름을 붙일 수 있게 된다.  
```kotlin
class Person {
  companion object Constant {
    const val MAX_AGE: Int = 500
  }
}
```
이름을 붙였다고 사용법이 달라지지는 않는다.   
```kotlin
println(Person.MAX_AGE) //가능
println(Person.Constant.MAX_AGE) //가능
```

# companion object에서 인터페이스 구현하기  
위에서 언급한 것처럼 companion object도 엄연히 하나의 객체이기 때문에   
interface를 구현할 수도 있다.   
```kotlin
interface Movable {
  fun move() 
}

class Person {
  companion object: Movable {
    override fun move() {
      println("움직였다!!")
    }
  }
}

fun main() {
  Person.move()
}
```
여기서 주의할 점은 companion object 역시 클래스, 즉 설계도에 붙어 있는   
객체이기 때문에 프로세스에서 한 인스턴스만 존재하게 된다(간단히 말해 싱글  
톤이다).  
```kotlin
class Person {
  companion object {
    var age: Int = 10
  }
}

fun main() {
  println(Person.age)
  Person.age++
  println(Person.age)
}
```


















