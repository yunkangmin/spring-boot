Redis에서 Keys 명령어는 성능상으로 문제가 있다.  
Redis의 One Thread 정책으로 인해서 해당 작업을 처리하기 위해서 서버가 멈춰버린다.  
그래서 이를 대안하기 위해서 Redis의 Scan이라는 기능을 사용할 수 있다.  
  
Scan은 cusor를 기반으로 동작하는 Itorator이다.  
처음 시작은 scan 번호를 0으로 지정해서 시작한다.  
  
```redis
> scan 0
1) "9"
2) 1) "kidfk"
   2) "kdfjkd"
   3) "wedul"
   4) "dkfijdk"
   5) "cjung"
   6) "123"
   7) "fkdf"
   8) "user"
   9) "cellphone"
  10) "counter"
```
  
그러면 두 개의 값을 반환을 하는데 첫번째 값은 다음 cursor의 번호이고 그 다음 값은 키 값들이 출력된다.  
다음 데이터를 찾기위해서는 1번 값에서 반환된 커서 값을 이용해서 검색하면 된다. (scan 9)  
그리고 scan 후 나온 다음 cursor의 값이 0인 경우 그 이후에 값이 없음을 의미한다.  
  
### Option
scan은 모든 반복에서 반환되는 요소의 값의 크기를 제한할 수 있다.  
바로 count option을 사용하는 것이다.  
사용방법은  단순하게 scan 0 count [count 크기]와 같은 방식으로 사용할 수 있다.  

```redis
>scan 0 count 2
1) "8"
2) 1) "kldfk"
   2) "kdfjkd"
> scan 8 count 2
1) "24"
2) 1) "wedul"
   2) "dkfjdk"
> scan 24 count 5
1) "14"
2) 1) "cjung"
   2) "123"
   3) "fkdf"
   4) "user"
   5) "cellphone"
```
  
### Match Option
match options은 keys [패턴] 명령어에서 사용하는 방식과 비슷하며 패턴은 glob-style 패턴만을 지원한다.  
(glob-style 패턴은 \*(와일드 카드), ?(조커)) match는 검색할 때 where 조건 처럼 필터해서 가지고 오는  
방식이 아니라 모든 데이터를 가지고 오고 사용자에게 반환하기 직전에 값을 필터해서 보여준다.  

```
> scan 0 match *jung*
1) "9"
2) 1) "cjung"
```

