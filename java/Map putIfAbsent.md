# putIfAbsent(Absent -> 결석한, 없는)  
key 값이 존재하는 경우 Map의 Value의 값을 반환하고, Key 값이 존재하지   
않는 경우 Key와 Value를 Map에 저장하고 Null을 반환한다.  

## 사용방법
```java
default V putIfAbsent(K key, V value)
```
s
## 매개변수  
- key - 지정된 값이 연관될 키
- value - 지정된 키와 연결될 값

## 반환값   
- key 값이 존재하는 경우 -> Map의 value 값을 반환  
- key 값이 존재하지 않는 경우 -> key와 value를 Map에 저장하고 null을 반환  

## 기본구현  
```java
V v = map.get(key);
if (v == null)
    v = map.put(key, value);
return v;
```

## 예제
```java
package testProject;

import java.util.HashMap;

public class putIfAbsentTest {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        map.put("A", 1);
        map.put("B", 1);
        map.put("C", 1);
        map.put("D", null);

        //result: {A=1, B=1, C=1, D=null}
        System.out.println("result : " + map.toString());

        //result1 : {A=1, B=1, C=1, D=null, E=1}
        map.putIfAbsent("E", 1);
        System.out.println("result1 : " + map);

        //result2 : {A=1, B=1, C=1, D=null, E=1}
        map.putIfAbsent("E", 2);
        System.out.println("result2 :" + map);

        //result3 : {A=1, B=1, C=1, D=1, E=1}
        map.putIfAbsent("D", 1);
        System.out.println("result3 : " + map);
    }
}
```














