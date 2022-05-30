# Unit
```kotlin
fun printHello(name: String?): Unit {
  if (name != null) 
    println("Hello ${name}")
  else
    println("Hi there!")
  //return Unit 혹은 return이라고 써도 되고 안 써도 된다.  
  // 'return Unit' or 'return' is optional
}
```
반환값이 필요없을 때, 함수의 반환 타입으로 Unit을 사용한다. 반환 타입이   
Unit이면 함수 끝에 return을 쓰지 않아도 된다. 물론 굳이 return을 쓰고 싶다면   
써도 된다. 단, 이때는 return 뒤에 표현식을 적지 말고 return만 단독으로  
사용해야 한다.    
  
코틀린의 Unit 타입은 자바의 void에 대응되는 개념이다. 하지만 이 둘이 완전  
히 같은 것은 아니다. void는 반환 값이 없음을 의미하는 특수 타입이지만,  
Unit은 class 키워드로 정의된 일반 타입이기 때문이다. 자바의 void 클래스와  
비슷한 개념으로 보면 된다.   
  
Unit 타입을 반환하는 함수는 , return을 생략한다고 해도 암묵적으로 Unit  
타입의 객체를 return하도록 되어 있다. 단, 그 Unit 객체는 싱글톤 인스턴스   
이기 때문에 매번 객체를 생성하지는 않는다.   




















