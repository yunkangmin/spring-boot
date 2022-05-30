## FlatMap이란?
만약 우리가 처리해야 하는 데이터가 2중 배열 또는 2중 리스트로 되어 있고  
이를 1차원으로 처리해야 한다면 어떻게 해야 할까? 이러한 경우에 map을 이용해도  
결과는 2중 Stream의 형태일 것이다. 이처럼 중첩 구조를 한 단계 제거하기 위한  
중간 연산이 필요한데, 이 것이 바로 flatMap이다. flatMap은 Function 함수형  
인터페이스를 매개변수로 받고 있는데, 이 매개변수는 Stream을 extends하여 구현한  
객체여햐 한다.  
예를 들어 다음과 같이 2중 리스트가 존재한다고 할 때, 이를 1중 리스트로 변환하기 위해서  
flatMap을 이용할 수 있다.
```
//flatMap 함수
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);

// [[a], [b]]
List<List<String>> list = Arrays.asList(Arrays.asList("a"), Arrays.asList("b"));

//[a, b]
List<String> flatList = list.stream()
  .flatMap(Collection::stream)
  .collect(Collectors.toList());
```

## Optional map flatMap 차이점
Optional isPresent()를 제거하다 보니 flatMap을 사용해야 하는 경우가 발생해서 정리해본다.  
로직에서 Optional이 연속적으로 리턴되는 경우에 flatMap을 사용해야 한다.  
Optional map과 flatMap의 메서드 시그니처는 아래와 같다.
```
public<U> Optional<U> map(Function<? super T, ? extends U> mapper);

public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper);
```
차이점은 mapper function의 리턴 타입인데 flatMpa의 경우 리턴타입이 Optional이어야 한다.

```
assertEquals(Optional.of(Optional.of("STRING")), Optional.of("string")
                                                         .map(s -> Optiona.of("STRING")));
assertEquals(Optional.of("STRING"), Optional.of("string")
                                             .flatMap(s -> Optional.of("STRING")));
```
**※ map은 결과를 Optional로 감싸서 리턴하는 반면 flatMap()은 그냥 반환한다.**

























