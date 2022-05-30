@JvmStatic은 static 변수의 get/set 함수를 자동으로 만들라는 의미이다.   
예제로 알아보자.  
  
다음 Bar 클래스는 barSize라는 변수를 companion object에 선언함으로써,   
전역변수를 만들었다.   
```kotlin   
class Bar {
    companion obejct {
        var barSize : Int = 0
    }
}
```
사실 companion object는 자바의 static과 다르다. 자바로 변환해보면 Bar  
클래스에 barSize는 선언되었지만 get/set함수는 Bar.Companion 클래스에   
등록되었다.   
```java
public final class Bar {
    private static int barSize;
    public static final class Companion {
        public final int getBarSize() {
            return Bar.barSize;
        }
        public final void setBarSize(int var1) {
            Bar.barSize = var1;
        }
    }
}
```
자바에서 get/set 함수에 접근하려면 다음처럼 Companion을 꼭 써줘야 한다.  
```java
Bar.Companion.getBarSize();
Bar.Companion.setBarSize(10);
```

`@JvmStatic`은 Bar 바로 밑에 get/set 함수가 생성되게끔 만든다. 아래 코틀린  
코드는 barSize를 선언할 때 `@JvmStatic`와 함께 선언했다.   
  
```kotlin  
class Bar {
    companion object {
        @JvmStatic var barSize : Int = 0
    }
}
```
위 코드를 자바로 변환해보면 다음과 같다. Bar 바로 밑에 get/set 함수가   
생겼고, Companion은 이전과 동일하게 get/set을 갖고 있다.  

```java
public final class Bar {
    private static int barSize;
    public static final int getBarSize() {
        return barSize;
    }

    public static final void setBarSize(int var0) {
        barSize = var0;
    }

    public static final class Companion {
        public final int getBarSize() {
            return Bar.barSize;
        }
        public final void setBarSize(int var1) {
            Bar.barSize = var1;
        }
    }
}
```
자바에서 위 코드를 접근하면 Bar.Companion도 가능하지만 Bar.getBarSize처럼  
바로 접근해도 된다.   

```kotlin    
Bar.getBarSize();
Bar.setBarSize(10);
Bar.Companion.getBarSize();
Bar.Companion.setBarSize(10);
```
정리하면 `@JvmStatic`은 Companion에 등록된 변수를 자바의 static처럼 선언  
하기 위한 annotation이다.  


































