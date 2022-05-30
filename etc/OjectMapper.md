# 정리
- API 호출 시 응답이 JSON으로 오면 이걸 객체로 변환
   - 객체 -> JSON (serialization)
   - JSON -> 객체 (deserialization)  

---

# ObjectMapper를 이용해서 JSON 파싱학   
프로젝트를 진행하면서 로그인 기능을 구현하기 위해 카카오 api를 사용했다.   

발급받은 client_id와 redirec_uri를 포함하여 요청을 보냈더니 JSON 형태로   
응답을 받을 수 있었다.   
  
그 다음 응답받은 JSON 객체를 POJO 형태로 deserialization시켜 내가 사용하고  
자 하는 데이터를 JSON 객체로부터 가져와야 했다.       
  
필자는 여기서 JSON 객체를 역직렬화 시키기 위해 사람들이 많이 사용하고 유  
명한 jackson 라이브러리의 ObjectMapper 클래스를 이용했다.  
  
필자가 ObjectMapper 클래스를 이용하여 JSON 객체를 역직렬화시켰던 과정을  
정리해보려 한다.   
  
# ObjectMapper란?   
- JSON 컨텐츠를 Java 객체로 deserialization 하거나 Java 객체를 JSON으로  
serialization할 때 사용하는 Jackson 라이브러리의 클래스이다.   
- ObjectMapper는 생성 비용이 비싸기 때문에 bean/static으로 처리하는 것이  
좋다.  

# ObjectMapper 이용하기 
ObjectMapper를 이용하면 JSON을 Java 객체로 변환할 수 있고, 반대로 Java 객  
체를 JSON 객체로 serialization할 수 있다.   
아래와 같은 Person 클래스를 이용하여 어떻게 사용하는지 간단하게 알아보자   
```java
import lombok.AllArgsConstructor
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Getter
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class Person {
  private String name;
  private int age;
  private String address;
}
```

# Java Object -> JSON
Java 객체를 JSON으로 serialization하기 위해서는 ObjectMapper의 writeValue  
() 메서드를 이용한다.   
```java
import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.File;
import java.io.IOException;

public class ObjectMapperEx {
  public static void main(String[] args) {
    ObjectMapper objectMapper = new ObjectMapper();
    
    //Java Object -> JSON
    Person person = new Person("zooneno", 25, "seoul");
    try {
      objectMapper.wirteValue(new File("src/person.json"), person);
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
위와 같이 파라미터로 JSON을 저장할 파일과 직렬화시킬 객체를 넣어주면 된다.  
여기서 주의할 점은 JSON으로 직렬화시킬 클래스에 Getter가 존재해야 한다는  
것이다.   
Jackson 라이브러리는 Getter와 Setter를 이용하여 prefix를 잘라내고 맨 앞을  
소문자로 만드는 것으로 필드를 식별한다.  
그렇기 때문에 만약 직렬화시킬 클래스에 Getter가 존재하지 않으면 클래스에서  
필드를 식별하지 못하고 결국 값을 가져오지 못하여 에러가 발생하게 된다.   
정상실행하면 다음과 같이 내가 지정한 경로에 json 파일이 생성된나.   
<img src="https://user-images.githubusercontent.com/33191974/163519851-b3f1889d-d37b-4e7a-8da2-12ffcb201aba.png" width="50%" height="50%"/>  
파일을 열어보면 Java 객체로 넣어줬던 값들이 JSON 형태로 잘 저장되어 있는  
것을 볼 수 있다.  
#### src/person.java   
```
{"name": "zooneon", "age": 25, "address": "seoul" }
```
# JSON -> Java Object
JSON 파일을 Java 객체로 deserialization하기 위해서는 ObjectMapper의 read  
Value() 메서드를 이용한다.  
```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;

