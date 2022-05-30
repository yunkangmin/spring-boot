## \<T extends Class\>

### \<?\>
메소드 매개변수의 자료형에 사용되는 제네릭  
- \<? extends Object\>의 줄임 표현
- 어떤 자료형의 객체도 매개변수도 받겠다는 의미
- Unbounded WildCard라고 알려져 있음

예제  
```
public class WindTest {
  
  public static void main(String[] args) {  
    //List는 인터페이스이기 때문에 ArrayList 생성 후 Upcasting 이용
    List<String> list = new ArrayList();
    list.add("test1");
    list.add("test2");
    list.add("test3");
    
    //Integer 자료형 list2 객체 생성
    List<Integer> list2 = new ArrayList();
    list2.add(1);
    list2.add(2);
    list2.add(new Integer(3));
    
    //Double형 list3 생성
    List<Double> list3 = new ArrayList();
    list3.add(10.1);
    list3.add(11.2);
    list3.add(12.3);
    
    //static 메소드 호출
    printData(list);
    printData(list2);
    printData(list3);
    
    /*
    test 1  test2  test3
    1       2      3
    10.1    11.2   12.3
    */
  }
  
  // 리스트 출력 메소드
  //List 제네릭 자료형이 String, Integer, Double인 것을
  //아래 메서드 매개변수에서 List<?>로 다 받고 있다. 
  public static void printData(List<?> list) {
    for (Object v : list) {
      System.out.println(v);
    }
  }
}
```

### \< T extends 클래스 \>
상속을 이용해서 T의 자료형을 제한함  
- 클래스 선언시 사용하며 인스턴스 생성시 특정 클래스를 상속받은 클래스형만  
  인스턴스 내부에서 사용할 수 있도록 함
- 특정 인터페이스를 구현한 클래스만 사용하려는 경우에도 사용 가능

### \<? extends 클래스 \>
**매개변수**의 자료형을 특정 클래스를 상속받은 클래스로 제한함

#### 예제 
extends의 역할은 자료형의 제한으로 동일하기 때문에  
\<? extedns 클래스 \>의 예제만 다룸
```
//Person 상속X 클래스
class Test {
  String name;
}

//Person 클래스
class Person {
  String name;
}

//Person 상속 Man 클래스
class Man extends Person {
  //생성자
  Man(String name) {
    this.name = name;
  }
  
  //name 반환 메소드
  public String toString() {
    return name.toString();
  }
}

// Person 상속 Woman 클래스
class Woman extends Person {
  Woman(String name) {
    this.name = name;
  }
  
  public String toString() {
    return name.toString();
  }
}

public class WildExtends {
  
  public static void main(String[] args) {
    
    //Person
    List<Person> listP = new ArrayList<Person>();
    listP.add(new Person());
    printData(listP);   //j200210.Person@15db9742
    
    //Man
    List<Man> listM = new ArrayList<Man>();
    listM.add(new Man("이순신"));
    listM.add(new Man("하현우"));
    listM.add(new Man("박효신"));
    printData(listM); //이순신 하현우 박효신
    
    //Woman
    List<Woman> listW = new ArrayList<Woman>();
    listW.add(new Woman("유관순"));
    listW.add(new Woman("백예린"));
    listW.add(new Woman("박정현"));
    printData(listW); //유관순 백예린 박정현
    
    //Test
    List<Test> listT = new ArrayList<Test>();
    listT.add(new Test());
    //printData(listT); -> Person 클래스를 상속받지 않았기 때문에  
    //메소드 호출 불가
  }
  
  //Person 클래스와 그 하위 클래스로 생성된 인스턴스만 매개변수로 전달 가능
  public static void printData(List<? extends Person> list) {
    for (Person obj : list) {
      System.out.println(obj):
    }
  }
}
```

### \<? super 클래스 \>
매개변수의 자료형을 특정 클래스와 그 클래스의 상위 클래스로만 제한함  
※ super는 T에 사용불가

예제  
위의 예제와 유사하며 extends 대신 super 사용  

```
class Person {
  String name;
  
  //기본 생성자
  Person() { 
  }
  
  //생성자 오버로딩
  Person(String name) {
    this.name = name;
  }
  
  public String toString() {
    return name;
  }
}

//Person 상속 Man 클래스
class Man extends Person { 
  // 위의 예제와 동일
}

//Person 상속 Woman 클래스
class Woman extends Person {
  //위의 예제와 동일
}

public class WildSuper {
  
  public static void main(String[] args) {
    
    //Person
    List<Person> listP = new ArrayList<Person>();
    listP.add(new Person("사람"));
    listP.add(new Person("인간"));
    printData(listP); //사람 인간
    
    //Man
    List<Man> listM = new ArrayList<Man>();
    listM.add(new Man("하현우"));
    listM.add(new Man("박효신"));
    printData(listM); //하현우 박효신
    
    //Woman
    List<Woman> listW = new ArrayList<Woman>();
    listW.add(new Woman("백예린"));
    listW.add(new Woman("박정현"));
    //printData(listW); -> Man 클래스의 상위 클래스가 아니기 때문에 메소드 호출 불가
    
  }
  
  //Man 클래스와 그 상위 클래스로 생성된 인스턴스만 매개변수로 전달 가능
  public static void printData(List<?> super Man> list) {
    for (Object obj : list) {
      System.out.println(obj);
    }
  }
}
```
제네릭 <?> 사용방법과 extends, super로 <T>와 <?>의 자료형을 제한하는 방법에 대해 알아보았다. 
<?>는 어떤 자료형도 매개변수로 받을 수 있기 때문에 와일드 카드라고도 불리며  
필요한 경우에 extends와 super를 사용해서 자료형의 범위를 제한할 수 있다.  
<T>의 경우에는 extends는 사용가능하지만 super는 사용할 수 없다는 점을 기억해야한다.  
    



    
    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
  
