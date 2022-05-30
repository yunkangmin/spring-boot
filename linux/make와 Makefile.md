# make & Makefile 이란?   
SHELL(CLI와 같은 명령어를 입력하는 환경)에서 컴파일을 해봤다면, make 명령어로   
컴파일을 실행하는 경우를 자주 봤을 것이다. Makefile이 있는 디렉터리에서 make  
만 치면 컴파일이 실행된다. 어떻게 이런 일이 일어날 수 있을까? make는 파일  
관리 유틸리티이기 때문이다.   
  
make는 파일 간의 종속관계를 파악하여 Makefile(기술파일)에 적힌 대로 컴파일러  
에 명령하여 SHELL 명령이 순차적으로 실행될 수 있게 한다.   
  
# make를 쓰는 이유  
하지만 여기서 의문점이 있을 수 있다. 그냥 컴파일러로 컴파일하면 되지 왜  
굳이 Makefile을 만들고 make 명령을 실행해야 할까?  
make을 쓰면 다음과 같은 장점이 있다.   
- 각 파일에 대한 반복적인 명령의 자동화로 인한 시간 절약
- 프로그램의 종속 구조를 빠르게 파악할 수 있으며 관리가 용이
- 단순 반복 작업 및 재작성을 최소화   

글로만 보니 이해가 잘 안될 것이다. 그럼 make의 필요성을 느껴보기 위해 기본  
적인 컴파일과 make를 이용한 컴파일을 직접 해보자.   

# 예제  
<img src="https://user-images.githubusercontent.com/33191974/160282625-e5367cb1-9473-49f3-888b-e8dc6154c166.png" width="50%" height="50%"/>  
이제 위의 종송관계 표를 보며 diary_exe라는 실행파일을 만들어보자.  

## 1. diary.h 헤더 파일 만들기   
세 개의 c 파일이 include할 헤더파일을 생성해보자.  
```
vi diary.h (헤더 파일 생성)  
```
코드   
```
#include <stdio.h>
void memo();
void calendar();
```
## 2. 재료로 사용될 C파일 만들기   
```
vi memo.c
vi calendar.c
vi main.c
```
코드   
memo.c  
```
#include "diary.h"
void memo() {
  printf("I'm function Memo! \n");
}
```
calendar.c  
```
#include "diary.h"
void calendar() {
  printf("I'm function Calendar() \n");
}
```
main.c  
```
#include "diary.h"

int main(void) {
  memo();
  calendar();
  return 0;
}
```
## 3. 생성된 파일 확인하기  
위의 모든 파일을 생성했다면 제대로 생성되었는지 `ls`명령으로 확인해보자.  
```
ls
```
<img src="https://user-images.githubusercontent.com/33191974/160283220-768865e8-1dab-48ce-a0f8-d61b31beddd1.png" width="50%" height="50%"/>    

모든 파일을 생성했다면 이제 두가지 방법으로 컴파일을 해보자.  

# 기본적인 컴파일 과정
먼저 기본적인 방법으로 컴파일을 해보자. 컴파일은 gcc를 이용했다.  
## 1. c파일에서 object 파일 생성하기   
```
gcc -c -o memo.o memo.c
gcc -c -o calendar.o calendar.c
gcc -c -o main.o main.c
```
각 c파일에서 object 파일을 생성한다.   
여기서 -c 옵션은 object 파일을 생성하는 옵션이고 -o 옵션은 생성될 파일  
이름을 지정하는 옵션이다.  

> 여기서는 -o 옵션을 넣지 않아도 object 파일이름이 (c파일이름).o로 자동   
> 생성된다. 하지만 실행 파일 생성 시 -o 옵션을 넣지 않으면 모든 파일이  
> a.out이라는 이름을 가지게 되므로 여러 개의 실행 파일을 생성해야 할 때   
> 효율적인 옵션이다.   

ls 명령어로 object 파일이 제대로 생성되었는지 확인해보자.   
<img src="https://user-images.githubusercontent.com/33191974/160283650-258d36d7-548d-48bc-bfdd-eff7d5a74087.png" width="50%" height="50%"/>  

## 2. 각 object 파일을 묶어 컴파일을 통해 diary_exe 실행파일 생성하기   
이제 실행 파일을 생성해보자.  
```
gcc -o diary_exe main.o memo.o calendar.o
```
여기서 object 파일들의 순서는 상관이 없다.  
위의 명령어를 실행하면 드디어 diary_exe 실행파일이 생성된다.   
  
ls 명령어로 확인해보자.  
<img src="https://user-images.githubusercontent.com/33191974/160283890-626349ae-40af-47d1-bf36-f6f49aa0f5f6.png" width="50%" height="50%"/>     

