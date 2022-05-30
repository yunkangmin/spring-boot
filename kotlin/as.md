# "Unsafe" cast operator  
일반적으로, 캐스트 연산자는 캐스트할 수 없는 경우에 exception을 던진다.  
그러므로 unsafe라고 부른다. unsafe cast는 코틀린에서 as 연산자로 사용한다.  
```kotlin
var x : String = y as String
```
※ null은 String으로 캐스팅될 수 없다. 즉 위 코드에서 y가 null이라면 excep  
tion이 발생한다.   
```kotlin  
fun main(args: Arrays<String>) {
  val y : String? = null
  var x : String = y as String
  print(x)
}
```

<img src="https://user-images.githubusercontent.com/33191974/162448963-190b4f8c-4ac5-4c85-bebc-977195ad7c0a.png" width="50%" height="50%"/>    

때문에 아래처럼 사용한다.   
```kotlin
var x : String? = y as String?
```
```kotlin
fun main(args: Array<String>) {
  val y : String? = null
  var x : String? = y as String?
  print(x)
}
```

<img src="https://user-images.githubusercontent.com/33191974/162449319-fcc02450-2c19-40f2-8a4d-1c247793acbf.png" width="50%" height="50%"/>    

# "Safe" (nullable) cast operator  
exception의 발생을 피하려면 안전한 연산자인 as?를 사용하면 된다.  
as?를 사용하면 캐스팅이 안될 경우 null을 반환한다.   






















