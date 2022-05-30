모든 코드는 [여기](https://github.com/lannstark/aws-sqs-spring-boot)있다.  
SQS로부터 메시지를 받는 두번째 방법은 바로 Annotation을 이용하는 방법이다.  
아래의 PersonListener 코드를 보자.  
```
@Slf4j
//Bean 등록을 꼭 해줘야 한다.
@Component
public class PersonListener {
  
  //deletionPolicy(삭제 정책)
  @SqsListener(value = "sqs-study", deletionPolicy = SqsMessageDeletionPolicy.NEVER) 
  //acknowledgment(승인)
  public void listen(@Payload Person person, @Headers Map<String, String> headers, Acknowledgment ack) {
    log.info("{}", person);
    log.info("{}", headers);
    log.info("{}", ack != null);
  }
}
```
@SqsListener 어노테이션의 value는 만들었던 SQS의 이름이 들어간다.   
deletionPolicy는 메시지를 받은 이후의 삭제 정책인데, 4가지 경우의 옵션이 있다.  
```
public enum SqsMessageDeletionPolicy {
  //메소드로 Message가 들어오면 무조건 삭제요청을 보낸다. 
  ALWAYS,
  
  //절대 삭제 요청을 보내지 않는다.
  //Acknowledgment 파라미터를 바인딩 받아 ack 메서드를 호출할 때 삭제 요청을 보낸다.
  NEVER,
  
  //redrive(재구동) policy(DLQ)가 정의되지 않으면 메시지를 삭제한다.
  NO_REDRIVE,
  
  //SqsListener 어노테이션이 붙은 메소드에서 에러가 나지 않으면 메시지를 삭제한다. 
  //에러가 난 경우에는 메시지를 삭제하지 않는다.
  ON_SUCCESS
}
```
개인적인 생각으로는 NEVER와 ON_SUCCESS 중에 적절한 것을 사용하면 좋을 것 같은데  
NEVER를 사용하고 Acknowledgment를 쓰는게 조금 더 끌린다.  
메소드를 만들었으니 바로 테스트를 해서, 메시지가 잘 들어오는지 로그를 확인해보자.  
기존의 Controller와 Service도 약간 수정했다. 메시지가 전달되면 @SqsListener로 잘 들어오는지  
테스트 할 예정이다.  
```
//Controller
@PostMapping("/message")
public void sendMessage(@RequestBody Person person) {
  messageService.sendMessage(person);
}

//Service
public void sendMessage(Person person) {
  log.info("SQS에 Person을 전달합니다 : " + person);
  queueMessagingTemplate.convertAndSend("sqs-study", person);
}
```
```
# log-info에 timestamp를 찍기위해 추가
logging:
  pattern:
    console: '%d{yyyy-MM-dd HH:mm:ss} %-5level --- [%thread] %logger(35) : %msg %n'
```
메시지를 보내고 받아보자.  
![image](https://user-images.githubusercontent.com/33191974/139573220-52825c91-cf4d-4c17-8dec-b5b85a53c7c7.png)  
SqsListener를 사용하면 get 요청을 안해도 자동으로 메시지를 받는다.  
  
API를 호출해보면 console에 이렇게 찍힌다.    
![image](https://user-images.githubusercontent.com/33191974/139573304-aa5da3e5-fedf-4a7a-b006-6f1f1240f7ba.png)
30초마다 로그가 반복된다.  
DELETE를 해주지 않았기 때문에 Queue에 메시지가 남아 있고 DLQ 설정도 되어 있지 않기 때문이다.  
Header로는 다음과 같은 정보들이 넘어왔다.  
```
SentTimestamp=1635666419074,
ReceiptHandle=AQEBc5xVrI5kdtm0yjfSfT16nXdyjVrRcYzAGE8bIkXuUbKEvHD6CTbmbSqXL3sCunNVaPr+3FQN6VE72cVrM1+lNASGc+NWVaSyNGHRWbbfgPSdTV/+EAq8mLATWWSrK1fq8GFSr8mn3ilDZIfQifxRBSD1gGcRyQmaLKnX0FJRo6VNtDteKV/DFs3RFpvFlUaIpsYr3p/ThB8c18xi1vd6+slcNWhPbozBAEknjPHxiLWEcECsm98iOmlsCT064bKbE6V8cdKD8J3TvYPlRnugThHJUrMNwf0Z0i5e7a/DNSW0dtWXbqY2jhcZIJiy9AXvD3aq2Hacz3TNIJgw9N968BQwfgKxtUnTY8XcjK/CABLQsIS12Zm/MJatTWVVkNN5t7x/X1gBVhRix0JgUM3QRw==,
SenderId=AIDA5K5N3XSUGYS2C4TXL,
LogicalResourceId=sqs-study,
ApproximateReceiveCount=1,
Acknowledgment=org.springframework.cloud.aws.messaging.listener.QueueMessageAcknowledgment@558e6e09,
Visibility=org.springframework.cloud.aws.messaging.listener.QueueMessageVisibility@45a3b60b,
contentType=application/json,
lookupDestination=sqs-study,
ApproximateFirstReceiveTimestamp=1635666419080,
MessageId=ca0f3628-8709-4c8c-8c4b-437242eac2ec
```
- SentTimestamp : 메시지가 전송된 timestamp
- ReceiptHandle : 메시지에 대해 응답할 때 사용될 것 같은 문자열
- SenderId : 보내는 주체를 특정한 문자열
- LogicalResourcedId : Queue의 이름
- ApproximateReceiveCount : 대략적으로 이 메시지를 몇 번째로 받았는지(숫자가 계속 증가한다.)
- Acknowledgment : ack(한 컴퓨터가 일련의 데이터를 네트웍을 통해 다른 컴퓨터로 보낼 때,  
  만약 그 데이터 전송이 성공적이었다면 수신측 컴퓨터가 ACK 코드 값을 송신측에 되돌려준다.  
  그러나, 만약 그 전송에서 에러가 발생하였다면 이때에는 negative ACK를 의미하는 코드,   
  즉 NAK 코드 값이 되돌려진다. 일반적으로, 송신측 컴퓨터가 일정시간동안 ACK 코드 값을 받지 못했거나,  
  또는 NAK 코드 값을 받은 경우에는 원래의 데이터가 재송신된다.)를 위한 spring-cloud-aws의 구현체
- Visibility : 이 메시지의 visibility timeout을 설정하기 위한 spring-cloud-aws의 구현체 
- contentType : application/json
- lookupDestination(조회대상) : sqs-study
- Approximate(대략적)FirtstReceiveTimestamp : 대략적으로 이 메시지를 첫 번째 받았을 때의 timestamp
- MessageId

Visibility timeout이란 그 메시지가 특정 소비자에게 전달된 이후로, 다른 소비자가 가져가지 못하는 시간을  
의미한다.  
![image](https://user-images.githubusercontent.com/33191974/139573723-48a8d834-6506-4eec-8b46-81d9b2270315.png)  
메시지 받기가 30초마다 반복되는 이유는 폴링 기간이 30으로 설정되어 있기 때문이다.  
![image](https://user-images.githubusercontent.com/33191974/139573776-da186d44-aa97-4902-8e70-8eab9fdf8c5e.png)  
메시지 수신 정보를 보면, 다음과 같은 설명이 되어있다.  
긴 폴링(long polling)의 경우, 폴링 기간 값을 0보다 크게 설정한다.  
긴 폴링은 빈 응답수와 잘못된 응답을 제거하여 Amazon SQS 사용 비용을 줄이는데 도움이 된다.  
긴 폴링은 다음과 같은 이점을 제공한다.  
- 응답을 전송하기 전에 대기열에서 메시지를 사용할 수 있을 때까지 Amazon SQS가 대기하도록 하여  
  빈 응답을 제거한다. 
- 하위 집합이 아닌 모든 Amazon SQS 서버를 쿼리하여 잘못된 빈 응답을 제거한다.  

이 두 번째 문장의 의미는 SQS의 원리를 알아야 이해가 되는데, SQS는 내부적으로 메시지를 분산해서  
저장한다고 한다. 이 때 메시지를 요청하는 짧은 폴링(short polling)의 경우는 분산된 모든 서버에서  
메시지를 받아오는게 아니라, 특정 몇 개의 분산서버에서만 메시지를 받아올 수 있다.  
만약 메시지가 5개가 있고 2군데의 분산 서버에 몰빵(?)되어 있는데 다른 분산 서버를 short polling으로  
찌르면 '잘못된 빈 응답'이 나올 수 있는 것이다.  
![image](https://user-images.githubusercontent.com/33191974/139574040-aa91ec59-fe50-4c5d-9624-a7c159701c64.png)  
긴 폴링은 이런 상황을 막아준다고 한다.  
이제 ack를 해보자.
```
@SqsListener(value = "sqs-study", deletionPolicy = SqsMessageDeletionPolicy.NEVER)
public void listen(@Payload Person person, @Headers Map<String, String> headers, Acknowledgment ack) {
  log.info("{}", person);
  ack.acknowledge();
}
```
이렇게 하고 application을 재구동하니 한 번의 수신 이후 다시 로그가 나오지 않는다.  
받는 방법 자체는 끝났지만, 내부 동작 원리에 대해서는 생각해볼거리가 한 가지 있다.  

### Thread
로그를 통해 알 수 있는 한 가지 흥미로운 점은 thread가 Spring Boot Tomcat thread가 아닌  
SimpleMessageListenerContainer의 스레드란 점이다. 이 말은 메시지가 N개가 들어올 때 여러 스레드가  
생성될 수 있다는 것이다.  
ack를 안주고 sleep을 걸어보자.  
메시지 2개를 Queue에 미리 넣어놓고 테스트를 해보자.  
50초를 sleep하도록 설정한다.   
```
@SqsListener(value = "sqs-study", deletionPolicy = SqsMessageDeletionPolicy.NEVER)
public void listen(@Payload Person person) throws Exception {
  log.info("{}", person);
  Thread.sleep(50 * 1000);
}
```
결과는 아래와 같이 나왔다.  
![image](https://user-images.githubusercontent.com/33191974/139574585-aa700c62-39ff-4e7b-828c-eb9866ead3f9.png)
메시지를 받아 잠들었고, PersonListener의 listen은 동시에 호출되었다.  
메시지를 3개이상 넣어 놓고 해봐도 로그는 동일하다. 설정된 최대 max thread 개수만큼만 동시에  
처리할 수 있도록 되어 있다.(SqsListener가 메세지를 처리하는 기본 thread 수는 2개이다.)  

### AbstractMessageListenerContainer
@SqsListener 어노테이션 역시 내부적으로 폴링을 하고 있는데 관련 코드는 AbstractMessageListenerCon  
tainer와 그 구현체인 SimpleMessageListenerContainer에서 살짝 볼 수 있다.  
몇 가지 알고 있으면 좋은 사항들을 나열해보자면  
- 메시지를 처리하는 기본 thread 개수는 2개이다. 때문에 위의 테스트에서 최대 2개의 thread만  
  활용되었다.
- Long polling 방식을 기본적으로 사용하고 있으며, 폴링 요청 이후 메시지를 대기하는 시간은  
  최대값인 20초를 쓰고 있다. 공식 문서에는 최대값이 20초라고 되어 있는데 정작 메시지 수신 정보에서는  
  0 ~ 999로 되어있다.(큐 폴링 설정과 SqsListener 폴링 설정이 각각 달라서 인듯 여기서는 큐 폴링 설정을  
  이야기하는 것 같다.)  
![image](https://user-images.githubusercontent.com/33191974/139574974-28844d91-7018-40a1-8cf3-431d384e7c0d.png)
  





















































