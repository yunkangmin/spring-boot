## gradle init 명령과 type 종류
Gradle 프로젝트를 만든 gradle init 명령에 대해 설명한다.  
  
이 것은 "init"이라는 작업을 수행하는 것이다.  
Gradle은 수행할 작업은 "테스트(task)"라고 한다.  
gradle 명령은 이 테스트를 지정하고 실행하는 것이다.  
  
init 테스트는 폴더에 프로젝트 파일이나 폴더들을 생성하고 폴더를 초기화한다.  
이전 페이지에서는 --type 옵션을 넣었다.  
이 옵션에 의해 "어떤 종류의 프로그램 작성을 위한 프로젝트를 초기화하는지"를 지정할 수 있다.  
이 유형은 조금씩 증가하고 있다.

### java-application
Java 어플리케이션 프로젝트 작성에 대한 타입이다.  
기본적으로 App.java가 제공된다.

### java-library
Java 라이브러리 프로젝트 생성을 위한 타입이다.  
단순히 샘플로 제공되는 소스 코드 파일이 응용 프로그램의 메인 클래스가 되어 있지 않다는 정도의 차이이다.  

Java 프로그래머는 java-application, java-library만 알고 있으면 충분하다.

## build.gradle 내용 및 플러그인
이 전에 Gradle으로 "테스크"라는 것을 지정하고 다양한 작업을 수행했었다.  
이러한 Gradle 명령으로 수행하는 처리는 "빌드 파일"이라는 파일에 작성된 내용을 바탕으로 실행된다.  
  
그럼 빌드 파일에 어떤 처리가 적혀있는 것인지 build.gradle 파일의 내용을 보자.

```gradle
apply plugin: 'java'
apply plugin: 'application'

repositories {
   jcenter()
}
 
dependencies { 
    compile 'com.google.guava:guava:22.0'
    testCompile 'junit:junit:4.12'
}

mainClassName = 'App'
```

build.gradle은 Groovy로 작성되었다. Groovy는 Java와 마찬가지로 //와 /* \*/로 주석을 입력할 수 있다.  

### java 플러그인 추가

```gradle
apply plugin: 'java'
```

처음에 apply plugin: 라는 것은 Gradle 플러그인을 사용하기 위한 것이다.

java는 Java 프로그램을 위한 기능을 제공하는 플러그인이다.  
이 전에 compileJava라는 테스크를 사용했는데 이것은 사실 java 플러그인에서 제공하는 것이다.  

### application 플러그인 추가

```
apply plugin: 'application;
```

또 다른 플러그인이 추가되어 있다.  
이 application은 응용 프로그램에 대한 기능을 제공하는 플러그인이다.  
앞에서 run 응요 프로그램을 실행했는데, 이 것이 application 플러그인에 의해 제공되는 테스트이다. 

### 메인 클래스 이름

```gradle
mainClassName = 'App'
```

도중에 조금 넘어와서 마지막에 있는 mainClassName 값을 보자.   
이 것은 application 플러그인으로 사용되는 것으로, 메인 클래스를 지정한다.  
run으로 응용 프로그램을 실행할 수 있었던 것도 이 mainClassName 메인 클래스가 지정되어 있었기  
때문이다.  
  
### java plugin
프로젝트에서 java plugin을 사용하려면 build.gradle 파일에 다음과 같이 설정하면 된다.  

```gradle
apply plugin: 'java'
```
