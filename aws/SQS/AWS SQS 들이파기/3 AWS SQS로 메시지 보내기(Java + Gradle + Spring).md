## AWS SQS로 메시지 보내기(Java + Gradle + Spring)
SQS로 메시지를 보내는데 몇 가지 방법이 있을 것 같지만  
여기서는 스프링 부트로 SQS에 메시지를 쏴볼 것이다.  

1. 의존성 설정
```
plugins {
  id 'org.springframework.boot' version '2.3.3.RELEASE'
  id 'io.spring.dependency-management' version '1.0.10.RELEASE'
  id 'java'
}

group = 'com.lannstark'
version = '0.1.0'
sourceCompatibility = '11'

configurations {
  compileOnly {
    extendsFrom annotationProcessor
  }
}

repositories {
  mavenCentral()
}

dependencies {
  implementation 'org.springframework.boot:spring-boot-starter-web'
  implementation group: 'org.springframework.cloud', name: 'spring-cloud-aws-messaging', version: '2.2.3.RELEASE'
  //Auto Configure 없으면 안된다..!! No bean of AmazonSQS등과 같은 에러를 볼 수 있다.
  implementation group: 'org.springframework.cloud', name: 'spring-cloud-aws-autoconfigure', version: '2.2.3.RELEASE'
  
  compileOnly 'org.projectlombok:lombok;
  annotationProcessor org.projectlombok:lombok
  
  testImplementation ('org.springframework.boot:spring-boot-starter-test') {
    exclude group: 'ogr.junit.vintage', module: 'junit-vintage-engine'
  }
}

test {
  useJUnitPlatform() 
}
```
2. Controller, Service 간단하게 만들어보기
application.yml 설정
```
cloud:
  aws:
    region:
      static: ap-northeast-2
    stack:
      suto: false
```
```
@RequiredArgsConstructor
@RestController
public class MainController {
  
  private final SqsMessageSender messageSender;
  
  @PostMapping("/message")
  public void sendMessage(@RequestBody String message) {
    messageSender.sendMessage(message);
  }
}
```
```
@Service
public class SqsMessageSender {
  
  private final QueueMessagingTemplate queueMessagingTemplate;
  
  @Autowired
  public SqsMessageSender(AmazonSQS amazonSQS) {
    this.queueMessagingTemplate = new QueueMessagingTemplate(AmazonSQSAsync) amazonSqs);
  }
  
  public void sendMessage(String message) {
    Message<String> newMessage = MessageBuilder.withPayload(message).build();
    queueMessagingTemplate.send("sqs-study", newMessage);
  }
}
```
spring-cloud-aws를 이용하면 QueueMessagingTemplate을 이용하여 SQS에 메시지를 보낼 수 있다.  
SqsMessageSender가 해주는 역할은 간단하다. 문자열을 받아서 sqs-study라는 Queue로 전송한다.  
  
우선 로컬에서 실행해보면 몇 가지 에러가 나올 수 있다. 에러들의 해결 방법은 에러 폴더를 참고한다.
소스는 [여기](https://github.com/lannstark/aws-sqs-spring-boot)를 참고한다.
  
테스트를 위해서 크롬 [확장프로그램](chrome-extension://aejoelaoggembcahagimdiliamlcdmfm/index.html#requests)을 설치하자.
![image](https://user-images.githubusercontent.com/33191974/139530018-bc1a299e-7928-41ca-ac0f-c23e0f30f52d.png)  
@RequestBody 문자열에는 해당 JSON 문자열이 그대로 들어가게 되고, AWS SQS 콘솔에서 들어가 있는  
메시지를 확인할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/139529997-d3debbb0-b894-49d4-a0c1-98a50fde45f5.png)    
메세지 전송 및 수신을 눌러 다음 페이지로 들어간다.  
![image](https://user-images.githubusercontent.com/33191974/139530684-8aa74795-3d3d-42d7-8fd1-839e1a6d68d9.png)  
메세지 폴링을 누르면 아래 하나의 메시지가 나온다.  
![image](https://user-images.githubusercontent.com/33191974/139530779-142f5af3-0441-4bb9-bbfa-f9e7f8789cb2.png)  
![image](https://user-images.githubusercontent.com/33191974/139530807-798eb6cb-389b-435b-8aa4-9bb20f9ec250.png)    
![image](https://user-images.githubusercontent.com/33191974/139530819-7d36fb73-5c18-4333-8a58-33e4f39f7a6e.png)  
선택 후 삭제하면 Queue에서 제거된다.  
![image](https://user-images.githubusercontent.com/33191974/139530870-a89a44bc-877b-445c-afc0-4bc971e368fa.png)  
![image](https://user-images.githubusercontent.com/33191974/139530880-3584c0b6-f5e9-4670-9b15-e7adf6e408df.png)











































