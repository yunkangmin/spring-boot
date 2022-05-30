## Lua 프로그래밍 기초 문법
Lua는 로블록스 게임 개발을 위해 필요한 프로그래밍 언어이다.  
여태까지 사용했던 Java, PHP, JS와는 문법이 좀 다르다. 

### 1. 주석
```lua
-- 루아에서 두개의 연속된 대쉬(--)는 그 대쉬가 있는 한 줄을 주석으로 처리한다.

--[[
    여러줄 주석을 사용하려면 이 형태를 이용한다. 
]]--
```

### 2. 변수 - 범위(scope)
- Lua에서는 변수선언할 때 앞에 아무런 글자를 붙히지 않으면 전체(Global)에 해당한다.  
(Golbal 일 때는 따로 정의를 하지 않는다.)  
- 한 영역에서만 유효하려면 "local"로 정의한다.  
```lua
print("What is local")

a=3

print(a) //3

if true then
  local a = 20
  local b = "bbbb"
  print (a) //20
  print (b) //bbbb
end

print (a) //3
print (b) //nil
```

### 3. 유형, 변수타입 (Type)
Lua는 Java처럼 변수를 선언할 때 타입을 설정할 필요는 없다.  
물론 형 자체는 존재한다.  
- nil : 빈 값, type도 nil (자바의 null)
- boolean : true 또는 false 값을 가진 type이다.
- number : 모든 숫자이다. 정수, 소수점이 있는 숫자 모두 포함이다.
- string : 문자를 갖고 있으면 문자형이 된다. 
- function : 로직을 포함한 함수
- table : 여러 변수들의 그룹을 나타내는 변수이다.  
아주 중요(Array 유형도 table이다.) / 테이블을 클래스처럼 활용할 수도 있다.
```lua
local a = 10
print(type(a)) //number

a = nil
print(type(a)) //nil

a = "hello"
print(type(a)) //string

a = 3123.1231
print(type(a)) //number

a = (1 > 3)
print(type(a)) //boolean

a = {}
print(type(a)) //table

b = {"Lua", "Tutorial"}
print(type(b)) //table
```

### 4. 연산자 (Operation)
자바와는 조금 다르다.  
자바의 "++"같이 증가나 이런건 없다.
```lua
// 1. 수식 연산자
+ : 더하기
- : 뺴기
* : 곱하기
/ : 나누기 --> 결과가 소수점까지 나옴
% : 나머지
^ : 몇 승

//2. 관계 연산자 - 결과가 boolean
== : 같다
~= : 다르다
>
<
>=
<=

//3. 논리연산자 - 결과가 boolean
and : A and b -- A가 참이고 B가 참이어야 참
or : A or B -- A 또는 B가 참이어야 참
not : 참이면 거짓, 거짓이면 참

//4. 기타 : 오히려 가장 쓸모 있는
.. : 두 문자를 붙여줌
# : 배열/테이블의 갯수 카운트

//예시
local a = {"hi", " my", " name", " is", " noval"}
print(a[1].."!!!"..a[2]..a[3]..a[4]..a[5]) print(#a)

//결과
hi!!! my name is nova
```

### 5. 반복문
Lua에서 제공하는 반복 기능은  
1. while
2. for
3. repeat until
4. ipairs / pairs ( table 대상 반복)

---
#### 1. while (조건) do ~ end
```lua
a = 10

while (a < 20)
do
  print("value of a:", a)
  a = a + 1
end
```
while 다음에 오는 조건이 참이 될 때가지 do ~ end 사이를 계속 실행한다.  
종종 while(true) 같은 것을 이용해서 무한히 돌게 만들기도 하는데, 그러면 사이에 반드시 wait()  
같은 간격을 두어야 한다.  
그런게 없이 while (true)하면 바로 프로그램은 종료된다.  

#### 2. for ~ do ~ end
```lua
for i = 10,20,1
do
   print(i)
end
```
처음 값이 두번째 값이 될 때까지, 세 번째 값을 처음 값에 더한다.  
세 번째 값을 생략하면 (1)이라고 생각해서 계속 더한다.  
  
#### 3.repeat ~ until (조건)
```lua
a = 10

repeat
   print("value of a:", a)
   a = a + 1
until( a > 15 )
```
while과 거의 비슷한데 while(조건) do ~ end 대신  
repeat ~ until (조건)으로 표시한다.  
개인적으로 조건이 앞에 있는 것들이 혼동을 덜 줘서 가능하면 while을 사용한다.  
  