public class ObjectMapperEx {
  public static void main(String[] args) {
    ObjectMapper objectMapper = new ObjectMapper();
    
    //JSON -> Java Object
    String json = "{\"name\": \"zooneon\", \"age\": 25, \"address\":\"  
    seoul\"}";
    try {
      Person deserializePerson = objectMapper.readValue(json, Person.class);
      System.out.println(deserialization);
    } catch (JsonProcessingException e) {
      e.printStackTrace();
    }
  }
}
//실행결과 : Person(name=zooneon, age=25, address=seoul)
```
위와 같이 파라미터로 JSON 형태의 문자열 or 객체와 역직렬화시킬 클래스를  
넣어주면 된다.   
  
여기서 주의할 점이 있는데, 역직렬화시킬 클래스(여기서는 Person 클래스)에  
JSON을 파싱한 결과를 전달할 생성자가 있어야 한다.   
  
필자는 기본 생성자를 이용하였지만 생성자에 Jackson 라이브러리의 @JsonCrea  
tor 어노테이션을 쓰는 등 다양한 방법이 있다.   
  
만약 다음과 같은 에러가 발생한다면, 클래스에 적절한 생성자가 없는 경우이다.  
```
com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Cannot construct instance of `Person` (no Creators, like default constructor, exist): cannot deserialize from Object value (no delegate- or property-based Creator)
 at [Source: (String)"{"name":"zooneon","age":25,"address":"seoul"}"; line: 1, column: 2]
	at com.fasterxml.jackson.databind.exc.InvalidDefinitionException.from(InvalidDefinitionException.java:67)
	at com.fasterxml.jackson.databind.DeserializationContext.reportBadDefinition(DeserializationContext.java:1764)
	at com.fasterxml.jackson.databind.DatabindContext.reportBadDefinition(DatabindContext.java:400)
	at com.fasterxml.jackson.databind.DeserializationContext.handleMissingInstantiator(DeserializationContext.java:1209)
	at com.fasterxml.jackson.databind.deser.BeanDeserializerBase.deserializeFromObjectUsingNonDefault(BeanDeserializerBase.java:1415)
	at com.fasterxml.jackson.databind.deser.BeanDeserializer.deserializeFromObject(BeanDeserializer.java:362)
	at com.fasterxml.jackson.databind.deser.BeanDeserializer.deserialize(BeanDeserializer.java:195)
	at com.fasterxml.jackson.databind.deser.DefaultDeserializationContext.readRootValue(DefaultDeserializationContext.java:322)
	at com.fasterxml.jackson.databind.ObjectMapper._readMapAndClose(ObjectMapper.java:4593)
	at com.fasterxml.jackson.databind.ObjectMapper.readValue(ObjectMapper.java:3548)
	at com.fasterxml.jackson.databind.ObjectMapper.readValue(ObjectMapper.java:3516)
	at ObjectMapperEx.main(ObjectMapperEx.java:22)
```  


# Jackson으로 파싱한 JSON 속성값을 생성자로 전달하기   
## @JsonCreator
Jackson에서 제공하는 @JsonCreator, @JsonProperty를 값을 전달할 생성자와   
메서드 파라미터에 붙인다.  
  
AccessLog의 생성자에 @JsonCreator 선언   
```java
import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonProperty;

public classs AccessLog {
  
  //멤버 변수 선언 생략
  
  @JsonCreator
  public AccessLog(
    @JsonProperty("accessDateTime") Instant accessDateTime,
    @JsonProperty("ip") String ip,
    @JsonProperty("username") String username) {
      this.accessDateTime = accessDateTime;
      this.ip = ip;
      this.username = username;
    }
    
    //getter 생략
}
```
- 장점
   - JSON의 속성명과 객체의 멤버변수명이 다를 때도 자연스럽게 활용할 수   
   있다.   
   - 생성자가 여러 개일 때 Jackson에서 사용할 생성자를 명시적으로 지정할  
   수 있다.  
- 단점
   - Jackson에 의존적인 방법이다.   
      - Jar파일로 배포하는 클래스 안에서 이 방법을 사용하려면 Jackson에  
      대한 의존성이 추가된다.  
      - JSON 파싱 라이브러리를 교체한다면 전체 클래스를 수정해야 한다.  



















