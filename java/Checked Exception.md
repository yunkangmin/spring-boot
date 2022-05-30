Checked Exception을 처리하는 전략과 그 것에 유의해야 할 점들에 대해서 이야기한다.  
자바에서는 대표적으로 Checked Exception과  Unchecked Exception이 있다.  
먼저 이 둘의 차이를 살펴보자.   

# Checked, Unchecked Exception 차이
<img src="https://user-images.githubusercontent.com/33191974/150465402-a0a01b1d-54fb-417b-9dad-7574e1968dc9.png" width="50%" height="50%"/>   
Error는 시스템이 비정상적인 상황에서 발생한다. 이 수준의 Error는 시스템 레벨에서  
발생하는 심각한 수준의 오류이기 때문에 개발자가 미리 예측할 수도 없고 처리할 수   
있는 방법도 없다. 애플리케이션 단에서는 Error에 대한 처리를 신경쓰지 않아도 된다.  
OutOfMemoryError이나 ThreadDeath 같은 에러는 try catch으로 잡아도 할 수 있는 것이  
없기 때문이다.  
  
그렇기 때문에 애플리케이션 단에서는 Checked, Unchecked Exception에 대한 처리가  
상대적으로 중요하다.   
(Checked Exception | Unchecked Exception)  
- 처리 여부 : 반드시 예외 처리 해야함 | 예외 처리하지 않아도 됨
- 트랜잭션 Rollback 여부 : Rollback 안됨 | Rollback 진행  
- 대표 Exception : IOException, SQLException | NullPointerException, IllegalArgumentException  

Checked, Unchecked은 개발자들이 만든 애플리케이션 코드에서 예외가 발생했을 경우  
사용하게 된다.  
  
**위 상속 구조처럼 Unchecked Exception는 RuntimeException을 상속하고 Checked Exception는  
RuntimeException을 상속하지 않는다. 이 것으로 두 Exception을 구분하는 중요한 포인트이다.**
  
## Unchecked Exception
명시적인 예외처리를 강제하지 않는 특징이 있기 때문에 Unchecked Exception이라 하며,  
catch로 잡거나 throw로 호출한 메서드로 예외를 던지지 않아도 상관이 없다. 

## Checked Exception
반드시 명시적으로 처리해야 하기 때문에 Checked Exception이라고 하며, try catch를 해서  
에러를 잡든 throws를 통해서 호출한 메서드로 예외를 던져야 한다.   

## Code : 예외 처리 여부
```java
@Test
public void throws_던지기() throws JsonProcessingException {
  final ObjectMapper objectMapper = new ObjectMapper();
  final Member member = new Member("yun");
  final String valueAsString = objectMapper.writeValueAsString(member);
}

@Test
public void try_catch_감싸기() {
  final ObjectMapper objectMapper = new ObjectMapper();
  final Member member = new Member("yun");
  final String valueAsString;
  try {
    valueAsString = objectMapper.writeValueAsString(member);
  } catch (JsonProcessingException e) {
    e.printStackTrace();
    throw new RuntimeException();
  }
}
```
위 JsonProcessingException는 IOException을 상속하는 Checked Exception이다.  
그렇기 때문에 throws로 상위 메서드로 넘기든 자신 try catch해서 throw를 던지든  
해야한다. 이것은 문법적인 강제 선택이다. 그에 반해 Unchecked Exception은 명시적인  
예외 처리를 하지 않아도 된다.   

## Code : Rollback 여부
```java
@Service
@RequiredArgsConstructor
@Transactional
public class MemberService {
  
  private final MemberRepository memberRepository;
  
  // 1. RuntimeException 예외 발생
  //RuntimeException 예외 발생시키면 yun이라는 member는 rollback이 진행된다.
  //참고로 RuntimeException은 Chekced Exception이다.
  public Member createUncheckedException() {
    final Member member = memberRepository.save(new Member("yun"));
    if (true) {
      throw new RuntimeException();
    }
    return member;
  }
  
  // 2. IOException 예외 발생
  //IOException 예외 발생이 되더라도 wan은 rollback이 되지 않고 트랜잭션이  
  //commit까지 완료된다.  
  public Member createCheckedException() throws IOException {
    final Member member = memberRepository.save(new Member("wan"));
    if (true) {
      throw new IOException();
    }
    return member;
  }
}
```

## 왜 Checked Exception은 Rollback되지 않는 것일까?
기본적으로 Checked Exception은 복구가 가능하다는 매커니즘을 가지고 있다.  
예를 들어서 특정 이미지 파일을 찾아서 전송해주는 함수에서 이미지를 찾지 못했을 경우  
기본 이미지를 전송한다. 복구 전략을 가질 수 있게 된다.   

```java
public void sendFile(String fileName) { 
  File file;
  try {
    file = FileFindService.find(fileName);
  //FileNotFoundException은 IOException으로 checked exception이다.
  } catch (FileNotFoundException e) {
    //파일을 못찾았으니 기본 파일을 찾아서 전송한다. 
    file = FileFindService.find("default.png");
  }
  send(file);
}
```
기본적으로 복구가 가능하니 네가 복구 작업을 진행했을 수 있으니까 Rollback은 진행하지  
않는 것이라는 주관적인 생각이다. 
   
