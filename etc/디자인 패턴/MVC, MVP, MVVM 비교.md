웹 개발자로 일을 하면서 가장 먼저 접한 디자인 패턴이 바로 MVC 패턴이다.   
그만큼 유명하고 많이 쓰이는 디자인 패턴인 MVC 패턴과 MVC 패턴에서 파생되어져   
나온 MVP 패턴과 MVVM 패턴을 이야기해 본다.   
이렇게 역할을 분리하는 이유는   

> 각각의 역할을 나눠 코드 관리를 하자

역할을 나누어 관리가 된다면, 유지보수와 개발효율이 좋아질 것이다.   
  
# 1. MVC
MVC 패턴은 Model + View + Controller를 합친 용어이다. MVC 패턴의 구조, 동작,   
특징, 장점, 단점을 이야기한다.   

## 1) 구조   
<img src="https://user-images.githubusercontent.com/33191974/163674699-f09e5290-1bcc-4881-8127-5189359d591d.png" width="50%" height="50%"/>  
- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분이다.   
- View : 사용자에게 보여지는 UI 부분이다.   
- Controller : 사용자의 입력(Action)을 받고 처리하는 부분이다.  

## 2) 동작   
MVC 패턴의 동작 순서는 아래와 같다.  
1. 사용자의 Action들은 Controller에 들어오게 된다.  
2. Controller는 사용자의 Action을 확인하고, Model을 업데이트한다.   
3. Controller는 Model을 나타내줄 View를 선택한다.   
4. View는 Model을 이용하여 화면을 나타낸다.   
※참고 - MVC에서 View가 업데이트 되는 방법
- View가 Model을 이용하여 직접 업데이트하는 방법
- Model에서 View에게 Notify하여 업데이트하는 방법  
- View가 Polling으로 주기적으로 Model의 변경을 감지하여 업데이트하는 방법   

## 3) 특징   
Controller는 여러개의 View를 선택할 수 있는 1:n 구조이다.   
Controller는 View를 선택할 뿐 직접 업데이트하지 않는다(View는 Controller  
를 알지 못한다)  

## 4) 장점  
MVC 패턴의 장점은 널리 사용되고 있는 패턴이라는 점에 걸맞게 가장 단순하다.   
단순하다 보니 보편적으로 많이 사용되는 디자인패턴이다.    

## 5) 단점   
MVC 패턴의 단점은 View와 Model 사이의 의존성이 높다는 것이다. View와 Model  
의 높은 의존성은 어플리케이션이 커질수록 복잡해지고 유지보수가 어렵게  
만들 수 있다.   

# 2. MVVM
MVVM 패턴은 Model + View + View Model를 합친 용어이다. Model과 View은 다른  
패턴과 동일하다. MVVM 패턴의 구조, 동작, 특징, 장점, 단점을 이야기한다.   

## 1) 구조
<img src="https://user-images.githubusercontent.com/33191974/163674976-eed65a09-02b2-42df-b95d-4a3d5a1a31f0.png" width="50%" height="50%"/>   
- Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분이다.  
- View : 사용자에서 보여지는 UI 부분이다.  
- View Model : View를 표현하기 위해 만든 View를 위한 Model이다. View를 나타내   
주기 위한 Model이자 View를 나타내기 위한 데이터 처리를 하는 부분이다.   

## 2) 동작  
MVVM 패턴의 동작 순서는 아래와 같다.   
1. 사용자의 Action들은 View를 통해 들어온다.   
2. View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달한다.   
3. View Model은 Model에게 데이터를 요청한다.  
4. Model은 View Model에게 요청받은 데이터를 응답한다.   
5. View Model은 응답받은 데이터를 가공하여 저장한다.   
6. View는 View Model과 Data Binding하여 화면을 나타낸다.   

## 3) 특징
MVVM 패턴은 Command 패턴과 Data Binding 두가지 패턴을 사용하여 구현되었다.   
Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을   
없앴다. View Model과 View는 1:n 관계이다.   

## 4) 장점  
MVVM 패턴은 View와 Model 사이의 의존성이 없다. 또한 Command 패턴과 Data  
Binding을 사용하여 View와 View Model 사이의 의존성을 또한 없앤 디자인패턴  
이다. 각각의 부분은 독립적이기 때문에 모듈화하여 개발할 수 있다.   

## 5) 단점   
MVVM 패턴의 단점은 View Model의 설계가 쉽지 않다는 점이다.   
























