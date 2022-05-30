# 1. @PostConstruct란?   
의존성 주입이 이루어진 후 초기화를 수행하는 메서드이다. @PostConstruct가   
붙은 메서드는 클래스가 service(로직을 탈 때?로 생각됨)를 수행하기 전에   
발생한다. 이 메서드는 다른 리소스에서 호출되지 않는다해도 수행된다.   

---

다음의 서비스 클래스가 있다.   
```java
@Service
public class test {
    Map<String, Object> map;

    public void action() {
        System.out.println(map.get("key"));
    }
}
```
action을 바로 실행하면 객체가 생성되지 않았기 때문에 에러가 난다.   
그래서 @PostConstruct를 이용하면 된다.   
```java
@Service
public class test {
    Map<String, Object> map;

    @PostConstruct
    public void init() {
        map = new HashMap<String, Object>();
        map.put("key", "test");
    }

    public void action() {
        System.out.println(map.get("key"));
    }
}
```
그럼 Autowired를 이용해 외부의 의존을 가져야할 때에는 어떻게 될까?   
예를 들어 아래와 같은 상황   
```java
@Service
public class test {
    Map<String, Object> map;

    @Autowired
    private Test2Service test2Service;

    @PostConstruct
    public void init() {
        map = new HashMap<String, Object>();
        map.put("key", test2Service);
    }

    public void action() {
        map.get("key");
    }
}
```
autowired가 먼저 생성된 후에 init 메서드가 실행된다.  





