## 3. 결과 확인하기  
```
./diary_exe
```
<img src="https://user-images.githubusercontent.com/33191974/160283931-537c7983-17f9-476e-9a8b-6865af88dfd5.png" width="50%" height="50%"/>  
기존의 컴파일 과정이 여기서는 그리 귀찮지 않다. 모든 c파일을 각각 컴파일해도  
3번만 명령해주면 되기 때문이다. 하지만 만약 하나의 실행파일을 생성하는 데   
필요한 c파일이 1000개라면 1000개의 명령어가 필요하다. 이러한 상황을 해결해  
주는 것이 바로 make와 Makefile이다.   

# make를 이용한 컴파일 과정
그럼 이제 Makefile을 먼저 어떻게 만드는지 알아본 후 make 명령으로 위의 파일   
들을 컴파일해보자.   

# Makefile의 구성   
Makefile은 다음과 같은 구조를 가진다.   
- 목적파일(Target): 명령어가 수행되어 나온 결과를 저장할 파일  
- 의존성(Dependency): 목적파일을 만들기 위해 필요한 재료   
- 명령어(Command): 실행되어야 할 명령어들   
- 매크로(macro): 코드를 단순화 시키기 위한 방법   

# Makefile의 기본구조   
위의 구성에서 말한 요소들은 실제 Makefile 코드에서 다음과 같이 배치된다.  
<img src="https://user-images.githubusercontent.com/33191974/160284179-79458c84-c98d-4755-a01a-9daf695291b0.png" width="50%" height="50%"/>    

# Makefile 작성규칙  
> 목표파일: 목표파일을 만드는데 필요한 구성요소들    
> (tab)목표를 달성하기 위한 명령1  
> (tab)목표를 달성하기 위한 명령2  
//매크로 정의: Makefile에 정의한 string으로 치환한다.  
//명령어의 시작은 반드시 탭으로 시작한다.  
//Dependency가 없는 target도 사용 가능하다.   

# make 예제 따라해보기  
```
vi Makefile
```
아래 그림에서 main.o 옆에 `:`을 넣어주자.   
<img src="https://user-images.githubusercontent.com/33191974/160284491-ac170c00-8ac6-473a-b6ef-9c317dfffe4e.png" width="50%" height="50%"/>  
```
diary_exe: memo.o calendar.o main.o
        gcc -o diary_exe memo.o calendar.o main.o

memo.o : memo.c
        gcc -c -o memo.o memo.c

calendar.o : calendar.c
        gcc -c -o calendar.o calendar.c

main.o :  main.c
        gcc -c -o main.o main.c

clean :
        rm *.o diary_exe
```

여기서 더미타켓은 파일을 생성하지 않는 개념적인 타켓으로   
```
make clean
```
위와 같은 명령어를 실행하면 현재 디렉터리의 모든 object 파일들과 생성된   
실행파일인 diary_exe를 rm 명령어로 제거해준다.   
  
이제 아래 명령어로 Makefile을 실행해준다(의존 관계에 있는 파일들을 생성하기  
위한 명령어들을 먼저 실행하는 듯하다).   
```
make
```
<img src="https://user-images.githubusercontent.com/33191974/160284837-e1635512-943b-4881-b2eb-454cf5fb4c22.png" width="50%" height="50%"/>  
  
명령어들이 실행되면서 타겟파일이였던 diary_exe가 만들어졌다.   
실행결과는 기본적인 컴파일 과정에서 본 결과와 동일함을 알 수 있다.    
그런데 아직까지 기본적인 컴파일 과정을 묶어둔 것 외에는 특별한 점이 없어    
보인다.   
위의 코드를 더욱 단순화시키기 위해서 매크로(macro)를 사용해보자.     

# Makefile 개선하기: 매크로 사용   
매크로는 생각보다 간단하다. 위의 코드에서 중복되는 파일 이름들을 특정 단어로   
치횐하면 된다.   
마치 C언어에서 #define을 하는 것과 비슷한 원리이다.  

# Makefile 매크로 사용 예제  
<img src="https://user-images.githubusercontent.com/33191974/160285084-28e45545-4c04-4df6-9a6a-14830701d37a.png" width="50%" height="50%"/>  

## 작성 규칙   
- 매크로를 참조할 때는 소괄호나 중괄호를 둘러싸고 앞에 `$`를 붙인다.  
- 탭으로 시작해서는 안되고, `:=#""`등은 매크로 이름에 사용할 수 없다.  
- 매크로는 반드시 치환될 위치보다 먼저 정의되어야 한다.   

여기서 -W -Wall는 컴파일 시 컴파일이 되지 않을 정도의 오류라도 모두 출력되게   
하는 옵션이다.   
```
make clean  
vi Makefile //매크로 사용 예제처럼 수정  
./diary_exe
```
위 명령으로 전과 같은 결과가 나오는지 확인해보자.  






  

















