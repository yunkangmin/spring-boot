## @VisibleForTesting의 의미
#### 쓰는 의도
테스트하려는 클래스의 private 메서드, 필드, 클래스를 테스트하기 위해서 붙이는 어노테이션.  
이걸 가시성 완화라고 부름.  
  
말 그대로 대상 메서드, 필드, 클래스가 테스트 코드에서는 Visible하게 하고 그렇지 않은 코드 부분에서는  
private, package-private 등 다른 접근 지정자를 설정할 수 있음.

#### 절차
일단 private 지우고 (다른 패키지에서 접근이 안되므로)

```java
class SomeService {
  public int process(int param) {
    int intimidate = processStep1(param);
    return processStep2(intimidate);
  }
  
  //원래는 private여야 하지만 테스트를 위해 package-private으로 가시성을 완화한다.
  @VisibleForTesting
  /* private */ int processStep1(int param) {
  }
}

class SomeServiceTest {
  @Test
  void testStep1() {
    SomeService service = new SomeService();
    //processStep1() 메서드의 가시성이 완화되 테스트 클래스에서 접근할 수 있다.
    assertEquals(0, service.processStep1(1));
  }
}

class AnotherService() {
  void foo() {
    SomeService service = new SomeService();
    service.process(0);
    //processStep1() 메서드에 접근할 수는 있지만
    //@VisibleForTesting 에너테이션이 붙어있으므로
    //메서드를 호출하면 안된다.
    //service.processStep1(0);
  }
}
//출처 : https://d2.naver.com//helloworld/8725603
```

@VisibleForTesting하면 다른 코드들(엄밀히 말하면 Production code)에게는 private으로 처리되고  
테스트 코드에서는 접근 가능하다.

#### 응용
---
#### @VisibleForTesting(otherwise = VisibleForTesting.NONE)</br> int TEST;
TEST 변수가 테스트 코드에서 쓸 수 있고, Production code에선 다른 패키지던 같은 패키지던   
아예 못씀. 완전 테스트용이라는 의미.

#### @VisibleForTesting(otherwise = VisibleForTesting.PACKAGE_PRIVATE) </br> int TEST;
TEST 변수가 테스트 코드에서 쓸 수 있고, Production code에선 다른 패키지는 아예 못씀.
같은 패키지는 이 메서드 쓸 수 있음.

#### @VisibleForTesting(otherwise = VisibleForTesting.PRIVATE) </br> int TEST
TEST 변수가 테스트 코드에서 쓸 수 있고, Production code에선 다른 패키지, 같은 패키지 못쓰고  
클래스 내부에서만 쓸 수 있음.

#### @VisibleForTesting(otherwise = VisibleForTesting.PROTECT) </br> int TEST;
TEST 변수가 테스트 코드에서 쓸 수 있고, Production code에선 상속까지 허용하는 범위에서  
쓸 수 있음.

#### @VisibleForTesting </br> int TEST;
지정되지 않을 경우, private으로 간주.

## 정리
@VisibleForTesting을 사용하기 위해서는  
일단 private int a; 에 있는 private을 지우고
위에 @VisibleForTesting으로 설정해둔 뒤  
otherwise로 접근 지정자 제한해준다고 생각하면 된다.  
otherwise로 아무것도 안하면 package-private나 마찬가지이다.  
otherwise로 private로 할건지 protect로 할 건지 설정해줘야 한다.

