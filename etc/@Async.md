# @Async 어노테이션을 사용해서 비동기 서비스 만들기   
일단 아주 간단한 springboot 웹 프로젝트를 생성했다.  
  
spring에서 Async, 즉 비동기 기능을 사용하는 방법은 아주 간단하다.   
- @EnableAsync로 비동기 기능을 활성화   
- 비동기로 동작을 원하는 메소드(public 메소드)에 @Async 어노테이션을 붙여준다.  
간단하게 위 두가지 행위로 spring에서 비동기 기능을 구현할 수 있다.   

# springboot에서 Async 비동기 기능 활성화 및 thread pool 설정  
AsyncConfig.java라는 java config 파일을 추가했다.  
```java
@Configuration
//Spring의 메소드의 비동기 기능을 활성화해준다.  
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
  
  @Override
  public Executor getAsyncExecutor() {
    //ThreadPoolTaskExecutor로 비동기로 호출하는 Thread에 대한 설정을 한다.  
    ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
    //corePoolSize: 기본적으로 실행을 대기하고 있는 Thread의 개수
    executor.setCorePoolSize(2);
    //MaxPoolSize: 동시 동작하는, 최대 Thread 개수
    executor.setMaxPoolSize(10);
    //QueueCapacity: MaxPoolSize를 초과하는 요청이 Thread 생성 요청 시   
    //해당 내용을 Queue에 저장하게 되고, 사용할 수 있는 Thread 여유 자리가  
    //발생하면 하나씩 꺼내져서 동작하게 된다.   
    executor.setQueueCapacity(500);
    //spring이 생성하는 쓰레드의 접두사를 지정한다.  
    executor.setThreadNamePrefix("hahumoka-async");
    executor.initialize();
    return executor;
  }
}
```
이제 비동기로 동작할 메소드를 갖는 Service를 생성해보자.   

# @Async를 적용한 Service 만들기   
AsyncService.java 소스 코드   
```java
@Service
public class AsyncService {
  
  ...
  
  //비동기로 동작하는 메소드  
  @Async
  public void onAsync() {
    try {
      Thread.sleep(5000);
      logger.info("onAsync");
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }
  
  //동기로 동작하는 메소드
  public void onSync() {
    try {
      Thread.sleep(5000);
      logger.info("onSync");
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }
}
```
Service의 비동기로 호출할 메소드 위에 @Async 어노테이션을 달아주면 해당  
메소드는 호출 시 비동기로 동작하게 된다. 따라서 해당 메소드를 호출한  
Thread(예를 들어 Controller)는 논블록킹으로 동작하게 된다.  
  
**주의할 점은 private 메소드는 @Async를 적용해도 비동기로 동작하지 않으며  
반드시 public 메소드에 @Async를 적용해야 한다.**     




















