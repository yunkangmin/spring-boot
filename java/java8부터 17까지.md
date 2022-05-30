# JDK 출시일
- JDK 1.7 : 2011년 7월
- 1.8 : 2014년 3월
- 9 : 2017년 9월
- 11 : 2019년 9월
- 17 : 2021년 9월

JDK 11부터는 6개월에 한 번씩 출시된다.  
자바는 95년 5월 첫 출시되었다.   

# 자바가 프로그램을 실행하는 방식
1. 자바 소스 코드(.java)를 자바 컴파일러가 컴파일하여 바이트코드(.class)를 만든다.     
2. JVM이 바이트 코드를 실행시킨다. 이 때 클래스 파일을 실행하기 위해 필요한    
모든 class 파일을 불러들이고 코드를 검증한 뒤 메모리로 올리는 작업을 한다.  
  
어떻게든 자바 바이트 코드만 만들면 JVM을 통해 실행할 수 있다.  
Groovy, Kotlin, Scala같은 언어는 자바 바이트 코드를 만들어서 JVM을 통해 실행한다.  

# Java 8
- Java 7까지는 싱글코어를 사용한 멀티스레드에 집중
   - 쓰레드를 활용한 병렬 프로그래밍(Fork/Join Framework)을 지원하였으나, 활용도가  
   낮음(fork와 join을 개발자가 직접 하나하나 작성해야 했다. 즉, 각각의 스레드에  
   일을 맡기는 작업에 대한 코드와 작업한 결과를 합칠 때 각각의 쓰레드의 작업이   
   언제 끝났는지 체크하는 코드 그리고 끝나는 시점에 합치는 코드등을 직접   
   작성해야 했다. 그래서 활용도가 낮았다.)  
- Java 8 
   - 간결한 코드
      - 자바를 위협하는 다양한 언어들(Python 등)
      - 함수형 프로그래밍의 장점 채용
   - 멀티 코어를 활용한 병렬 프로그래밍
      - 많은 양의 데이터를 빠르게 처리할 수 있는 시대적 요구사항(빅데이터)
   
Java 8부터는 위의 간결한 코드와 멀티 코어를 활용한 병렬 프로그래밍을 지원하기 위해 다음과  
같이 지원한다.

- 동작 파라미터화(Behavior parameterization)
   - 메서드에 파라미터로 코드를 전달
   - 함수형 프로그래밍
- 람다(Lambda)와 스트림(Stream)
- 병렬 프로그래밍

## 동작 파라미터화(Behavior Parameterization)
메서드에서 어떤 동작을 수행시킬 것인가?   
메서드의 동작이 파라미터로 넘어온 코드에 의해 결정된다.  
```java
public class main {
  public static void main(fianl String[] args) {
    final List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
    //odd 홀수
    //매개변수로 인터페이스 구현체를 넘겨준다(new OddFilter).
    System.out.println(getNumbers(numbers, new OddFilter());
  }
  
  public static List<Integer> getNumbers(List<Integer> numbers, NumberFilter filter) {
    final List<Integer> filteredList = new ArrayList<>();
    for (final Integer number : numbers) {
      if (filter.isFiltered(number)) {
        filteredList.add(number);
      }
    }
    return filteredList;
  }
}
```
```java
public static void main(final String[] args) {
  List<Integer> numbers = new ArrayList<>(Array.asList(1, 2, 3, 4, 5));
  //익명클래스 사용
  System.out.println(getFiltered(numbers, new NumberFilter() {
    @Override
    public boolean isFiltered(final int number) {
      return 0 == number % 2;
    }
  }));
}

public static List<Integer> getFiltered(List<Integer> numbers, NumberFilter filter) {
  List<Integer> filteredList = new ArrayList<>();
  for (Integer number : numbers) {
    if (filter.isFiltered(number)) {
      filteredList.add(number);
    }
  }
  return filteredList;
}
```
**java 1.8에서는 람다식으로 추상 메서드 하나만을 가지는 인터페이스를 구현할 수 있다.**  
```java
public static void main(String[] args) {
  List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
  //람다식 사용
  System.out.println(getFiltered(numbers, (Integer number) -> number % 3 == 1);
}

public static List<Integer> getFiltered(List<Integer> numbers, NumberFilter filter) {
  List<Integer> filteredList = new ArrayList<>();
  for (Integer number : numbers) {
    if (filter.isFiltered(number) {
      filteredList.add(number);
    }
  }
  return filteredList;
}
```
   
