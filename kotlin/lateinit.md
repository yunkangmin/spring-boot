코틀린에서는 변수 선언을 먼저하고 초기화는 뒤로 미루는 기능들을 제공한다.   
**초기화를 늦추면 좋은 점이, 사용할지 모르는 데이터를 미리 초기화할 필요가  
없어서 성능 향상에 도움이 된다. 예를 들어, Rest API로 GitHub의 데이터를  
가져오는 기능이 있는데, 앱이 실행했을 때 미리 가져오는 것보다 데이터를   
화면에 보여줄 때 가져오는 것이 CPU 자원도 아끼고, 네트워크 자원도 아낄 수  
있다.**     
  
코틀린에서 제공하는 초기화 지연은 다음과 같은 것들이 있다.   
- Late initialization : 필요할 때 초기화하고 사용할 수 있음. 초기화하지  
않고 쓰면 Exceptoin 발생  
- Lazy initialization : 변수를 선언할 때 초기화 코드도 함께 정의. 변수가  
사용될 때 초기화 코드 동작하여 변수가 초기화됨.   
  
위의 두 개의 기능은 초기화를 지연한다는 점에서 유사해보이지만 차이점이  
많다. 각각의 특징을 알아보고 차이점을 비교해보자.   
  
# Late initialization
late initialization은 var 앞에 lateinit을 붙여 변수를 선언하면 된다. late  
라는 말에서 코드를 늦게 초기화한다는 의미로 생각할 수 있다. 코드가 직관적  
이기 때문에 코드로 먼저 살펴보자.   
```kotlin
class Rectangle {
  lateinit var area: Area
  fun initArea(param: Area): Unit {
    this.area = param
  }
}

class Area(val value: Int)

fun main() {
  val rectangle = Rectangle()
  rectangle.initArea(Area(10))
  println(rectangle.area.value)
}
```
위의 코드에서는 lateinit var area로 선언하고 초기값을 지정하지 않았다.  
이 변수는 nullable이 아니기 때문에 초기값을 할당하지 않으면 에러가 발생  
한다. 하지만 lateinit을 붙여 나중에 초기화하겠다고 컴파일러에게 말했기  
때문에 컴파일 에러가 발생하지 않는다.   
  
코드를 자세히 보면, main에서 `rectangle.initArea(Area(10))`로 늦게 변수를  
초기화하고 그 다음 줄에 Area 객체를 사용하고 있다. 만약 초기화를 하지  
않고 Area 변수에 접근하면 `UninitializedPropertyAccessExeption`이 발생한다.   
  
다음은 초기화보다 Area 객체에 먼저 접근한 코드이다.   
```kotlin  
fun main() {
  val rectangle = Rectangle()
  println(rectangle.area.value)
  rectangle.initArea(Area(10))
}
```
코드를 실행해보면 Area의 value 객체에 접근할 때 초기화가 되어있지 않아  
아래와 같이 Exception이 발생했다.   
```
Exception in thread "main" kotlin.UninitializedPropertyAccessException: lateinit property area has not been initialized
	at Rectangle.getArea(test5.kt:2)
	at Test5Kt.main(test5.kt:15)
	at Test5Kt.main(test5.kt)
```
정리하면, var에 lateinit을 붙이면 초기값을 나중에 설정할 수 있다. 하지만,  
초기값을 설정하기 전에 사용하면 예외가 발생한다.   
  
# lateinit의 특징   
사실 lateinit은 var에만 사용할 수 있다. 또한, primitive type에 적용할 수  
없다. Primitive type은 Int, Boolean, Double등의 코틀린에서 제공하는 기본  
적인 타입을 말한다. 따라서 아래처럼 val을 사용하거나, Int를 사용한 코드는  
컴파일 에러가 발생한다.  
```kotlin  
lateinit val area : Area //compile error
lateinti var width : Int //compile error
```
그리고 lateinit 프로퍼티는 custom getter/setter를 설정할 수 없다. 또한   
non-null 프로퍼티만 사용이 가능하다. 따라서, 아래의 코드들은 컴파일이   
안된다.   
```kotlin   
lateinit var area: Area? //compile error
lateinit var area: Area //compile error
  get() {
    area;
  }
```
정리하면 lateinit의 특징은 다음과 같다.   
- var 프로퍼티만 사용 가능
- primitive type(Int, Boolean)은 사용할 수 없음
- Custom getter/setter를 만들 수 없음
- Non-null 프로퍼티만 사용 가능   

# lateinit의 동작 원리  
자바 프로그래머를 위해 lateinit을 사용한 코틀린 코드가 자바로 어떻게 변환   
되는지 살펴보자. 자바로 디컴파일된 코드를 보면 lateinit이 내부적으로 어떻게  
동작하는지 알 수 있다.    
  
다음은 위의 예제를 자바로 디컴파일한 코드이다.   
```kotlin   
public final class Area {
  private final int value;
  
  public final int getValue() {
    return this.value;
  }
  
  public Area(int value) {
    this.value = value;
  }
}

public final class Rectangle {
  @NotNull
  public Area area;
  
  @NotNull
  public final Area getArea() {
    Area var10000 = this.area;
    if (var10000 == null) {
      Intrinsics.throwUninitializedPropertyAccessException("area");
    }
    
    return var10000;
  }
  
  public final void setArea(@NotNull Area var1) {
    Intrinsics.checkParameterIsNotNull(var1, "<set-?>");
    this.area = var1;
  }
  
  public final void initArea(@NotNull Area param) {
    Intrinsics.checkParameterIsNotNull(param, "param");
    this.area = param;
  }
}
 
public static final void main() {
  Rectangle rectangle = new Rectangle();
  rectangle.initArea(new Area(10));
  int var1 = rectangle.getArea().getValue();
  System.out.println(var1);
}
```
디컴파일된 자바 코드를 보면 public Area area로 변수가 초기화되지 않은  
상태로 선언되었다. 그리고 가장 중요한 것은 다음 코드이다.   
```kotlin  
public final Area getArea() {
  Area var10000 = this.area;
  if (var10000 == null) {
    Intrinsics.throwUninitializedPropertyAcceessException("area");
  }
  
  return var10000;
}
```
Area.getArea()로 Area 객체를 가져올 때 null check를 하고 있다. null이면  
Exception을 던진다. Area를 가져올 때는 모두 getArea()를 사용하기 때문에  
초기화 전에 쓰려고 시도하면 예외가 발생하는 것을 알 수 있다. 이 때문에   
custom getter/setter를 제한했다는 것을 추측해볼 수 있다.  























