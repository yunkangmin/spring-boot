- 출처  
https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Call-by-Value-Call-by-Reference
https://github.com/jobhope/TechnicalNote/blob/master/programming_language/call-by-sharing.md
https://dojang.io/mod/page/view.php?id=550
https://deveric.tistory.com/92
https://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_sharing
http://www.tcpschool.com/cpp/cpp_arrayPointer_pointerIntro

---

```javascript
function change(num, obj1, obj2) {
  num = num * 10;
  obj1.item = "changed";
  obj2 = { item: "changed" };
  console.log(num); //100
  console.log(obj1.item); //changed
  console.log(obj2.item); //changed
}

var num = 10;
const obj1 = { item : "unchanged" };
const obj2 = { item : "unchanged" };
change(num, obj1, obj2);

console.log(num); //10
console.log(obj1.item); //changed
console.log(obj2.item); //unchanged
```
위 문제를 통해서 Call By Sharing이라는 개념을 알게 되었다.  
전체적으로 이해한 바로는 아래와 같다.   
1. 값 던지면 값을 지역 변수로만 사용 -> 전역에 있는 값은 변경되지 않음   
(Value)   
2. 객체의 인자를 던지면 그 객체가 가지고 있는 속성의 값을 변경은 가능   
(Reference)  
3. 함수 내부로 할당받은 객체에 새로운 메모리 값으로 할당은 불가능    

---

# Call by Reference
```c++
//n이라는 박스에 10을 저장. 박스에는 메모리 위치가 있다.
//00120B14이라는 메모리 주소에 int 크기 즉, 4Byte 만큼 공간을 잡고 10을 
//넣어줌(항상 C언어는 메모리 시작 번지만 가지고 위치를 표현하고 거기서부터   
//자료형의 크기만큼 공간을 할당한다)  
int n = 10; 
double d = 3.141592;
```
## 메모리 주소값을 저장할 수 있는 자료형이 포인터이다.  
- 포인터도 하나의 타입이다.  
```c++
#include <iostream>
using namespace std;
void add(int a, int b, int sum) {
  sum += a + b;
  cout << "함수 안 sum 메모리 위치" << &sum << "\n";
}

int main() {
  int a = 3, b = 4;
  int sum = 0;
  add(a, b, sum);
  //&sum -> 변수명 앞에 &를 붙여주면 실제 잡힌 메모리 위치를 보여준다. 
  //&변수명(주소가 들어있음, 해당 위치를 가리킴) -> 할당된 공간
  cout << "main sum 변수의 메모리 위치" << &sum << "\n";
  return 0;
}
```
scanf("%d", &a); 여기 &도 결국 주소를 의미한다. a라는 변수의 주소에다가  
값을 받아서 저장시킨다는 것이다.   
- `int* ptr1 = NULL` -> 여기서 자료형은 포인터 변수의 크기가 아닌, 포인터  
변수가 가진 주소가 가리키는 값이 int형 변수라는 뜻이다.  
- 포인터 역참조  
자료형없이 `* 포인터 변수명`하면(`*ptr`) 포인터 변수에 저장되어 있는 주소를   
역으로 참조해 그 주소가 가지고 있는 실제 값을 가져온다.  
```c++
int main() {
  double num = 3.1415;
  double* numAddress = &num;
  cout << sizeof(numAddress) << "\n";
  cout << sizeof(num) << "\n";
  cout << num << "\n";
  cout << &num << "\n";
  cout << numAddress << "\n";
  cout << &numAddress << "\n";
  cout << *numAddress << "\n";
  return 0;
}
//결과
//4
//8
/3.1415
//00B3FA00
//00B3FA00
//00B3F9F4
//3.1415
```
<img src="https://user-images.githubusercontent.com/33191974/163904412-9e81cfcf-227f-4cea-a49d-b3fcc38d729b.png" width="50%" height="50%"/>    
`*numAddress` 호출을 하면 numAddress에 저장되어 있는 주소값 00B3FA00을 찾아가서   
그 안에 저장된 값 3.1415를 가져오는 것을 볼 수 있다.   
```c++
int swap(int* a, int* b) {
  int t;
  t = *a;
  *a = *b;
  *b = t;
}
```
<img src="https://user-images.githubusercontent.com/33191974/163904770-a431086b-ea51-4b58-a4fb-9d0772b198c9.png" width="50%" height="50%"/>      
```c
void addOne(int& y) {
  y = y + 1;
}
```
함수가 호출되면 y는 인수에 대한 참조가 된다. 변수에 대한 참조는 변수 자체와 똑같이  
취급되므로 참조에 대한 모든 변경사항은 인수에도 적용된다. 
```c++
int main() {
  int value = 5;
  int& ref = value;
  value = 6; //value = 6
  ref = 7; //value = 7
  ++ref; //value = 8
  return 0;
}
```
<img src="https://user-images.githubusercontent.com/33191974/163905179-b93f5c60-98e8-4616-bbc5-a1f55d73bc9a.png" width="50%" height="50%"/>    
ref와 value는 동의어가 된다.   
- `*변수명` -> 역참조
- `*변수명 = *변수명` -> 값할당
- 자바는 Call by Reference가 없다.  
주소값을 복사해서 넘기기 때문에 Call by Value이다.    
- Call by Sharing   
자바에서도 사용되고, 일반적 용어는 아니다. 자바에서는 Call by Value라고 말한다.  
여기서 값은 boxed된 객체를 말한다.     