## 람다(Lambda)와 스트림(Stream)
- Lambda
   - 특정 클래스에 종속되지 않는 함수
      - 파라미터, 바디, 반환 형식, 예외 등을 가진다.
      - (Parameters) -> expression(특정 로직)
      - (Parameters) -> { statements(2개 이상의 명령어나 리턴값이 있는 로직); }

- 함수형 프로그래밍
   - Functional programs do not have assignment statements - Robert C. Martin   
   - 함수가 동일한 인풋에 항상 동일한 아웃풋이 나와야 한다.  
   -> 함수에 상태값을 저장하는 코드가 없어야 하고 코드가 간결해져야 한다.  
```java
public static void main(String[] args) {
  Comparator<Number> numberComparator = new Comparator<Number>() { 
    @Override
    public int compare(final Number o1, final Number o2) {
      return o1.intValue() - o2.intValue();
    }
  };
}

//위의 익명클래스가 아래와 같이 람다식으로 간결하게 표현할 수 있다.
public static void main(String[] args) {
  Comparator<Number> numberComparator = (o1, o2) => o1 - o2;
}
```
람다식으로 표현하는 방법  
```java
//함수형 인터페이스
@FuntionalInterface
public interface Predicate<> {
  boolean test(T t) ;
}

public static void main(String[] args) {
  Predicate<Integer> OddPredicate = (number) => number % 2 == 0;
  System.out.println(OddPredicate.test(100));
}
```
결론은 Java 8부터 함수형 인터페이스를 지원하면서 람다식을 사용할 수 있게 되었다.  