하지만 이런 식의 예외는 복구하는 것이 아니라 일반적인 코드의 흐름으로 제어해야 한다.  
```java
public void sendFile(String fileName) {
  if (FileFindService.existed(filename)) {
    //파일이 있는 경우 해당 파일을 찾아서 전송
    send(FileFindService.find(fileName));
  } else {
    //파일이 없는 경우 기본 이미지 전송
    send(FileFindService.find("default.png"));
  }
}
```
## 하지만 현실은...
하지만 우리가 일반적으로 Checked Exception 예외가 발생했을 경우 복구 전략을 갖고  
그 것을 복구할 수 있는 경우는 그렇게 많지 않다.  
  
유니크해야 하는 이메일 값이 중복되서 SQLException이 발생하는 경우 어떻게 복구 전략을  
가질 수 있을까? 유저가 입력했던 이메일 + 난수를 입력해서 insert시키면 가능하겠지만  
현실에는 그냥 RuntimeException을 발생시키고 입력을 다시 유도하는 것이 현실이다.  
  
여기서 중요한 것은 해당 Exception을 발생시킬 때 명확하게 어떤 예외가 발생해서  
Exception이 발생했는지 정보를 전달해주는 것이다. 위 같은 경우에는 DuplicateEmail  
Exception(Unchecked Exception)을 발생시키는 것이 바람직하다.   
  
Checked Exception을 만나면 더 구체적인 Unchecked Exception을 발생시켜 정확한 정보를  
전달하고 로직의 흐름을 끊어야 한다. 우리는 JPA에 구현체를 가져다 사용하더라도  
Checked Exception을 직접 처리하지 않고 있는 이유도 더 적절한 RuntimeException으로  
예외를 던져주고 있기 때문이다.  

# Checked Exception 처리 전략
## Code
```java
public class ObjectMapperUtil {
  
  private final ObjectMapper objectMapper = new ObjectMapper();
  
  //예외처리를 throws를 통해서 위임하고 있다.
  public String writeValueAsString(Object object) throws JsonProcessingException {
    return objectMapper.writeValueAsString(object);
  }
  
  public <T> readValue(String json, Class<T> clazz) throws IOException {
    return objectMapper.readValue(json, clazz);
  }
}
```
wrtieValueAsString, readValue 메서드는 Checked Exception을 발생시키는 메서드이다.   
반드시 예외 처리를 진행해야 한다. 예외 처리를 상위로 던져버리기 때문에 메서드를  
사용하는 곳에서 다시 throw를 하던지 예외를 try catch 하던지 해야 한다. 이렇게  
무의미하고 반복적인 예외를 던지는 것은 좋지 않다.   
  
## 더 구체적인 Unchecked Exception을 발생시켜라
```java
public String writeValueAsString(Object object) {
  try {
    return objectMapper.writeValueAsString(object);
  } catch (JsonProcessingException e) {
    //Checked Exception이 발생하는 wrtieValueAsString() 메서드를  
    //try-catch로 처리하고 다시 구체적인 Unchecked Exception인  
    //JsonSerializeFailed Exception을 발생시키고 있다. 
    throw new JsonSerializeFailed(e.getMessage());
  }
}
  
public <T> readValue(String json, Class<T> clazz) {
  try {
    return objectMapper.readValue(json, clazz);
  } catch (IOException e) {
    throw new JsonDeserializeFailed(e.getMessage());
  }
}
```
[Spring Exception Guide](https://github.com/cheese10yun/spring-guide/blob/master/docs/exception-guide.md#controller)에서 정리한 try catch 전략과 비슷하다.       
  
```java
@Test
public void writeValueAsString_test() {
    final Member member = new Member("");
    //writeValueAsString메서드에 빨간줄이 없어졌다.
    objectMapperUtil.writeValueAsString(member);
}
  
@Test
public void readValue_test() {
  final String json = "{\"name\":\"yun\"}";
  final Member member = objectMapperUtil.readValue(json, member.class);
}
```
Checked Exception을 Unchecked Exception으로 던지고 있기 때문에 메서드를 사용하는  
곳에서는 아무러 예외처리를 진행하지 않아도 된다.   
    
물론 해당 에러가 왜 발생했는지에 대해서 에러 메시지 뿐만이 아니라 더욱 구체적인   
정보를 전달해주는 것이 더 좋다.  
  
# 결론
예외 복구 전략이 명확하고 그 것이 가능하다면 Checked Exception을 try-catch로 잡고  
해당 복구를 하는 것이 좋다.  
    
하지만 그러한 경우는 흔하지 않으며 Checked Exception이 발생하면 더 구체적인  
Unchecked Exception을 발생시키고 예외에 대한 메시지를 명확하게 전달하는 것이   
효과적이다.  
  
  
  
  
  

















































