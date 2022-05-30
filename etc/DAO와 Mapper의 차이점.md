Spring 프로젝트를 진행하면서 다양한 프로젝트 소스를 참고하다가 한가지 고민이 생겼다.  
필자는 스프링 프로젝트의 구조를 잡을 때 Controller - Service - Mapper.java - Mapper.xml  
형식으로 구조를 잡았는데, 다른 스프링 프로젝트 구조를 보면 Controller - Service -  
DAO.java - Mapper.xml 형식으로 구조를 잡고 있었다. 단순히 순서상으로만 보면  
Mapper.java와 DAO.java만 변경하면 되는 것처럼 보이지만, 처음 구조에서 Mapper.java는  
단순 인터페이스지만 두번째 구조에서 DAO.java는 인터페이스와 클래스의 결합된 형태였고  
그 외에도 여러 다른 점들이 눈에 띄었다.  
  
Mapper 인터페이스를 활용한 프로젝트와 DAO를 활용한 프로젝트의 명확한 차이점을  
확인하기 위해서 두가지 구조로 프로젝트를 만들어 볼 것이다. 우선 DAO와 Mapper의  
정의에 대해 설명하고 두 구조로 프로젝트를 만들어보자. 
  
# DAO와 Mapper 인터페이스 정의
## 1. DAO란?
Data Access Object의 약어로 실질적으로 DB에 접근하여 데이터를 조회하거나 조작하는  
기능을 전담하는 객체를 말한다. DAO의 사용이유는 효율적인 커넥션 관리와 보안성 때문이다.  
DAO는 저수준의 Logic과 고급 비즈니스 Logic을 분리하고 domian logic으로부터 DB관련  
mechanism을 숨기기 위해 사용한다.  
## Mapper 인터페이스란?
Mybatis 매핑 XML에 기재된 SQL을 호출하기 위한 인터페이스이다.  
Mybatis 3.0부터 생겼다.  

## Mapper 인터페이스를 사용하지 않을 경우
- SqlSession을 등록해줘야 한다.
- DAO 인터페이스와 인터페이스를 구현한 DAO 클래스를 생성해줘야한다. 
- Mapper 인터페이스를 사용하지 않았을 때는 네임스페이스 + "." + SQL ID로  
지정해서 SQL을 호출해야 한다.  
```
session.selectOne("com.test.mapper.TimeMapper.getReplyer, bno));
```
- selectOne, insert, delete 등 제공하는 메소드를 사용해야 한다.
- 문자열로 작성하기 때문에 버그가 생길 수 있다.
- IDE에서 제공하는 code assist를 사용할 수 없다.  

## Mapper 인터페이스를 사용하는 방법
- Mapper 인터페이스는 개발자가 직접 작성한다.
- mapper 네임스페이스는 패키지명을 포함한 인터페이스 명으로 작성한다.
- SQL id는 인터페이스에 정의된 메서드명과 동일하게 작성한다.  

# Controller - Service - Mapper.java - Mapper.xml 구조로 프로젝트 생성
우선 원래 프로젝트를 수행 시 구축했던 Mapper 인터페이스 구조를 만들어보자.  

...

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