- 스트림(Stream)
   - 컬렉션 데이터를 함수형으로 처리할 수 있는 기술
      - 컬렉션 데이터를 여러 함수로 가공하여 데이터 흐름을 구성한다.
      - 멀티스레드 구성없이 컬렉션 데이터를 병렬로 처리 가능하다.  

   - Collection과 Stream  
      - 데이터 저장 | 데이터 처리
      - 데이터 추가 저장 기능 | 데이터 추가 저장 불가능
      - 외부 반복자 필요(for문) | 내부 반복자 사용
      - 외부 반복자에 의해 여러번 순회 가능 | 내부 반복자에 의해 한번만 순회 가능  
      - Eager Loading(처리하는 데이터를 모두 순회) | Lazy Loading(불필요한 연산을  
      피한다.) [참고](https://dororongju.tistory.com/137)    
      
스트림 사용 예시    
컬렉션 데이터를 여러 함수로 가공하여 데이터 흐름을 구성한다.  
스트림을 사용하는 것만으로도 내부적으로 멀티스레드 구성없이(멀티스레드를 사용하는데  
코드를 직접 사용하지 않는 다는 것), 병렬처리를 한다.  
Fork/Join FrameWork 활용  
```java
planDisplayProducts.stream()
                   .filter(p -> isThemeProduct(p.getThemeRank()))
                   .sorted(comparing(PlanDisplayProduct::getThemeRank))
                   .map(PlanDisplayProduct::getThemeNo)
                   .distinct()
                   .collect(Collectors.toList());
```
하지만 스트림을 사용하는 게 항상 옳은 것만은 아니다.  
- 순서에 의존하는 연산의 경우, 오히려 성능이 떨어질 수 있다.    
-> sorting을 하지 위한 연산이 추가로 발생  
- 소량의 데이터에서는 병렬 스트림이 큰 도움이 되지 않는다.  
따라서 확신이 서지 않으면, 직접 벤치마킹이 필요하다.  

중간 연산(filter, map)은 컬렉션에 있는 엘리먼트를 모두 순회할 때까지  
엘리먼트마다 반복하고 최종연산(collect)는 한번만 한다.   

## 병렬 프로그래밍
람다식을 이용하여 코드가 간결해졌다.  
비동기 프로그래밍을 예로 들자면 비동기 프로그래밍에서는 아래 인터페이스를   
사용한다.    
- Future 인터페이스 (불편해서 아래 CompletableFuture가 만들어졌다.)
   - Java5
- CompletableFuture
   - Java8
```java
//Future
//1. 비동기 작업의 완료 시점을 모르기 때문에 TimeUnit으로 결과값 획득 완료시점을  
//제어한다.
//2. 깔끔한 예외처리가 어렵다.
//3. 1번의 사유로 두 개의 무거운 비동기 작업을 수행 후, 두 개의 결과값을 합치고    
//싶을 때, 메인 쓰레드에서 각각의 Future의 상태값을 지속적으로 확인해야 한다.   
ExecutorService executor = Executors.newFixedThreadPool(10);
Future<Double> future = executor.submit(new Callable<Double>() {
  public Double call() {
    return getValueFromHeavyCodes();
  }
});
try {
  //1초까지만 기다린다.
  //문제는 메인스레드에 기다리는 시간동안 슬립이 걸린다.  
  Double result = future.get(1, TimeUnit.SECONDS);
  System.out.println("executed By MainThread");
  System.out.println(result);
//아래 에러에 대한 제어를 해야 한다.
} catch (ExecutionException ee) {
  // the computation throw an exception
} catch (InterruptedException ie) {
  // the current thread was interrupted while waiting
} catch (TimeoutException te) {
  // the timeout expired before the Future completion
}

//CompletableFuture
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Hello Future")
                                                     //예외처리
                                                     .exceptionally(ex -> {
                                                        System.out.println(ex.getMessage());
                                                        return "Error";
                                                     //완료시점에 결과를 받을 수 있다.  
                                                      }).thenApply(String::toUpperCase);  
CompletableFuture<Void> future1 = CompletableFuture.runAsync(() -> System.out.println("Hello"));
CompletableFuture<Void> future2 = CompletableFuture.runAsync(() -> System.out.println("wolrd"));

//비동기 작업 2개를 한 번에 실행
//완료시점을 알 수 있기 때문에 
CompletableFuture<Void> combine = CompletableFuture.allOf(future1, future2);
```

# Java 11
- 리액티브 프로그래밍
   - 비동기를 어떻게 효율적으로 처리할 것인가?
      - 데이터의 변화를 하나의 Event(Message)로 정의
      - 발행(publish) - 구독(subscribe)
      - 이벤트가 발생했을 때(발행), 해당 이벤트와 관계있는 모든 컴포넌트(구독)가  
      반응할 수 있도로고 해야 한다.  
      - 단, 컴포넌트가 수용할 수 있는 범위 내에서만 처리되어야 한다.   

- 자잘한 변화
   - 불변 Collection Factory Methods
      - Set.of(), List.of(), Map.of() 등등  
   - 타입 추론 키워드 추가
      - var
   - 신규 가비지 컬렉터  
      - G1GC(JDK 9 ~ / 기본 JDK 11)
      - ZGC(JDK 11 ~)

- Flow API  
   4개의 인터페이스  
   - Publisher 
      - 발행자, 구독자 관리 및 이벤트 발행
   - SubScriber  
      - 구독자, 구독된 이벤트에 대한 실행
   - Subscription
      - 발행자와 구독자간의 중계
   - Processor
      - 발행자와 구독자간 전달되는 이벤트에 대한 변경 또는 에러 처리  

```java
public class SubscriberExample<T> implements Flow.Subscriber<T> {
  
  private Flow.Subscription subscription;
  
  @Override
  public void onSubScribe(final Flow.Subscription subscription) {
    //publisher와 subscriber를 중개하는 역할
    this.subscription = subscription;
    subscription.request(1);
  }
  
  //받아온 데이터를 처리
  @Override
  public void onNext(final T item) {
    System.out.println(Thread.currentThread().getName() + ", " + item);
    subscription.request(1);
  }
  
  @Override
  public void onError(final Throwable throwable) [
    throwable.printStackTrace();
  }
  
  @Override
  public void onComplete() {
    System.out.println("Done");
  }
}
```
예)  
```java
List<String> items = List.of("1", "2", "3", "4", "5", "6", "7", "8", "9", "10");
//퍼블리셔 구현체
SubmissionPublisher<String> publisher = new SubmissionPublisher<>();
//구독자 등록
publisher.subscribe(new SubscriberExample<>());
publisher.subscribe(new SubscriberExample<>());
//보낼 데이터 등록
//등록을 하면 자동으로 내부에서 처리를 한다(각각의 쓰레드에서 처리).
items.forEach(publisher::submit);
try {
  Thread.sleep(3000);
} catch (InterruptedException e) {
  e.printStackTrace():
}
publisher.close();
}

```
Java11은 Java8과 비교해서 큰 변경점이 있는 것은 아니었다.  
Flow API가 추가된 정도이다.  
Java11과 17도 큰 차이는 없다.  

