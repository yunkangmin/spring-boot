자바에서 리스트를 다루다 보면, 하나의 리스트를 잘라서 사용해야 할 때가 생긴다.  
이런 경우 List.subList를 사용한다.  
예를들어
```java
List<Integer> list = Lists.newArrayList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
System.out.println(list.subList(0, 3));
```
이와 같이 List에서 0, 2까지의 인덱스를 자르고 싶은 경우 아래와 같은 방식으로 사용한다.  
다만, 이런 subList에는 메모리 누수 위험성이 있다. 
이유는 subList의 소스코드를 보면 나온다. 

```
public List<E> subList(int fromIndex, int toIndex) {
  subListRangeCheck(fromIndex, toIndex, size);
  return new SubList(this, 0, fromIndex, toIndex);
}
```
이 소스는 ArrayList의 subList의 소스코드이다.  
return new SubList라는 생성자를 통해서 새로운 리스트를 만들어서 내보낸다.  
여기서 문제는 SubList라는 객체에 있다.  

```
static void subListRangeCheck(int fromIndex, int toIndex, int size) {
  if (fromIndex < 0)
    throw new IndexOutOfBoundsException("fromIndex = " + fromIndex);
  if (toIndex > size)
    throw new IndexOutOfBoundsException("toIndex = " + toIndex);
  if (fromIndex > toIndex)
    throw new IllegalArgumentException("fromIndex(" + fromIndex + ") > toIndex("  
                                  + toIndex + ")");
}

private class SubList extends AbstractList<E> implements RandomAccess {
  private final AbstractList<E> parent;
  private final int parentOffset;
  private final int offset;
  int size;

```
소스는 직접 보는 것이 더 좋다. 우선 SubList의 클래스코드의 앞 뒤로 살짝 잘라서   
복붙했다. ArrayList의 SubList는 자신이 생성된 parent 값을 가지고 있다.  
이를 통해 다음 2가지 문제가 생긴다. 
1. 캐시 사용 시 불필요한 메모리 점유  
예를 들어 1만개의 리스트에서 단순 10개만 subList로 만들고 이를 캐시에 저장하는 경우  
원래 원하던 그림은 10개의 데이터만 캐시되는 것 이겠지만, 이 구현 방식으로 인해 1만개  
의 리스크 값들이 모두 캐시되게 된다. 이를 통해 불필요한 메모리를 점유할 수 있다. 
2. parent가 바뀌는 게 subList에 영향(parent 값을 참조만 하는듯)  
예를 들어
```
List<Integer> list = Lists.newArrayList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> subList = list.subList(0, 3);
System.out.println(subList);
list.remove(0);
System.out.println(subList);
```
실행시  
```
[1, 2, 3]


에러
```

### 정리
subList는 다음과 같이 사용할 수 있다.  
```
List<Integer> subList = new ArrayList<>(list.subList(0, 3));
```
이런 식으로 subList를 따로 생성하는 것이다. 카피(값을 복사)해서 따로 생성하는 듯. 
예제는 다음과 같다.  
```
List<Integer> list = Lists.newArrayList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> subList = new ArrayList<>(list.subList(0, 3));
System.out.println(subList);
list.remove(0);
System.out.println(subList);
```
실행 결과는 다음과 같다.  
```
[1, 2, 3]
[1, 2, 3]
```
























