### 6. 조건문
- #### if (조건) then ~ A ~ else ~ B ~ end  
  조건이 참이면 A를 실행하고 거짓이면 B를 실행한다.  
  else ~ B ~ 부분은 없어도 상관없다.  
  즉, 조건이 참이면 A만 실행 아니면 그냥 바로 밖의 다음 줄로 진행이 된다.  
  ```lua
  local a = false
  local b = true
  
  if (a and b)
  then
    print("A and B is true")
  else
    print(" A or B is false")
  end
  ```
- #### if 문 안의 if
  ```lua
  if false
  then
    print (1)
  else if true
    then
      print (2)
    else
      print (3)
    end
  end
  ```
  
  ### 7. 함수
  #### 함수 정의 방법
  ```lua
  local function hihi (a,b)
          print("hihihi")
          return 123
  end
  
  hihi() //hihihi
  print(type(hihi)) //function
  print(hihi) //function: abc...(함수의 위치주소)
  print(hihi()) //hihihi 123
  ```
  #### 함수 사용의 팁
  1. 변수와 마찬가지로 local을 붙히면 해당 범위 안에서 사용, 아무것도 없으면 global 함수가 된다.
  2. 함수의 파라미터 안에 다른 함수를 넣어서 함수 안에서 불러 쓸 수 있다.
  ```lua
  myprint = function(param)
          print("This is my print function - ##", param, "##")
  end
  
  function add(num1, num2, functionPrint)
         result = num1 + num2
         functionPrint(result)
  end
  
  myprint(10)
  add(2, 5, myprint)
  ```
  3. 파라미터가 미리 확정되지 않았을 때 ...형태로 넘겨서 함수 안에서 하나씩 불러 쓸 수 있다.
  ```lua
  function average(...)
         result = 0
         local arg = {...}
      for i, v in inpairs(arg) 
      do
        result = result + v
      end
      return result/#arg
  end
      
      print("The average is", average(10, 5, 3, 4, 5, 6))
  ```
  
  ### 8. String
  유용한 String들 모음
  1. 모든 글씨 대문자 : string.upper()
  2. 날짜를 자리수에 맞춰 표시 : string.format()
  3. 변수에 저장된 이름으로 대화 만들기 : "Hi! "..playname
  4. 반복되는 문장 함수로 표시 : string.rep("WHAT!", 100)
  5. 날짜 등으로 자리수 고정해야 할때 : string.format("%03d", aaa)하면 빈칸에 0
  
  #### 예시
  ```lua
  myname = "nobakee"
  
  print(string.upper(myname)) //NOBAKEE
  
  date =3; month = 5; year = 2020
  print(string.format("Date %02d/%02d/%03d", date, month, year)) //Date 03/05/2020
  
  print("Hello! " ..myname) //Hello! nobakee
  print(string.rep("WHAT! ", 100)) //WHAT! x 100번
  ```
  
  ### 9. Array
  다른 언어들과 다르게 Lua에서는 Array의 첫 번째 값의 인덱스가 1부터 시작한다. 
  
  animals = {강아지, 돼지, 닭, 고양이, 말}
  - 다른 언어 : animals[3] = 고양이
  - Lua : animals[3] = 닭
  
  #### Array 기초 사용 예시
  ```lua
  texts = {}
  print(type(texts))
  
  //#texts는 texts라는 배열의 크기를 나타냄
  texts = {"a", "b", "c", "d", "e", "d", "f"}
  print("size : "..#texts)
       for i=1, #texts do print(texts[i])
       end
       
  texts = {1, 2, 3, 4, 5}
  print("size : "..#texts)
       for i=1,#texts do print(texts[i])
       end
  ```
  #### 이중 Array 사용 예시
  ```lua
  m_array = {}
   for i=1, 10 do
     m_array[i] = {}
        for j=1, 10 do
     m_array[i][j] = i*j
   end
  end
  
  for i=1, 10 do
    for j=1, 10 do
       print("["..i.."]["..j.."]"..m_array[i][j])
       end
    end
  ```
  여기서 중요한 것은 값을 할당할 때
  m_array[i] = {}로 미리 array를 쭉 선언해줘야 동작한다는 것이다.

