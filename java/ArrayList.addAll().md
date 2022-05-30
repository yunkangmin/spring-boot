# ArrayList.addAll()   
ArrayList의 addAll() 메소드는 인자로 전달되는 Collection 객체의 모든  
아이템을 리스트에 추가한다.   
  
인자가 다른 두 개의 메소드가 있다.  
- ArrayList.addAll(Collection c) 
- ArrayList.addAll(int index, Collection c) 

## 1. ArrayList.addAll(Collection c) 
인자로 Collection 객체를 받고 그 Collection에 있는 아이템들을 리스트에   
모두 추가한다.   
```java
public boolean addAll(Collection<? extends E> c) 
```
### 1-1. ArrayList.addAll(Collection c) 예제   
아래 코드는 marvel 리스트를 movie 리스트에 모두 추가하는 예제이다.  
addAll()에 인자로 Collection을 넘기면 그 것의 아이템 모두 리스트에   
추가된다.   
```java
ArrayList<String> marvel = new ArrayList<>();
marvel.add("Iron man");
marvel.add("Hulk");
marvel.add("Captain america");
System.out.println("marvel: " + marvel.toString());
```
결과   
```java
marvel: [Iron man, Hulk, Captain america]
movies: []
movies: [Iron man, Hulk, Captain america]
```

## 2. ArrayList.addAll(int index, Collection c)   
Collection의 아이템들을 모두 추가한다는 점에서 위와 동일하지만, 리스트의  
몇 번째 인덱스부터 아이템을 추가할 수 있는지 정할 수 있다. 인자로 인덱  
스가 전달되는데, 이 인덱스부터 아이템에 추가된다.   
```java
public boolean addAll(int index, Collection<? extends E> c)  
```

### 2-1. ArrayList.addAll(int index, Collection c) 예제   
위의 예제와 거의 비슷한데, 이 번엔 movies 리스트에 아이템이 3개가 추가  
되었다. `movies.addAll(2, marvel)`는 movies의 2번째 인덱스부터 marvel  
리스트를 추가한다.   
```java
ArrayList<String> marvel = new ArrayList<>();
marvel.add("Iron man");
marvel.add("Hulk");
marvel.add("Captain america");
System.out.println("marvel: " + marvel.toString());

ArrayList<String> movies = new ArrayList<>();
movies.add("Untouchable");
movies.add("Spiderman");
movies.add("Untouchable");
System.out.println("movies: " + movies.toString());

movies.addAll(2, marvel);
System.out.println("movies: " + movies.toString());
```
결과   
```
marvel: [Iron man, Hulk, Captain america]
movies: [Untouchable, Spiderman, Greenbook]
movies: [Untouchable, Spiderman, Iron man, Hulk, Captain america]
```
























