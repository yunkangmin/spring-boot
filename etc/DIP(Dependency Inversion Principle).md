# 정리  
- 고수준 모듈(AlarmService)와 저수준 모듈(A, B) 분리방법
   - 왜 분리?   
   A, B 클래스아 추가됨에 따라 AlaramService 로직을 변경하지 않기 위해서  
   - 어떻게 분리?  
   추상타입(인터페이스, 추상 클래스) 사용  

```java
public interface Alarm {
   String beep();
}
```
저수준 모듈이 상속받을 Alarm 인터페이스를 만들어준다.   
```java
//A사의 알람 서비스   
public class A implements Alarm {
   @Override
   public Strint beep() {
      return "beep!";
   }
}	

//B사의 알람 서비스 
public class B implements Alarm {
   @Override
   public String beep() {
      return "beep";
   }
}
```
그 후에 저수준 모듈들이 Alarm을 구현하게 하게 하면 된다.  
```java
//서비스 코드
public class AlarmService {
   private Alarm alarm;

   public AlarmService(Alarm alarm) {
      this.alarm = alarm;
   }

   public String beep() {
      return alarm.beep();
   }
}
```
더 이상 AlarmService는 알람 서비스가 추가된다고 코드를 변경하거나 추가하는 일이  
없어진다.  

저수준 모듈이 고수준 모듈에 의존하게 되는 것을 DIP(의존관계 역전 원칙)라고 한다.  
		
의존이란 예를 들어 A 클래스에서 B 클래스를 사용함으로 인해 B 클래스가 변경되거나   
C 클래스가 추가되면 A 클래스의 수정이 발생하는 것을 말한다.

---

DIP는 객체지향설계 5원칙(SOLID)에서 D에 해당하는 원칙이며, 아래와 같은 의미  
를 가지고 있다.   

> 저수준 모듈이 고수준 모듈에 의존하게 되는 것  

하지만 이 설명만 봐서는 도저히 무슨 소리인지 감이 잘 잡히지 않는다. 그래서   
DIP가 무엇인지, 왜 사용하는지, 어떻게 사용하는지에 대해 조금 더 자세히 알아   
보도록 하자.   

# 계층 구조 아키텍처
<img src="https://user-images.githubusercontent.com/33191974/163096339-4ee2dd69-f9f2-4a48-962d-fe8a996013b1.png" width="20%" height="20%"/>    

보통 계층 구조는 위 사진과 같은 구조로 되어 있다.  
- 표현 계층 : 사용자의 요청을 받아 응용 영역에 전달함과 동시에 처리 결과를   
사용자에게 표시   
- 응용 영역 : 사용자에게 제공해야 할 기능 구현   
- 도메인 영역 : 도메인의 핵심 로직 & 도메인 모델 구현   
- 인프라 계층 : 구현 기술을 다룸(ex. DB 연동)   
각 계층은 위와 같은 특징을 가지고 있으며, 상위 계층은 하위 계층에게 의존     
하지만, 그렇게 되면 인프라 계층에게 종속적인 현상이 많이 일어나게 된다.   
  
예제를 보자.   

# 기존 코드의 문제   
알람 서비스를 구현하려고 한다. 알람을 보내는 서비스는 A사에서 제공하는 알람  
기능을 사용하려고 한다.   
```java
/*
  A 사의 알람 서비스   
*/
public class A {
  public String beep() {
    return "beep!";
  }
}
```
```java
/*
  서비스 코드
*/
public class AlarmService {
  private A alarm;
  
  public String beep() {
    return alarm.beep();
  }
}
```
하지만 위 코드에는 두 가지 문제가 있다.   
  
## 1. 테스트의 어려움  
위 코드에서 AlarmService만 온전히 테스트할 수 없다. 인프라 계층에 속하는   
A가 완벽하게 동작해야만 AlarmService를 테스트할 수 있다.   

## 2. 확장 및 변경이 어려움   
만약 알람 서비스에 B사가 추가된다면 어떻게 될까? 아래와 같이 서비스 코드를   
변경해야 한다.   
```java
/*
  B사의 알림 서비스
*/
public class B {
  public String beep() {
    return "beep";
  }
}
```
```java
/*
  서비스 코드
*/
public class AlarmService {
  private A alarmA;
  private B alarmB;
  
  public String beep(String company) {
    if (company.equals("A")) {
      return alarmA.beep();
    } else {
      return alarmB.beep();
    }
  }
}
```
만약 여기서 C사의 알림서비스가 추가된다면 또 서비스 코드를 바꿔야 한다.   
C사의 알림 서비스 메소드를 추가하거나, if문을 사용해서 B사의 알림 서비스를  
사용할지, C사의 알림 서비스를 사용할지 정해야 한다. 이런 식으로 인프라 계  
층이 바뀌거나 추가될 때마다 서비스 코드를 바꾸는 것은 좋은 코드가 아니다.  
   
여기서 DIP를 적용하면 위 문제를 해결할 수 있다.   

# DIP  
방금 설명한 기능을 고수준 모듈과 저수준 모듈로 분리하면 아래와 같다.   
- 고순준 모듈 : 알림(AlarmService)
- 저수준 모듈 : A사의 알림 서비스, B사의 알림 서비스(A, B)     
지금까지 사용한 방법은 고수준 모듈이 저수준 모듈에 의존하는 방법이지만,   
DIP를 적용하게 되면 저수준 모듈이 고수준 모듈에 의존해야 한다. 그러기 위  
해 사용하는 것이 추상 타입(ex. 인터페이스, 추상 클래스)이다.   
```java
public interface Alarm {
  String beep();
}
```
저수준 모듈이 상속받을 Alarm 인터페이스를 만들어준다.   
```java
/*
  A사의 알람 서비스   
*/
public class A implements Alarm {
  @Override
  public String beep() {
    return "beep!";
  }
}
```
```java
/*
  B사의 알림 서비스  
*/
public class B implements Alarm {
  @Override
  public String beep() {
    return "beep";
  }
}
```
그 후에 저수준 모듈들이 Alarm을 구현하게 하면 된다. 그렇게 되면 서비스  
코드를 아래와 같이 변경할 수 있다.   

```java
/*  
  서비스 코드
*/
public class AlarmService {
  private Alarm alarm;
  
  public AlarmService(Alarm alarm) {
    this.alarm = alarm;
  }
  
  public String beep() {
    return alarm.beep();
  }
} 
```
더 이상 AlarmService는 알람 서비스가 추가된다고 코드를 변경하거나 추가해야  
하는 일이 없어진다.  
  
또한, 알람 관련 객체는 인터페이스인 Alarm에 의존하기 때문에 Alarm을 Mock  
객체로 만들어 다양한 시나리오로 AlarmService 기능을 온전히 테스트할 수 있  
다는 장점도 가져갈 수 있다.   

<img src="https://user-images.githubusercontent.com/33191974/163099003-1d9ae56f-fb64-4454-9f84-f4a88e4b453f.png" width="50%" height="50%"/>    

코드를 클래스 다이어그램으로 나타내면 위와 같다. 즉, 저수준 모듈이 고수준   
모듈에 의존하게 되는 것을 DIP(의존관계 역전 원칙)라고 한다.   
  
만약 위 예제와 같은 문제가 발생한 적이 있다면, DIP를 적용해보면 좋을 것이다.   
  



















