모든 코드는 [여기](https://github.com/lannstark/aws-sqs-spring-boot)있다.  
이제 메시지 보내는 것은 성공했으니 보낸 메시지를 받아보려고 한다.  
그 전에 MessageConverter에 대해서 간단히 알아보자.  
QueueMessagingTemplate은 단순한 String을 주고받는 메소드 외에, Java Object를 받는  
다양한 전송 메소드들이 존재한다. 그 중에 convertAndSend()와 receiveAndConvert()는  
MessageConverter 인터페이스에게 직렬화/역직렬화 책임을 위임하고 있다.  
실제로 코드를 살펴보면 아래와 같다.  

### AbstractMessageSendingTemplate(spring-messaging)
```
@Override
public <T> void convertAndSend(String destination, T payload) throws MessagingException {
  D channel = resolveMessageChannelByLogicalName(destinationName);
  //최종적으로 convertAndSend(channel, payload, null, null)호출
  convertAndSend(channel, payload);
}

@Override
public void convertAndSend(D destination, Object payload, @Nullable Map<String, Object> headers,  
  @Nullable MessagePostProcessor postProcessor) throws MessagingException {
  Message<?> message = doConvert(payload, headers, postProcessor);
  send(destination, message);
}

protected Message<?> doConvert(Object payload, @Nullable Map<String, Object> headers,  
  @Nullable MessagePostProcessor postProcessor) {
  //여차저차 전처리...
  
  MessageConverter converter = getMessageConverter();
  
  //바로 여기서 MessageConverter가 사용된다.
  Message<?> message = (converter instance of SmartMessageConverter ? 
                    ((SmartMessageConverter) converter).toMessage(payload, messageHeaders, conversionHint) :
                    convert.toMessage(payload, messageHeaders));
  
  //여차저차 후처리...
  
  return message:
}
}
```
### QueueMessagingTemplate(spring-cloud-aws-messaging)
```
//최종적으로 호출되는 receiveAndConvert
@Override
public <T> T receiveAndConvert(QueueMessageChannel destination, Class<T> targetClass) 
          throws MessaingException {
  Message<?> message = destination.receive();
  if (message != null) {
    //바로 여기서 MessageConverter가 사용된다.
    return (T) getMessageConverter().fromMessage(message, targetClass);
  }
  else {
    return null;
  }
}
```
QueueMessagingTemplate이 AbstractMessageSendingTemplate을 상속한다.   
그래서 QueueMessagingTemplate는 AbstractMessageSendingTemplate에 있는 convertAndSend() 메소드를  
사용할 수 있다.   
MessageConverter는 CompositeMessageConverter를 기본 구현체로 사용하고 CompositeMessageConverter안에는  
StringMessageConverter와 MappingJackson2MessageConverter가 들어있다. 
MessageConverter 인터페이스는 아래와 같이 생겼다.  
```
public interface MessageConverter {
  //Message의 payload를 주어진 targetClass로 역직렬화한다.  
  //지원하지 않는 media type이거나 역직렬화를 수행할 수 없으면 null을 반환한다. 
  @Nullable
  Object fromMessage(Message<?> message, Class<?> targetClass);
  
  //주어진 payload와 headers를 가지고 있는 Message를 만든다.
  @Nullable
  Message<?> toMessage(Object payload, @Nullable MessageHeaders headers);
}
```
테스트를 해보자.  
```
//MainController 메소드
@PostMapping("/message")
public void sendMessage(@RequestBody String name) {
  messageSender.sendMessage(name);
}

//SqsMessageSender 메소드
public void sendMessage(String name) {
  queueMessagingTemplate.convertAndSend("sqs-study", new Person(name, 20));
}

//Person 클래스
//Getter가 없으면 직렬화할 때 MappingJackson2MessageConverter가 처리할 수 없게 된다.
@Getter
@ToString
//기본 생성자가 없으면 역직렬화할 때 MappingJackson2MessageConverter가 처리할 수 없게 된다. 
@NoArgsConstructor
@AllArgsConstructor
public class Person {
  
  private String name;
  private int age;
}
```
body에 test01을 넣고 POST /message를 호출하니 SQS에 다음과 같은 JSON 형태로 message가 잘 존재하는 것 
을 확인할 수 있다.  
QueueMessagingTemplate이 생성될 때 Jackson 모듈이 있는지 확인해서 넣어준다.  
![image](https://user-images.githubusercontent.com/33191974/139532583-946951e3-696a-4904-aaa0-caed5542cfc2.png)
![image](https://user-images.githubusercontent.com/33191974/139532626-6dc3b638-555c-4f1c-9880-9419829de473.png)  
  
이제 메시지를 받아보자.  
메시지를 받는 방법은 2가지가 존재하는데 우선은 첫 번째 방법부터 알아보자.  
```
//MainController 메소드 추가
@GetMapping("/message")
public void getMessage() {
  messageService.getMessage();
}

//SqsMessageSender에서 SqsMessageService로 이름 변경후 메소드 추가
public void getMessage() {
  Person person = queueMessagingTemplate.receiveAndConvert("sqs-study", Person.class);
  System.out.println("SQS로부터 받은 메시지: " + person);
}
```
![image](https://user-images.githubusercontent.com/33191974/139532984-f75a4213-d907-454e-acd7-25b59b5023a8.png)
![image](https://user-images.githubusercontent.com/33191974/139532994-c90e4a1c-597a-4ed9-85ec-b8104dca2293.png)  
이 때 주의할 점은 receiveAndConvert는 Message 처리의 성공/실패여부와 무관하게 자동으로 Queue에 delete요청을  
날리게 된다. 또한 timeout 값에는 항상 0이 고정되어 있어 long polling 대신 short polling을 사용하게 된다.  
short polling을 사용하면 SQS 호출수가 늘어나 요금이 더 많이 청구될 수 있다.  
아래는 receiveAndConvert를 호출할 경우에 최종 사용되는 코드이다.   

가급적 사용하지 않는 편이 좋은 것 같다.

































                  
