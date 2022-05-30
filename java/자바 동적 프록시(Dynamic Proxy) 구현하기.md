## What is that?
프록시는(Proxy)의 사전적 의미는 대리인을 의미한다.  
프록시라는 말은 IT분야에서만 사용하는 IT 용어가 아니라 이미 실생활에서 흔히 사용되고 있는  
개념이다.  

예를 들어 부동산은 프록시라고 할 수 있다. 
1. 나는 집주인(Target)이 소유한 집(Method)을 구매(Call)했지만, 나의 구매 요청(Call)은  
부동산(Proxy)이 받았다.   
2. 부동산(Proxy)은 내가 건네 준 계약금, 서명(Arguments)을 적절히 처리한 뒤 집주인(Target)에게  
넘겨주었다(Call).  
3. 집주인(Target)은 자신이 서명한 계약서를 부동산(Proxy)에게 돌려주고(Return) 부동산(Proxy)은  
그 계약서를 나에게 돌려주었다(Return).

이처럼 프록시는 두 객체 사이에서 이루어지는 커뮤니케이션의 중개자 역할을 하는 일종의 행위 및   
주체를 뜻한다.  

배달앱도 프록시라고 할 수 있다.  
1. 가게(Target)와 배달앱(Proxy)는 연동되어 있다(Remote).
2. 우리는 배달앱(Proxy)에게 음식(Method)을 요청(Call)하지만, 배달앱(Proxy)는 외부(Remoted)에  
있는 가게(Target)에게 음식(Method)을 요청한다.  

부동산의 예와 동일한 케이스이지만, 여기서는 외부 객체(가게)와 프록시(배달앱)를 연결했다는 가정이  
다른점이다.  

## 자바에서 프록시를 구현해보자
자바에서 프록시를 구현하는 가장 직관적이고 단순한 방법은 프록시 클래스를 직접 작성해서  
사용하는 방법이다. 
  
다음 코드들은 부동산 예를 자바로 구현한 것이다. 

### 임대차 계약서
```
public class Contract {
  //임대인 서명
  private String signOfLessor;
  //
  private String signOfTenant;
  
  public Contract(String signOfLessor, String signOfTenant) {
    this.signOfLessor = signOfLessor;
    this.signOfLessor = signOfTenant;
  }
  
  public String getSignOfLessor() {
    return signOfLessor;
  }
  
  public String getSignOfTenant() {
    return signOfTenant;
  }
  
  public void setSignOfLessor(String signOfLessor) {
    this.signOfLessor = signOfLessor;
  }
  
  public void setSignOfTenant(String signOfTenant) {
    this.signOfTenant = signOfTenant;
  }
}
```
### 임대인 인터페이스
```
public interface Lessor {
  //임대인 계약을 한다.
  public Contract contract(int money, Contract contract);
}
```

### 집주인 클래스
```
public class Landlord implements Lessor {
  private int account;
  private String sign;
  
  //생성하면서 서명날인을 인자로 준다.
  public Landlord(String sign) {
    this.account = 0;
    this.sign = sign;
  }
  
  //계약은 집주인도 해야하므로 Lessor를 구현하면서 contract메소드를  
  //오버라이딩했다.
  @Override
  public Contract contract(int money, Contract contract) {
    account += money;
    contract.setSignOfLessor(sign);
  }
}
```
우리는 여기서 부동산의 역할을 하는 프록시를 만들어서 프록시가 계약요청을 받아서 중개수수료를  
제외한 금액과 임차인의 서명이 들어간 계약서를 집주인(Landlord)에게 전달하도록 할 것이다.  

### 부동산 프록시
```
public class Realtor implements Lessor {
  private Landlord landlord;
  
  public Realtor(Landlord landlord) { 
    this.landlord = landlord;
  }
  
  @Override
  public Contract contract(int money, Contract contract) {  
    money = money * 10 / 100;
    Contract contract = landlord.contract(money, contract);
    return contract;
  }
  
  public Contract newContract() {
    return new Contract();
  }
}
```
### 프록시 사용 예제
```
//예제에서는 사용자 쪽에서 타겟 객체를 생성했지만 실제로는 프록시 내에서 생성
//및 관리할 수 있음.
//집주인, 서명날인을 인자로 준다.
Landlord landlord = new Landlord("홍길동");
//부동산 프록시
//집주인 객체를 인자로 주어 계약시 부동산
//업자가 수수료를 떼고 집주인 싸인 받고 계약서를 리턴한다. 
Realtor realtor = new Realtor(landlord);

//계약서
Contract contract = realtor.newContract();
//부동산 싸인
contract.setSignOfTenant("김대차");
//프록시를 통해 계약 진행
contract = realtor.contract(100000, contract);
```
여기서 예제의 효율성이나 적합성보다는 프록시를 통해 계약이 진행된다는 점에만 주목하자.  
자바는 이러한 방법 외에 동적 프록시(Dynamic Proxy) 생성을 위한 라이브러리(java.lang.reflect)를  
제공하고 있다. 그리고 우리는 최종적으로 동적 프록시를 사용할 것이다.  

## Why use it?
위 예제처럼 프록시 클래스를 직접 생성해서 사용하는 방법은 두 가지 단점이 있다. 
1. 귀찮기도 하고 관리해야할 클래스가 하나 더 늘어난다.
2. 만약 계약의 종류(메소드)가 무수히 많고 각 계약에서 프록시가 하는 일이라곤 수수료 10%제외라면  
수수료 제외를 위해 무수히 많은 메소드를 오버라이딩해줘야 한다.  

자바의 동적 프록시는 이러한 문제를 한 번에 해결해준다.  
우선 동적 프록시는 다음 클래스의 객체를 생성하면 만들어진다.  
```
java.lang.reflect.Proxy
```
하지만 new 연산자로 Proxy의 객체를 생성할 순 없고 newProxyInstance 메소드를 통해서만 생성이  
가능하다.  


...

[프록시 참고](https://velog.io/@adduci/%EC%9E%90%EB%B0%94%EC%9D%98-%EB%8F%99%EC%A0%81-%ED%94%84%EB%A1%9D%EC%8B%9C)


