---

1. 참조자(reference)  
- C++에 새로 도입되는 새로운 개념  
- C언어에서는 어떠한 변수를 가리키고 싶을 때는 반드시 포인터를 사용해야   
한다.   
- C++에서는 다른 변수나 상수를 가리키는 방법으로, 또 다른 방식인 참조자(  
ref)를 제공한다.   
- 참조자를 사용하면 포인터에 비해 불필요한 `&`와 `*`의 사용이 없다. 따라서  
훨씬 더 코드를 간결하게 나타낼 수 있다.  
- 참조자란 '또 다른 이름(별명)'이라고 이해하면 된다.
```c++
#include <iostream>
using namespace std;

int main() {
  int a = 3;
  int& another_a = a;
  
  another_a = 5;
  cout << "a : " << a << endl;
  cout << "another_a : " << another_a << endl;
  
  return 0;
}
//결과   
//a : 5
//another_a : 5
```
- `int& another_a = a;` : another_a는, int형 변수 a의 참조자이다.  
- `another_a = 5;` : another_a는 a와 같은 것이므로, a의 값도 같이 변경  
된 것을 볼 수 있다. 

---
# 참조자(Reference)  
변수라고 하는 것은 할당된 메모리 공간에 붙여진 이름이다. 우리는 이 이름을   
가지고 해당 메모리 공간에 접근이 가능해진다. 그러면 참조자는 무엇일까?   
참조자는 할당된 하나의 메모리 공간에 다른 이름을 붙이는 것을 말한다.  
자신이 참조하는 변수를 대신할 수 있는 또 하나의 이름인 것이다. 쉽게   
말하면 별명이라고도 할 수 있을 것 같다.    
  
예시를 통해 설명을 들어보자. num1이라는 변수를 우선 선언해보자. 정수의  
값을 가지기 위해 int로 num1을 선언한다. 그러면 메모리의 어느 공간에  
num1이라는 이름을 부여한다. 그리고 우리는 그 num1이라는 메모리 공간에  
10을 선언한다. 
```c++
#include <iostream>
using namespace std;

int main() {
  int num1 = 10;
  //num1을 참조하는 num2
  //num1과 num2는 같은 메모리 공간 주소를 가짐
  itn &num2 = num1;
  
  cout << num1 << endl;
  cout << num2 << endl;
  
  //num1의 메모리 주소값
  cout << &num1 << endl;
  //num2의 메모리 주소값
  cout << &num2 << endl;
}
```
그러면 여기서 num1이라는 메모리 공간에 다른 이름도 부여하고 싶다. 그러면   
다음과 같이 선언을 하면 num2라는 이름이 num1의 메모리 공간에 동시에 부여  
된다. 그러면 num2의 메모리 공간의 값을 출력해도 num1의 메모리 공간의 값과  
같게 나오는 것을 볼 수 있다. 특정 사람에 대해 이름을 부르지 않고 별명을  
불러도 그 사람을 지칭하는 것으로 동일하니 이와 같은 경우라고 할 수 있다.  
결과   
```
10
10
006FFA10
006FFA10
```
하지만 참조자를 선언할 때에는 조건이 있다. 참조자는 이미 선언되어져 있는   
변수에 대해서만 선언이 가능하다. 선언됨과 동시에 누군가를 참조를 해야 하는  
것을 의미한다. 따라서 상수에 대한 참조는 유효하지 않다. 또한 아무것도 참조  
하지 않는 형태의 선언도 유효하지 않다. NULL로 초기화하는 것도 불가능하다.  
  
