Copy On Write란 말 그대로 작성시 이전의 내용을 Copy한다는 내용이다.  
컴퓨터 상의 한정된 Resource를 서로 다른 두 개의 프로세스가 공유할 때 유용하게 써먹을 수 있다.  
  
개념은 다음과 같다.  
  
분명 처리되고 있는 프로세스들 중에는 같은 Resource를 공유하는 경우가 종종 있다.  
물론 각각이 Resource 영역을 두고 처리하는 것이 가장 이상적이겠지만  Resource 영역이라는 것이  
한정되어 있기 때문에 상황에 따라서는 공유가 되어야 한다.  
하지만 문제는 공유하는 주체인 프로세스 중 하나가 이 Resource에 대해서 임의의 수정을 요구하는  
데서 발생한다.  
물론 그 주체 프로세스는 자신의 필요에 따라 수정하는 것이기 때문에 동작에 별 영향을 받지 않는다.  
하지만 이걸 같이 공유하는 다른 프로세스의 입장에서 보면 수정되기 이전의 Resource가 어쩌면 동작에  
있어서 중요한 역할을 차지할 수도 있는 것이다.  
결국 주체 프로세스가 Resource를 수정하고 난 이후에는 상황에 따라서 다른 프로세스의 동작에 영향을 줄 수  
있다는 것이다.  
  
Copy on Write는 이런 상황을 막기 위해 적용되는 방법이다.  
평소에는 Resource를 공유하다가도 위의 상황처럼 Resource를 수정할 경우가 발생하면  
이전 Resource의 복사본을 쓰게끔 하는 것이다.  
그 후에 각각의 프로세스의 포인터를 변경만 시켜주면 이전과 같이 원활하게 동작하도록 보장하는 방안이다.  
  
Single Thread System에서는 이런 Copy On Write 과정이 한 번에 이루어질 수 없기 때문에  
mutex라는 일종의 임계 영역을 처리하는 방법으로 Copy와 Write를 따로 따로 수행했었는데  
MultiThread라는 것이 도입되면서 Copy와 Write가 동시에 처리될 수 있게끔 되었다.  
  
아주 간단한 C++ 예제가 위키에 소개되어 있다.  
```c++
std::string x("Hello");

std::string y = x;

y += ", World!";
```
말 그대로 x라는 버퍼에 Hello라는 string을 집어넣었다.  
하지만 이 x에 그대로 string을 더 집어넣게 되면 원래의 데이터는 변질되는 것이다.  
물론 이 같은 경우는 시스템 운용에 전혀 영향을 미치지 않는 동작이지만  
만약 이 같은 경우가 주소 포인터를 변경하는 데서 발생한다면 동작에 영향을 받을 수 있다.  
그래서 y라는 복사본을 생성한 후에 y를 수정하게 되면 원래의 데이터는 살아있으면서 y는 변경된  
데이터도 같이 갖게 되는 것이다.  
  
OS에서는 Copy On Write가 보통 fork()를 수행할 때 적용된다.  
process1과 process2가 나눠져 있고 fork()를 사용하게 되면 process2는 process1의 child process가 될 것이다.  
이 두 개는 같은 영역의 resource를 공유하고 있다.  
![image](https://user-images.githubusercontent.com/33191974/128298269-bc6bb174-2c20-4d5e-88ef-acfe5e9bd2c3.png)

이 상태에서 parent나 child쪽이 resource를 수정하는 경우가 발생하면 Copy of page c를 process1이 점유하고 이에  
대한 포인터도 기존의 pageC를 가리키던 포인터를 Copy of page c를 가리키게끔 변경하면 이로써 Copy On Write가 적용  
된 것이다.  
물론 process2의 입장에서는 기존의 resource를 평소처럼 사용하게된다. 





















