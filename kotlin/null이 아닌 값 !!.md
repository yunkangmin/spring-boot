null이 아닌 값(not-null assertion)은 코틀린에서 느낌표를 이중(!!)으로 사용  
하면 어떤 값이든 null이 될 수 없는 타입으로 바꿀 수 있다.  
그렇기 때문에 실제 null 값에 대해 !!를 적용하면 NullPointerException이 발생  
한다.   
<img src="https://user-images.githubusercontent.com/33191974/161378167-0325d8fa-d307-475a-9c88-6081b2a095ea.png" width="50%" height="50%"/>   
```
/* null 아님 !! 예제 */
fun ignoreNull(s: String) {
  val stringNotNull: String = s!! //예외는 이 시점에서 발생된다.  
  println(stringNotNull.length) //stringNotNull은 null이 아닌 값으로   
  //인식된다.
}

>> ignoreNull(null)
Exception in thread "main" kotlin.KotlinNullPointerException...
```
근본적으로 !!는 컴파일러에게 개발자가 "이 값이 null이 아님을 확신한다!   
만약 내 생각이 잘못됐다면 예외가 발생해도 감수하겠다"라는 의미로 볼 수 있다.  
!! 기호의 모습 자체를 마치 컴파일러에게 소리를 지르는 듯한 느낌으로 코틀린  
설계자들이 일부러 의도한 것이라고 한다.  

























