## Optional
[Optional](http://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/)

## Optional이란?
- 메서드가 반환할 결과값이 ‘없음’을 명백하게 표현할 필요가 있고, null을 반환하면 에러를 유발할 가능성이  
높은 상황에서 메서드의 반환 타입으로 Optional을 사용하자는 것이 Optional을 만든 주된 목적이다.   

## 정리
1. 1번은 Optional로 반환받은 이후를 설명하는 것이고 2번은 Optional을 만드는 시점에 대한 설명이다.

## 1. isPresent() - get() 대신 orElse()/orElseGet()/orElseThrow()
```
//안좋음
Optional<Member> member = ...;
if (member.isPresent()) {
  return member.get();
} else {
  return null;
}

//좋음
//일단 Optional로 감싸서 member를 반환받고
//그 이후에 어떻게 해야 되는지를 설명
//member가 있으면 Optional<Member>가 아닌 Member 반환
//없으면 null 반환.
Optional<Member> member = ...;
return member.orElse(null);

//안 좋음
Optional<Member> member = ...;
if (member.isPresent()) {
  return member.get();
} else {
  throw new NoSuchElementException();
}

//좋음
Optional<Member> member = ...;
return member.orElseThrow(() -> new NoSuchElementException());
```


## 단지 값을 얻을 목적이라면 Optional 대신 null 비교
Optional은 비싸다. 따라서 단순히 값 또는 null을 얻을 목적이라면  
Optional 대신 null 비교를 쓰자.
```
//안좋음
//메소드에서 값을 얻을 목적
//메소드 안에서 리턴하는 코드
//여기서는 위에 처럼 Optional로 감싸서 받은 이후를 설명하는게 아니라
//처음부터 값을 어떻게 반환하는지를 설명
//Optional.ofNullable()은 매개변수가 null일 가능성이 있을 때 사용한다.
//null이 아니라면 Optional<Status>가 아닌 Status가 반환되고
//매개변수가 null이라면 orElse()의 매개변수가 리턴된다.
return Optional.ofNullable(status).orElse(READY);

//좋음
//메소드에서 값을 얻을 목적
//메소드 안에서 리턴하는 코드
return status != null ? status : READY;
```

---
## orElse(), orElseGet()
```
public void ohMyGod() {
  String username = null;
  String result1 = Optional.ofNullable(username).orElse(getDefaultName());
  System.out.println(result1);
  
  String result2 = Optional.ofNullable(username).orElseGet(() -> getDefaultName());
  System.out.println(result2);
}

private String getDefaultName() {
  return "no name";
}
```
이 경우에 결과는 어떻게 될까?
둘다 "no name"이다. 결과는 같지만 굉장한 차이가 있다.  
orElse의 경우는 "값"을 취한다. orElseGet은 "Supplier"를 취한다.  

위의 예시는 아래 코드로 다시 쓰여질 수 있다.  
```
String username = null;
//이 부분이 다르다.
String defaultName = getDefaultName(); 
String result1 = Optional.ofNullable(username).orElse(defaultName);
System.out.println(result1);

//다른 부분
Supplier<String> supplier = () -> getDefaultName();
String result2 = Optional.ofNullable(username).orElseGet(supplier);
System.out.println(result2);
```
>위 코드를 보면 알겠지만 orElse는 null이던말던 항상 불린다.
>orElseGet은 null일 때만 불린다.


















































