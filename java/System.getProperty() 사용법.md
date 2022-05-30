# System.getProperty() 사용법  
자바를 실행할 때, 실행되는 곳의 정보를 얻어오거나 운영체제의 정보가 필요할  
때가 있다. 실행 위치에 있는 파일을 읽어들여야 하는데, 현재 위치를 알 수   
있는 방법 등 시스템의 정보를 가져올 때 `System.getProperty()`를 사용한다.   
  
`System.getProperty()`으로 괄호 안에 주어진 특정 문자를 적어넣으면 그 값이  
String으로 출력된다.   
```java
String dir = System.getProperty("user.home");
System.out.println(dir);

//리눅스인 경우 /home/유저명/
//macOS인 경우 /Users/유저명/ 
```

## 전혀 없는 키 만들기   
전혀 없는 키값인데, `System.getProperty("file.location.env");`라면서   
사용하는 경우가 있다. 이럴경우, JVM의 VM arguments 부분을 살펴보면  
다음과 같이 세팅되어 있다.   
```java
System.getProperty("file.location.env");
를 사용하기 위해 JVM의 VM arguments 부분에 설정해주세요
-Dfile.location.env="./fileUploads/"
```
JVM의 VM arguments는 JVM을 구동할 때, 필요한 여러가지 값을 세팅하는 것인데  
-D하고 바로 뒤에 키="값"을 세팅하면 프로그램 내에서 `System.getProperty(  
"키");`를 통해 값을 가져올 수 있다.   













































