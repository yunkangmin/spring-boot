## 1. HashMap.putAll()
이 방법을 사용하면, 같은 Key가 있을 때, value를 덮어 쓴다.  
```java
public class MergeHashMap {
  public static void main(String[] args) {
    
    //Map 1 준비
    Map<String, Integer> map1 = new HashMap<>();
    map1.put("Apple", 1000);
    map1.put("Banana", 2000);
    map1.put("Orange", 3000);
    
    //Map 2 준비
    Map<String, Integer> map2 = new HashMap<>();
    map2.put("Apple", 4000);
    map2.put("Tomato", 5000);
    map2.put("WaterMelon", 6000);
    
    //Map1 + Map2
    map1.putAll(map2);
    
    //결과 출력
    //{Apple=4000, WaterMelon=6000, Tomato=5000, Orange=3000, Banana=2000}
    System.out.println(map1);
  }
}
```
map1.putAll(map2);  
putAll() 메소드를 사용하여 2개의 Map을 합쳤다. 
이 경우, Key값이 같을 경우, 파라미터로 전달된 Map의 값으로 원본 Map의 value가 변경된다.  
위 예제에서는 2개의 Map에 모두 'Apple'이라는 key가 있기 때문에  
'Apple'의 value는 map2의 value인 4000으로 대체되었다.  