이젠 참조자와 함수의 관계를 알아볼 것이다. 참조자의 활용에서 변수와 참조  
자를 동시에 선언하여 사용할 상황이 많이 있겠는가? 위의 상황보다는 참조자를  
함수에 활용하는 경우가 매우 많을 것이다.   
```c++
#include <iostream>
using namespace std;

int main() {
  //상수를 참조 불가능
  int &num1 = 2;
  //참조하는 값이 없는 경우 선언 불가능
  int &num1;
  //NULL값도 참조 불가능
  int &num1 = NULL;
}
```
Call-by-value와 Call-by-reference에 대해 들어보았는가? Call-by-value는   
값을 인자로 전달하는 함수의 호출방식이고 Call-by-reference는 주소값을  
인자로 전달하는 함수의 호출방식이다. 인자를 값과 주소값으로 나누어진다는  
것이 차이점이다. Call-by-value 기반의 함수는 함수 외부에 선언된 변수에  
대한 접근이 불가능하다. 하지만 Call-by-reference 기반의 함수는 함수    
외부에 선언된 변수의 변형이 불가능하다.  
```c++
#include<iostream>
using namespace std;

//Call-by-value
//외부값을 변화시킬 수 있다.
void Swap(int a, int b) {
  int temp = a;
  a = b;
  b = temp;
}

int main() {
  int num1 = 5;
  int num2 = 10;
  
  //값의 변화가 없다.  
  Swap(num1, num2);
  
  cout << num1 << endl;
  cout << num2 << endl;
}
```
결과  
```
5
10
```
메모리 기반의 원리를 통해 두 가지 경우를 비교해보자. 우선 Call-by-value에  
대한 예시를 들도록 하겠다. 두 변수에 값을 바꾸어주는 Swap()이라는 함수를   
예시로 들어본다. 메모리 측면에서 보면 우선 num1과 num2라는 메모리 공간을   
할당한다. 그 위치에 5와 10이라는 값을 각각 넣어준다. 그리고 Swap()이라는  
함수에 num1과 num2를 인자로 넣는다. 그런데 여기서 Swap(int a, int b)로  
a와 b라는 변수에 대한 메모리 공간이 생성된다. 이 공간은 num1과 num2와는   
다른 공간이다. 식을 다르게 작성하면 int a = num1, int b = num2라는 식을  
사용한 것과 같은 효과를 나타낸다. 그리고 a와 b 사이의 값의 교환이 일어난  
다. 그리고 함수가 종료되면 a와 b는 지역변수이므로 메모리의 값을 반환하고   
사라진다. 그러면 num1과 num2의 값에는 변화가 생길까? 전혀 그렇지 않다.  
이런 것을 통해 Call-by-value에 의해서는 함수 외부의 선언된 변수로 접근이   
불가능하는 것을 알 수 있다.   
```c++
#include<iostream>
using namespace std;

//Call-by-reference
//외부 값을 변화 가능
void Swap(int &a, int &b) {
  int temp = a;
  a = b;
  b = temp;
}

int main() {
  int num1 = 5;
  int num2 = 10;
  
  //값이 바뀐다.
  Swap(num1, num2);

  cout << num1 << endl;
  cout << num2 << endl;
}
```
결과   
```
10
5
```
그러면 Call-by-reference는 어떨까? 앞의 경우와 같이 num1과 num2가 선언이  
된다. 값도 똑같이 들어가 있다. 그런데 Swap()함수를 불러올 때 Swap(int& a,   
int& b)로 함수가 선언이 되어있다. 그러면 이는 int& a = num1, int& b = num  
2로 num1과 num2에 다른 이름인 a와 b를 만들어주는 것이다. 그러므로 a와   
num1은 같은 메모리 주소를 가지고 있고 b와 num2도 같은 메모리 주소를 가지게   
된다. 이에 대해 값을 바꾸어주게 되면 num1과 num2의 값을 바꾸어 주는 것과   
같다. 그리고 함수가 끝나면 참조자의 형태로 이름이 주어진 a와 b라는 이름이  
사라지게 된다. 출력을 해보면 값이 바뀌었음을 알 수 있다.  
  
<img src="https://user-images.githubusercontent.com/33191974/163916936-36dfb5cb-210a-4004-aee1-f724b351c689.png" width="50%" height="50%"/>     



---

# 정리
1. `&`가 선언문 뒤에서 변수 타입 뒤에 붙으면 "별명"을 선언한다는 뜻이다(참조  
자). 이 것은 c++에서만 가능하고 c언어에서는 불가능하다.  
```c++
int a;
int& b = a;
b = 3;
```
여기서 b는 a의 별명이다(aliasing). 따라서 위의 코드에서 a에는 3이 들어간다. 
2. `&`는 변수의 메모리 주소값을 이야기한다.  



































