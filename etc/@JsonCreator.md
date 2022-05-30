# @JsonCreator
- JSON key와 멤버필드의 이름이 일치하지 않을 경우에도 사용할 수 있다.     
```
{
  "id": 1,
  "theName": "My bean"
}
```
```
public static class BeanWithCreator {
  public int id;
  public String name;
  
  @JsonCreator
  public BeanWithCreator(
    @JsonProperty("id") int id,
    @JsonProperty("theName") String name
  ) {
    this.id = id;
    this.name = name;
  }
}
```

---

## @JsonCreator는 왜 쓰는 걸까?   
Jackson의 objectmapper를 이용해 객체 serialization/deserialization을   
무의식 중에 많이 해왔다. Kotlin을 사용하면서는 더욱더 문제될 것이 없었다.  
그런데 async_app에 mq worker를 추가하면서 message model에 @JsonCreator를  
사용하는 걸 발견했다. Jackson에서 다양한 기능을 제공하는 annotation들을 봐  
왔지만 @JsonCreator는 무엇일까? 지금까지 이거 없이도 deserialization 잘 해  
왔는데 애는 왜 쓰는걸까?   
  
Jackson이 json을 deserialize해서 객체로 만들기 위해서는 객체를 생성하고  
필드들을 채울 수 있어야 한다. 별도의 설정이 없으면 기본적으로 다음과 같이  
동작한다.   
  
### 생성  
- 기본 생성자 사용   
### 채우기   
- field가 public이면 직접 assignment(할당)  
- private면 setter사용    
객체를 encapsulation하는 것은 기본에 가까운 권장 습관이므로 무조건 setter를  
사용한다고 보면 된다. 근데 만약 deserialize 한 후에 해당 객체가 immutable  
(불변)하기를 원한다면? setter가 없어야 한다.       

이때 기본 생성자 + setter 조합을 대신해 객체를 생성할 수 있도록 해주는   
것이 @JsonCreator 어노테이션이다. 이 어노테이션을 생성자나 팩토리 메서드  
위에 붙이면 jackson이 해당 함수를 통해 객체를 생성하고 필드를 생성과   
동시에 채운다. 이렇게 생성자나 팩토리 메서드를 통해 필드 주입까지 끝내  
버리면 setter 함수가 필요없게 된다. jackson을 통해 deserialize한 immu  
table한 객체를 얻을 수 있는 것이다. 그래서 @JsonCreator를 쓰는 것 같다.  

생성자를 포함한 함수는 컴파일 되면 파라미터의 이름은 var0, var1처럼 임의  
적으로 바꿔 버린다. 그래서 jackson이 아무리 열심히 json을 파싱해서 key  
value를 얻어도 이걸 @JsonCreator가 붙은 함수에 어떤 순서로 호출해야 하는  
지 알 수 없다. 그래서 각 파라미터마다 `@JsonProperty(name="blah")`를  
넣어줘야 한다. reflection할 때 어노테이션에 들어 있는 이름을 통해서 파싱한  
데이터들을 적절한 순서로 넣어줘야 하기 때문이다.   

































