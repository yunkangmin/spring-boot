<h4>스트림은 컬렉션 인스턴스나 배열을 이용해서 생성할 수 있는데 배열로  
스트림을 생성하기 위해서는 Arrays.stream()을 이용하여 stream()함수의
매개변수로 배열을 넘겨주면 된다.</h4>

```
String[] arr = new String[]{"a", "b" "c"};
Stream<String> stream = Arrays.stream(arr);
// 1 ~ 2 요소 [b, c]
Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
```

### Stream이란?
자바 8에서 등장한 stream은 아래와 같은 특징이 있다.
1. 원본의 데이터를 변경하지 않는다.
2. 일회용이다.  
   한번 사용이 끝나면 재사용이 불가하다.
3. 내부 반복으로 작업을 처리한다.  
   기존에는 반복문을 사용하기 위해 for나 while등을 사용했지만  
   Stream에서는 반복문을 메소드 내부에 숨기고 있기 때문에   
   보다 간결한 코드 작성이 가능하다.

예를 들어 배열이나 리스트의 데이터를 정렬된 상태로 출력하고자 한다.  
stream을 사용하지 않은 경우 아래와 같이 코드를 작성할 수 있다.

```
//stream 사용전
String[] nameArr = ["IronMan", "Captain" "Hulk", "Thor"];
List<String> nameList = Arrays.asList(nameArr);

//원본의 데이터가 직접 정렬됨
Arrays.sort(nameArr);
Collections.sort(nameList);

for (String str : nameArr) {
  System.out.println(str);
}

for (String str : nameList) {
  System.out.println(str);
}
```
위의 코드들도 충분히 괜찮은 코드들이지만, 이를 더욱 간결하고 가독성있게 정리할 수 있고  
원본의 데이터에 변형을 가하지 않는 방법이 있다면 더욱 좋은 코드일 것이다.  
  
Java의 stream은 원본의 데이터 변경없이 위의 코드를 더욱 간략하게 작성하는 방법을  
제공하고 있다.   

```
//Stream 사용후
String[] nameArr = {"IronMan", "Captain", "Hulk", "Thor"};
List<String> nameList = Arrays.asList(nameArr);

//원본의 데이터가 아닌 별도의 Stream을 생성함
Stream<String> nameStream = nameList.stream();
Stream<String> arrayStream = Arrays.stream(nameArr);

//복사된 데이터를 정렬하여 출력함
nameStream.sorted().forEach(System.out::println);
arraysStream.sorted().forEach(System.out::println);
```

















