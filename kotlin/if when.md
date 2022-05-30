# if문
1. if문 사용법
코틀린에서 if문의 문법은 특별하지 않다.   
```kotlin
fun main(args: Array<String>) {
    val input = Scanner(System.`in`).nextInt()
    
    if (input < 0)
      println("음수")
    else if (input > 0)
      println("양수")
    else
      println(0)
}
```
하지만 코틀린에서 if문은 특별한 기능이 있다.  
바로 값을 리턴할 수 있다.   
  
2. 값을 리턴하는 if문   
코틀린에서는 삼항 연산자를 지원하지 않으므로 이를 if문을 사용하여 구현할   
수 있다.   
```kotlin
fun main(args: Array<String>) {
  val a = 5
  val b = 10
  
  val maxValue = if (a > b) {
    a
  } else {
    b
  }
  println(maxValue)
}
```
위 코드에서 a(5)는 b(10)보다 작으므로 else 문을 타고 maxValue에는 b(10)이  
들어가게 된다. 코틀린에서는 지원하지 않지만 삼항연산자로 표현한다면 아래  
와 같을 것이다.  
```kotlin
maxValue = (a > b) ? a : b
```
만약 if문의 블럭 내의 여러 줄의 식이 있다면 리턴값은 마지막 줄이 된다.   
```kotlin
val maxValue = if (a > b) {
    println(a)
    a
} else {
    println(b)
    b
}
```
# when문   
1. when문 사용법  
코틀린에서 when 문은 다른 언어의 switch문과 동일하다.   
```kotlin
fun main(args: Array<String>) {
  
  val value = 3
  
  when (value) {
      1 -> println("1")
      2 -> println("2")
      else -> println("3")
  }
}
```
value의 값이 1일 경우 1을 출력, 2일 경우 2를 출력, 그리고 그 밖의 모든  
경우는 3을 출력한다. switch문과 비교하자면 when 문에서는 'case: '는 '->'로  
표현하고 'default'는 'else'로 표현한다.  

2. 값을 리턴하는 when문   
when문과 if문과 마찬가지로 값을 리턴할 수 있다.   
```kotlin 
fun main(args: Arrays<String>) {
    
  val check: Boolean? = null
  
  val value = when(check) {
    true -> 1
    false -> 0
    else -> null
  }
}
```
단, 주의할 점은 when 문이 리턴값을 가지는 용도로 사용하게 되면 반드시 모든  
경우의 조건을 정의해야 한다.   
그렇지 않을 경우 컴파일 에러가 발생한다.   
```kotlin
fun main(args: Array<String>) {
    
  val check: Boolean? = null
  
  val value = when(check) {
    true -> 1
    false -> 0
  }
}
```
null branch를 추가하거나 else branch를 추가하라는 에러 메세지가 나온다.   
따라서 모든 branch(조건식)를 정의하거나 else branch를 반드시 정의해야 한다.   
```kotlin
fun main(args: Array<String>) {
    val check: Boolean? = null
  
    val value = when(check) {
        true -> 1
        false -> 0
        //null -> null or
        else -> null
    }
}
```
3. 다양한 조건식을 가질 수 있는 when문   
when문은 표현식, 범위(range)를 조건식으로 가질수도 있다.  
먼저, 표현식을 사용한 when문을 살펴본다.   
```kotlin
fun main(args: Array<String>) {
    
  val check: Boolean? = null
  val p = Person("Mark", 10)
  
  when (p) {
      is Person -> println("0")
      else -> println("X")
  }
}
```
위 코드에 대해 간략히 설명하면 객체의 p의 타입이 Person이면 O를 출력,   
그렇지 않으면 X를 출력한다. 인스턴스 p는 Person 타입이므로 is Person은  
true를 리턴하여 해당 branch의 명령을 수행한다.    
  
다음은 범위(range)를 사용하여 branch 조건을 설정한 경우이다.   
```kotlin  
fun main(args: Array<String>) {
    val score = 95
    val grade = when(score) {
        in 90 .. 100 -> "A"    //90 ~ 100
        in 80 until 90 -> "A"  //80 ~ 89
        in 70 until 80 -> "C"  //70 ~ 79
        else -> "F"            // ~ 69
```






















