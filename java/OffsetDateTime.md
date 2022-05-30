# OffsetDateTime  
LocalDateTime + ZoneOffset에 대한 정보까지 포함된 API이다.   
이러한 경우는 축구 경기 생중계 등등에 적합하다.   
   
레알마드리드와 바르셀로나의 경기인 엘 클라시코 더비의 경우를 살펴보자.     
<img src="https://user-images.githubusercontent.com/33191974/161967491-745ee84d-7ec8-437b-949a-e8ebaff09ad0.png" width="50%" height="50%"/>    
바르셀로나 홈 구장인 Camp Nou(바르셀로나에 위치)에서 2018-05-06T20:45:00  
+02:00에 경기가 시작했다.   
똑같은 경기를 한국 사람이 보려면 2018-05-07T03:45:00 +09:00에 경기가  
시작했다.  
```java
import java.time.OffsetDateTime;
import java.time.LocalDateTime;
import java.time.ZoneOffset;
import java.time.ZoneId;

public class OffsetTimeTest {
  public static void main(String[] args) {
    final var barca = OffsetDateTime.of(LocalDateTime.of(2018, 5, 6, 20, 45, 0), ZoneOffset.of("+2"));
    // 2018-05-06T20:45+02:00
    System.out.println(barca);
    
    final var seoul = OffsetDateTime.of(LocalDateTime.of(2018, 5, 7, 3, 45, 0), ZoneOffset.of("+9"));
    // 2018-05-07T03:45+09:00
    System.out.println(seoul);
    
    //둘을 UTC로 변환했을 때 같은 시간이기 때문에 둘은 같은 시간이라고 볼  
    //수 있다.  
    //2018-05-06T18:45Z, Zulu Time -> 딱 UTC
    System.out.println(barca.atZoneSameInstant(ZoneId.of("Z")));
    //2018-05-06T18:45Z
    System.out.println(seoul.atZoneSameInstant(ZoneId.of("Z)));
    
    //1970-01-01T00:00Z
    final var unixTimeOfUTC = OffsetDateTime.of(1970, 1, 1, 0, 0, 0, 0, ZoneOffset.UTC);
    //1970-01-01T00:00+09:00
    final var unixTimeOfUTC9 = OffsetDateTime.of(1970, 1, 1, 0, 0, 0, 0, ZoneOffset.of("+9"));
    // false, 둘은 다른 ZoneOffset을 가진다.   
    System.out.println(unixTimeOfUTC.equals(unixTimeOfUTC9));
    
    //1970-01-01T00:00
    final var unixTimeOfUTCLocalDateTime = unixTimeOfUTC.toLocalDateTime();
    //1970-01-01T00:00
    final var unixTimeOfUTCLocalDateTime = unixTimeOfUTC9.toLocalDateTime();
    //true, LocalDateTime은 ZoneOffset이 없기 때문에 둘 다 똑같은 걸로  
    //취급한다.  
    System.out.println(unixTimeOfUTCLocalDateTime.equals(unixTimeOfUTCLocalDateTime));
}    
```

---

# ZoneOffset
UTC 기준으로 시간(Time Offset)을 나타낸 것이라고 보면 된다.  
**우리나라는 KST를 사용하는데 KST는 UTC보다 9시간이 빠르므로 UTC +09:00로  
표기한다.**  
ZoneOffset은 ZoneId의 자식 클래스이다.  
```java
import java.time.ZoneOffset;
import java.time.ZoneId;

public class ZoneOffsetTest {
  public static void main(String[] args) {
    //UTC +09:00은 아래와 같이 나타낼 수 있다.  
    final var zoneOffset = ZoneOffset.of("+9");
    final var zoneOffset2 = ZoneOffset.of("+09");
    final var zoneOffsetIso8601Format = ZoneOffset.of("+09:00");
    final var zoneOffset3 = ZoneOffset.of("+09:00:00");
    final var zoneOffset4 = ZoneId.of("+9");
    final var zoneOffset5 = ZoneId.of("+09");
    final var zoneOffsetIso8601Format2 = ZoneId.of("+09:00");
    final var zoneOffset6 = ZoneId.of("+09:00:00");
    
    // UTC +-00:00은 아래와 같이 나타낼 수 있다.   
    final var zoneOffset7 = ZoneOffset.of("+0");
    final var zoneOffset8 = ZoneOffset.of("-0");
    ...

```



---

# UTC란?   
컴퓨터나 스마트폰의 시간 설정에서 다음 사진과 같은 UTC라는 용어가 나온다.  
서울은 UTC+09:00이다.  
<img src="https://user-images.githubusercontent.com/33191974/161968181-44454e9e-f78b-44b9-a0b7-52e227afb305.png" width="50%" height="50%"/>  
UTC를 우리말로는 대개 '협정세계시'라고 번역하는데, 영어로는 Coordinated  
Universal Time이다.     
  
그냥 UTC를 아주 단순하게 설명하자면, "영국을 기준(UTC+0:00)으로 각 지역의  
시차를 규정한 것이다. 한국은 영국보다 9시간 빠르므로 UTC+09:00이라고 표시  
한다. 미국 뉴욕은 영국보다 5시간 느리므로 UTC-5:00라고 표시한다" 정도   
된다. 그냥 일상생활에서 UTC를 이해하려면 여기까지만 알아도 된다.   
  