### 10. table
Table은 Lua의 유일한 자료구조이다.  
Table은 여러 다른 데이터들의 그룹이다.  
그 안에는 숫자, 문자, 함수, 다른 테이블 모두 포함할 수 있다.  
이 Table을 활용해서 클래스처럼 사용하기도 하고 배열로 사용하기도 한다.  
이 전의 Array도 하나의 Table이라고 볼 수 있다.  
```lua
table = {"abc", 1, 2, name = 'chan', age = 26, 999}

-- 위 예시 테이블에서 key 값이 없는 값들은 array처럼 순서대로 1부터 시작하는 
-- index를 가지게 된다.
-- 중간에 name = "chan" 처럼 키 값을 가지는 값이 있으면 무시하고 index를 카운트한다.

-- 테이블에서 값에 접근하는 예시
table = {"abc", 1, 2, name = 'chan', age = 26, 999}
table[1] = "abc"
table[2] = 1
table[3] = 2
table[4] = 999
table.name = 'chan'
table["name"] = 'chan'
```

테이블을 선언할 때 값을 넣는 방법
```lua
-- 테이블에서 값에 접근하는 예시
-- 두 방법 모두 같은 결과
t1 = {x = 0, y = 1}
t2 = {}; t2.x = 0; t2.y = 1

-- 굳이 이럴 필요는 없겠지만 이 역시 같은 표현
t3 = {"red", "yellow", "blue"}
t4 = {[1]="red", [2]="yellow", [3]="blue"}
```
테이블은 숫자 대신 'key' 형태로 이름을 가질 수 있고, 호출할 때는 '.'으로 접근한다.  
이런 그룹화된 table을 쉽게 다루기 위해서 iterator라는 반복을 도와주는 기능이 제공된다.  
for문과 함계 사용하여 쉽게 table 내부를 반복할 수 있다.  
- pairs : 테이블 안의 index, key index 무관하게 전체를 보여준다.
- ipairs : 테이블 안의 숫자 index만 뽑아서 보여준다.
```lua
table = {"abc", 1, 2, name = 'chan', age = 26, 999}

--pairs 예시
print('pairs 테스트')
for key, value in pairs(table) do
  print ("key: "..key, "value: "..value)
end
--결과 : table 안의 내용 모두 출력 (key: 1 value: "abc"..)형태 

--ipairs 예시
print('pairs 테스트')
for index, value in ipairs(table) do
  print("index: "..index, "value: "..value)
end
--결과 : table안의 key 값들을 제외한 내용 출력 (index : 4 value : 999..) 형태

--결과값 테스트
print("결과")
print(table.name) //chan
print(table["name"]) //chan
print(table[1]) //abc
```
테이블을 클래스처럼 사용하는 방법은 다른 글에서 설명한다.  
(테이블과 메타테이블을 활용해 클래스처럼 만들 수 있음. 상속 포함)

### 11. Module
Module은 어떤 함수들을 미리 만들어 놓고, 불러와서 사용할 수 있도록 한 "함수 모음"이라고 할 수 있다.  
lua_module.lua라는 파일에 table을 하나 만들어 return 한다.  
사용하는 쪽에서는 require 명령어를 이용해 불러서 사용할 수 있다.  
```lua
-- 1. module script 생성
-- 2. 테이블 선언 및 함수 정의
local mymath = {}

function mymath.add(a, b)
        print(a+b)
end

function mymath.sub(a, b)
        print(a-b)
end

function mymath.mul(a, b)
        print(a*b)
end

function mymath.div(a, b)
        print(a/b)
end

return mymath
```
사용하는 쪽에서는 
```lua
mymathmodule = require("mymath")
mymathmodule.add(10,20)
mymathmodule.sub(30,20)
mymathmodule.mul(10,20)
mymathmodule.div(30,20)
```
ServerStorage에 module Script를 생성하고 다른 곳에서 불러서 사용하는 예를 보자  
---
#### 실사용 예시
1. 모듈 생성
```lua
-- 보통 ServerStorage안의 module Script 내부에서 생성.
local MoneyManager = {}
local questReward = 100

--함수 정의
function MoneyManager.finishQuest(player)
       player.Money = player.Money + questReward
end

return MoneyManager
```
2. 사용
```lua
-- 다른 스크립트에서
local MoneyManager = require(ServerStorage.ModuleScript)
```
공통된 기능들을 이렇게 모두 한군데 모아두면 유지보수시 편하다.  


