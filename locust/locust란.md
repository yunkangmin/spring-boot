Locust는 오픈소스 부하테스트 도구이다. 
웹 사이트(또는 기타 시스템)의 로드 테스트 및 시스템이 처리할 수 있는 동시  
사용자 수를 파악하기 위한 것이며, 서버에서 지원하는 초당 요청 수(RPS)를  
대략적으로 알 수 있다.  
  
Locust는 메뚜기라는 뜻으로 테스트 중에 메뚜기 떼가 웹 사이트를 공격한다는 것에서  
착안된 이름이라고 한다.  
각 메뚜기(또는 원하는 경우 테스트 사용자)의 동작은 사용자가 정의하고 웹 UI에서  
실시간으로 군집 프로세스를 모니터링한다.   
이렇게하면 실제 사용자가 들어가기 전에 테스트와 병목 현상을 확인할 수 있다.  
  
Locust는 완전히 이벤트 기반이므로 단일 컴퓨터에서 수천명의 동시 사용자를 지원할 수 있다.  
다른 많은 이벤트 기반 앱과 달리 콜백을 사용하지 않고 경량 프로세스를 사용한다.  
이를 통해 콜백으로 코드를 복잡하게 만들지 않고도 Python에서 매우 간단한 시나리오를  
작성할 수 있다.   
  
### Why Locust?
- 구현이 쉬워서 작은 규모 프로젝트에 적합하다.
- python으로 api 테스트 코드를 작성할 수 있다. 
- 설치와 실행이 간단하고 대시보드를 통해 가시적으로 테스트 결과를 볼 수 있다.  

### Locust Install
EC2 instance AMZ linux2에 설치  
```
sudo yum install -y python3-devel
sudo yum install -y libevent-devel
sudo yum install -y gcc
sudo python3 -m pip install locust
```

### Locust 실행
- api 테스트할 내용을 담은 python 파일이 필요함

locustfile.py 예제  
```python
class User(HttpUser):
  
  #실제 api를 테스트할 함수 @task(1) 이런식으로 숫자를 넣어서 call하는 비중을 높일 수 있음
  @task(2)
  def get_all(self):
    self.client.get("/employees")
  
  @task(1)
  def get_id(self)
    emp_id = ["1", "2", "3", "4", "5"]
    self.client.get("/employees/" + random.choice(emp_id)
    
    @task
  def get_all(self);
    self.client.get("/test")
  
  def on_start(self):
    print("START LOCUST")
  
  def on_stop(self):
    print("STOP LOCUST")
  
      ## 최소 0.5 최대 1.5초 동안 기다렸다가 api call을 하겠다라는 설정
  wait_time = between(0.5, 1,5)
```

### 실행명령어
```python
locust -f locustfile.py
```
























