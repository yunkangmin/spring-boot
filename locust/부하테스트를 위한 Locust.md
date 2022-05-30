백엔드 개발을 하다보면 많이 사용되는 도구 중의 하나가 부하 테스트 툴인데,  
대표적인 도구로는 Apache Jmeter, nGrinder, SOAP UI등의 도구가 있지만 다소  
사용이 어렵고 스케일링을 하는데 어려움이 있는데, locust라는 도구는 설치와  
사용이 편리하고, 테스트 시나리오를 파이썬 스크립트로 작성을 하기 때문에 다양한  
시나리오 구현이 가능하다. 특히 쿠버네티스에 쉽게 배포할 수 있도록 Helm으로   
패키지화가 되어 있기 때문에 필요한 경우 대규모 부하테스트 환경을 설치하고  
테스트가 끝나면 쉽게 지워버릴수 있다.  
(참고 : locust는 영어로 메뚜기라는 뜻인데, 부하를 주는 것을 swarming이라고  
표현하는게 메뚜기떼로 부하를 비교한 것이 재미있다.)

## 퀵스타트
간단하게 설치 및 테스트를 해보자

## 설치
설치는 파이썬만 설치되어 있으면 간단하게 pip 명령을 이용해서 설치할 수 있다.  
```
%python -m pip install locust
```
명령으로 설치하면 된다.  
설치 확인은 아래 명령으로 확인하면 된다.  
```
locust --version
```
이 방식이외에도, 이미 빌드된 도커 이미지를 사용해서 테스트를 하거나 또는 쿠버네티스에서  
Helm으로 설치해서 실행하는 방법도 가능하다. 쿠버네티스에서 Helm을 사용하는 방법은  
나중에 자세하게 다루도록 한다.  

## 간단한 부하 스크립트 생성
locust 부하테스트는 파이썬 스크립트를 실행하는 방식으로 하기 때문에, 부하 테스트용 스크립트  
파일을 작성해야 한다. 아래는 대상 호스트의 /에 HTTP GET으로 요청을 보내는 스크립트이다.  
스크립트 작성방법은 후에 다시 설명하도록 한다.  
```
from locust import HttpLocust, TaskSet, task, between
class MyTaskSet(TaskSet):
  @task
  def index(self):
    self.client.get("/")
class MyLocus(HttpLocust)
  task_set = MyTaskSet
  wait_time = between(3, 5)
```

