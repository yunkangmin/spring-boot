## Scripted 문법 소개
이번 시간에는 젠킨스 파이프라인의 Scripted 문법을 소개한다.  
지난 시간에 말씀드린 것처럼 Scripted 문법은 쉘 스크립트를 짜듯이 자유롭게  
파이프라인을 구성할 수 있도록 지원한다. Scripted 문법은 Groovy 문법을 사용한다.  
Groovy를 안써본 사람이라도 Java나 기타 다른 언어를 써봤다면 쉽게 이해할 수 있다.  
  
### 문법
Scripted 문법에는 별도의 기능을 지원하는 Directive(지시어 정도로 이해하면 된다.)가 있다.  
이런 Directive를 이용해 필요한 파이프라인 작업들을 작성한다.  
아래는 대표적인 Directive들이다.  
- node
   - Scripted 파이프라인을 실행하는 젠킨스 에이전트
   - 최상단 선언 필요
   - 젠킨스 마스터-슬레이브 구조에서는 파라미터로 마스터-슬레이브 정의 가능

- dir
   - 명령을 수행할 디렉토리/폴더 정의

- stage
   - 파이프라인의 각 단계를 애기하며, 이 단계에서 어떤 작업을 실행할지 선언하는 곳  
     (즉, 작업의 본문)
     
- git
   - Git 원격 저장소에서 프로젝트 Clone

- sh
   - Unix 환경에서 실행할 명령어 실행  
     (윈도우에서는 bar)
     
- def
   - Groovy 변수 혹은 함수 선언
   - Javascript의 var같은 존재로 이해

위의 Directive는 Scripted 전용이다.  
즉, Declartive 문법에서는 사용할 수 없다.

이런, Directive를 통해 다음과 같은 포맷으로 스크립트를 작성할 수 있다.

```
node('worker') {
  stage('Source') { //스테이지 정의
    //스테이지에서 수행할 코드 작성
```
Scripted 파이프라인을 쓴다고하면 node는 필수로 선언해야 한다.  
node가 선언되었다면 이후 성격에 맞게 stage를 정의해서 사용하면 된다.
  
Node & Stage 간의 관계는 다음과 같이 볼 수 있다.
![image](https://user-images.githubusercontent.com/33191974/139032480-ffaa7b91-fa1f-496e-94b3-d1a5155705d6.png)  
Scripted 문법의 경우 별도의 Step 단계를 두고 있진 않다.  
Stage 안에서 실행되는 흐름을 Step이라고 보면 된다.  

### 사용법
Scripted 문법은 2가지 방식으로 사용할 수 있다.

#### 정식 흐름
웨에서 설명한 Directive로 샘플 스크립트를 작성해보자.  
파이프라인 Item을 새로 만들고 아래와 같이 코드를 작성해보자.   

```
node {
    def hello = 'Hello jojoldu' //변수 선언
    stage ('clone') {
      git 'https://github.com/jojoldu/jenkins-pipeline.git' //git clone
    }
    dir ('sample') { //clone 받은 프로젝트 안의 sample 디렉토리에서 stage 실행
      stage ('sample/execute') {
          sh './execute.sh'
      }
    }
    stage ('print') {
      printe(hello) //함수 + 변수 사용
    }
}

//함수 선언(반환 타입이 없기 때문에 void로 선언, 있다면 def로 선언하면 됨)
void print(message) {
  echo "${message}"
}
```
각 명령어는 다음과 같은 내용을 가진다.
- def로 hello 변수를 선언했다.
- stage에서는 clone, sample, print라는 작업을 선언했다.
- dir로 clone받은 프로젝트의 sample 디렉토리에서 명령어 수행을 지시한다.
   - sh로 sh 명령어를 수행한다.
- print는 def로 선언한 메소드이다.

위 코드를 복사해서 실행해보면 아래처럼 정상적으로 스크립트가 시작되는 것을 확인할 수 있다.

파이프라인 메뉴로 가보면 다음과 같이 정상적으로 각 Stage가 생성된 것을 알 수 있다.
![image](https://user-images.githubusercontent.com/33191974/139034061-57048dc7-b083-4f91-8153-96efbf49c703.png)  
일반적인 파이프라인의 경우 이처럼 처리가 가능하다.  

### 예외 흐름
위와 같이 정상적인 명령 흐름 외에, 예외 처리가 가능하다.  
파이프라인은 Groovy 기반이기 때문에 Groovy의 예외처리가 가능하다.  
특정 단계에서 어떤 이유로 실패할 경우 Exception을 던진다.  
이런 오류를 try/catch/finally로 잡아 별도의 처리를 할 수도 있다.
  
예를 들면 아래와 같은 코드이다.
```
node {
    stage('Example') {
      try {
          sh 'exit 1'
      }
      catch (exc) {
        echo 'Something failed, I should sound the klaxons!'
        throw
      }
    }
```
stage Example에서 sh 'exit 1' 명령어 수행시 오류가 발생할 경우 try catch로 예외를 잡아  
echo 'Something failed, I should sound the klaxons!'를 수행한다.  
  
일반적인 프로그래밍 언어에서의 예외처리와 같은 방식으로 처리가 가능한 것을 알 수 있다. 

  
































   