# Java 17
Java 15부터 지원  
```java
public class Json {
  String text = "{\n" + 
                " \"name\" : \"John Doe\", \n" +
                " \"age\" : 45, \n" + 
                " \"address\" : \"Doe Street, 23, Java Town\" \n" +
                "}";
  System.out.println(text);
  
  String text1 = 
  """
  {
    "name" : "John Doe",
    "age" : 45,
    "address" : "Doe Street, 23, Java Town"
  }    
  """
  System.out.println(text1);
```
Java 13부터 지원  
```
public class Switch {
  //JDK 13
  //break문이 없어짐
  public static void main(String[] args) {
    int switchable = 1;
    switch(switchable) {
      case 1, 2 -> System.out.println("2 이하");
      case 3, 4 -> System.out.println("3이상 4이하");
      case 10 -> System.out.println("10");
      default -> System.out.println("처리 못함");
    }
    
    //반환값을 받는다.
    //String을 반환한다.
    final String returnable = switch (switchable) {
      case 3 -> "2 이하 - ";
      default -> "처리 못함";
    }
    System.out.println(returnable);
    
    //JDK 15
    //yield를 사용 위의 방식과 비슷하지만
    // ":"을 사용한다는 점에서 다르다.
    final String yield = switch (switchable) {
      case 1 : yield "kk";
      default : yield "ll";
    };
    System.out.println(yield);
}
```
record라는 형식추가  
```java
public record Team(Long seq, String name) {
  private void abc() {
    //TODO
  }
}

//위 코드는 아래와 같다.
public class Team {
  private final Long seq;
  private final String name;
  
  public Team(final Long seq, final String name) {
    this.seq = seq;
    this.name = name;
  }
}

//record 클래스는 아래와 같이 사용한다.  
public class TeamClient {
  public static void main(String[] args) {
    Team team = new Team(1L, "프로모션플랫폼개발팀");
    //JDK 15
    //setter를 제외하고 lombok의 value와 동일
    //kotlin의 data class
    //getter와 toString 지원
    System.out.println(team.name());
    System.out.println(team.seq());
    System.out.println(team);
  }
}
```
```java
//SealedInterfaceImpl에만 구현이 가능하게
public sealed interface SealedInterface permits SealedInterfaceImpl {
  public void sealedMethod();
}

//구현 클래스에서는 final 키워드를 붙여줘서  
//다시 상속하지 않게 해야 한다. 아니면 
//non-sealed로 상속이 가능하다고 명시를 해줘야 한다.
public final  class SealedInterfaceImpl implements SealedInterface {
  @Override
  public void sealedMethod() {
  }
}
```
instanceof
```java
public class InstanceOfPattern {
  public static void main(String[] args) {
    Object o = new Team(1L, "PPD");
    
    if ((o instanceof Team)) {
      Team team = (Team) o;
      System.out.println(team.name() + " / " + team.seq());
    }
    
    if ((o instanceof Team team)) {
      System.out.println(team.name() + " / " + team.seq());
    }
  }
}
```
NPE  
로깅방식이 변경됬다.  
기존에는 어느 라인에서 에러가 발생했는지만 표시되었는데  
이제는 어떤 값, 어떤 메서드에서 발생했는지가 표시된다.  
```java
public class NPE {
  public static void main(String[] args) {
    Map<String, Team> teams = new HashMap<>();
    var abc = teams.get("abc").name();
  }
}
```
```
string.formatted
Stream.of().collect(Collectors.toList());
-> Stream.of().toList()(불변)
```

JDK 17에서는 kotlin의 장점을 많이 가져오는 모습을 볼 수 잇다.  



  





