## 실행하기
locust 실행은 locust -f {스크립트 파일명} {웹 포트번호} 형식으로 실행하면 된다.  
locust 서버가 기동되고, 부하를 줄 준비가 된 상태가 된다.  
실제 부하 생성은 locust 웹 콘솔에 접속해서 해야 한다.  
아래 스크립트는 8080포트로 locust 웹 클라이언트를 기동하도록 한 명령이다.  
```
%locust -f ./locustfile.py --port 8080
```
http://localhost:8080으로 접속하면 아래와 같이 웹 콘솔이 나온다.  
![image](https://user-images.githubusercontent.com/33191974/144750067-f5f69fdc-96b8-44fa-bd6c-2399ef3f8e94.png)  
첫번째 인자는, 몇 개의 클라이언트를 사용할 것인지(메뚜기를 몇마리 만들것인지)를 정의하고  
두번째는 Hatch rate라고 해서, 초마다 늘어나는 클라이언트 수이다.  
처음에는 클라이언트가 1대로 시작되서, 위의 설정의 경우 초마다 하나씩 최대 30개까지 늘어난다.  
그리고 마지막은 테스트할 웹 사이트 주소를 입력한다.  
  
Start Swarming 버튼을 누르게 되면 부하테스트가 시작되서 아래와 같이 부하 테스트 화면이 실시간으로  
출력된다.  
![image](https://user-images.githubusercontent.com/33191974/144750181-5d50da0f-8b50-4251-8ea4-6a06a5c6fee5.png)  
맨 위의 화면은 Total Request Per Second로, 초당 처리량이고, 두번째는 응답시간, 그리고 마지막은  
현재 클라이언트 수를 모니터링 해준다.  

## No Web
위의 부하테스트는 웹 콘솔을 열어서, 웹 사이트로 부하를 주는 시나리오인데, 웹 부하 테스트가 아니라  
데이터베이스의 부하테스트와 같은 경우에는 웹 사이트의 주소를 정의할 수 없다. 
이런 경우에는 CLI로 실행이 가능한데 --no-web 옵션을 주면 된다.  
```
%locust -f ./nested-locust.py --no-web -c 2 -f 1
```
위의 명령은 nested-locust.py 파일을 실행하되, --no-web으로 웹으로 부하를 주지 않고,  
-c 2로 2개의 클라이언트에 대해서 -r 1 1초마다 클라이언트를 (2까지) 하나씩 올리는  
부하 테스트 시나리오이다.  
웹 UI는 테스트 상태를 모니터링하고 사람이 쓰기 좋지만 만약에 테스트 과정을 CI/CD 파이프라인  
내에 넣거나 또는 자동화하고 싶을 때는 CLI를 이용할 수 있다.  
  
## 코딩 방법
사용방법을 이해했으면, 이제 스크립트 작성 방법을 살펴보자.  
스크립트는 Locust(클라이언트/메뚜기)를 정의해서, Locust의 행동 시나리오(TaskSet)를 정의해야 한다.  

### Locust
지원되는 Locust 종류는 HttpLocust와 Locust 두가지가 있다.  
HttpLocust는 웹 부하테스트용 클라이언트이고 범용으로 사용할 수 있는 클라이언트  
(예를 들어 데이터베이스 테스트)용으로 Locust라는 클래스를 제공한다.  
하나의 부하테스트에서는 한 타입의 클라이언트만이 아니라 여러 타입의 클라이언트를  
동시에 만들어서 실행할 수 있다.  
예를 들어서 하나의 부하테스트에서 안드로이드용 클라이언트와, IOS용 클라이언트를 동시에  
정의해서 부하 비율을 정의해서 테스트(안드로이드, IOS = 7:3으로)하는 것이 가능하다. 

## TaskSet
클라이언트(메뚜기)를 정의했으면, 이 클라이언트가 어떻게 부하를 줄지 시나리오를 정의해야 하는데  
이를 TaskSet이라고 한다.  
TaskSet는 Task의 집합으로 예를 들어, 테스트 시나리오에 리스트 페이지 보기, 상품 선택하기,  
댓글 달기 등의 Task 등을 정의할 수 있다.  

### Task
Task는 @task라는 어노테이션으로 정의하면, TaskSet이 실행될 때, TaskSet안에 있는  
task를 random하게 선택해서 실행한다. 아래 예제를 보자.  

```
class MyTaskSet(TaskSet):
  wait_time = between(5, 15)
  
  @task(3)
  def task1(self):
    pass
  
  @task(6)
  def task2(self):
    pass
```
위의 예제는 task1과, task2를 랜덤하게 선택해서 실행하게 하는데, 괄호안의 숫자는 가중치가 된다.  
즉 위의 TaskSet은 task1과 task2를 3:6의 비율로 실행하도록 한다.  

### wait_time
Task를 실행한 다음에 다른 Task를 수행할때 까지 delay 타임을 줄 수 있는데, TaskSet 클래스에  
wait_time이라는 attribute로 정의되어 있다. 여기에 값을 지정해주면, 그 시간만 기다렸다가  
다음 task를 수행한다. 위의 예제에서는 between(5, 15)를 이용해서 5 ~ 15초 사이 시간만큼  
랜덤하게 기다렸다가 다음 task를 실행하도록 하였다. 
  
### TaskSequence & seq_task
TaskSet의 다른 종류로 TaskSequence라는 클래스가 있는데, TaskSet이 task를 랜덤하게 실행한다고  
하면 TaskSequence는 Task를 순차적으로 실행한다.  
웹 테스트를 보면, 웹 사이트에서 로그인하고, 초기 화면을 들어가고, 상품 목록 페이지로 이동하고  
다음 상품 상세 페이지를 보는 것과 같은 순차적인 테스트가 필요할 수 있는데  
이를 지원하기 위한 것이 TaskSequence이다.  
TaskSet과 다르게, TaskSequence내에서 순차적으로 실행해야하는 task는 seq_task()로 정의하고  
괄호 안에는 그 순서를 정의한다.  
아래 예제를 보자. 

```
class MyTaskSequence(TaskSequence):
  @seq_task(1)
  def first_task(self):
    pass
  
  @seq_task(2)
  def second_task(self):
    pass
  
  @seq_task(3)
  @task(10)
  def third_task(self):
    pass
```
위의 예제는 first_task를 실행하고, 두 번째는 second_task를 실행한 후에, 세번째는 third_task가  
실행되는데, @task(10)으로 지정되어 있기 때문에, 10번이 실행된다. 

### Nesting
Locust에서 TaskSet은 다른 TaskSet을 호출(Nest)하는 것이 가능하다.  
예를 들어 부하 테스트 시나리오에서 게시판에 글을 쓰는 시나리오와 상품을 구입하는  
시나리오가 하나의 클라이언트에서 동시에 일어난다고 했을때, 게시판에 글을 쓰는  
TaskSet과, 상품을 구입하는 TaskSet을 정의하고 전체 시나리오에서 이 두 TaskSet을  
호출하는 것과 같이 반복적이고 재사용적인 시나리오를 처리하는데 사용할 수 있다.  

```
from locust import Locust, TaskSet, task, between

class Nested(TaskSet):
  @task(1)
  def task1(self):
    print("task1")
  
  @task(1)
  def task2(self):
    print("task2")
  
  @task(1)
  def stop(self):
    print("stop")
    self.interrupt()
    
class UserBehavior(TaskSet):
  tasks = {Nested:2}
  
  @task
  def index(self):
    print("user behavior task")
```
UserBehavior에는 index라는 Task와 Nested TaskSet이 정의되어 있고, Nested TaskSet은 가중치가  
2로 되어 있기 때문에, index task에 비해서 2배 많이 호출된다.  
Nested TaskSet은 task1, task2, stop을 가지고 있는데 각각 가중치가 1로 이 셋중 하나가 랜덤으로  
실행되는데 Nested TaskSet이 실행이 시작되면, 계속 Nested TaskSet안의 task들만 실행이 되고  
그 상위 TaskSet인 UserBehavior가 원칙적으로는 다시 호출되지 않는다. 즉, Nested TaskSet의 task  
들로 루프를 도는데, 그래서 stop task에 self.interrupt를 호출해서, Nested TaskSet호출을 멈추고  
UserBehavior TaskSet으로 리턴하도록 했다.  

### Hook
TaskSet은 실행 전후에, Hook을 정의할 수 있다.  

### setup & teardown
setup과 teardown은 TaskSet이 생성되었을 때와 끝나기 전에 각각 한번씩만 수행된다.  
locust가 실행되면, TaskSet 별로 정의된 setup 메서드가 실행되고, 프로그램을 종료하면  
종료하기전에 leardown 메서드가 실행된다.  
setup과 teardown은 테스트를 위한 준비와 클린업등에 사용할 수 있는데, 예를 들어 테스트를  
위한 데이터베이스 초기화 등에 사용할 수 있다. 주의할 점은 클라이언트를 여러개 만든다고  
하더라도, TaskSet의 setup과 teardown은 각각 단 한번씩만 실행이 된다.  

### on_start, on_stop
setup과 teardown이 전체 클라이언트에서 한번만 실행된다면, 클라이언트마다 실행되는 Hook은  
on_start와 on_stop이 된다.  
on_start는 클라이언트가 생성될 때마다 한 번씩 실행된다.  

```
from locust import Locust, TaskSet, task, between

class UserBehavior(TaskSet):
  def setup(self):
    print("nested SETUP");
  
  def teardown(self):
    print("nested TEAR DOWN");
  
  def on_start(self):
    print("nested on start");
  
  def on_stop(self):
    print("nested on stop");
  
  @task(1)
  def task1(self):
    print("task1")
  
  @task(1)
  def task2(self):
    print("task2")
  
class User(Locust):
  task_set = UserBehavior
  wait_time = between(1, 2)
```
코드를 
```
%locust -f ./locust.py --no-web -c 2 -r 1
```
명령으로 2개의 클라이언트를 실행하게 되면 on_start는 단 두번 실행이 된다. 

## Locust setup & teardown
TaskSet의 Hook에 대해서 알아봤는데, 테스트 클라이언트인 Locust class도, setup과  
teardown hook을 가지고 있다. 하나의 Locust 클라이언트는 여러 개의 TaskSet을 가지고  
있기 때문에 TaskSet마다 정의된 setup과 teardown이 실행되지만, Locust는 하나만 존재하기  
때문에, 전역적으로 setup과 teardown은 정확하게 단 한번만 수행된다.  
Locust와 TaskSet의 실행순서를 보면 다음과 같다.
  
실행순서  
- Locust setup
- TaskSet on_start
- TaskSet tasks...
- TaskSet on_stop
- TaskSet teardown
- Locust teardown

[참고](https://bcho.tistory.com/1369)


