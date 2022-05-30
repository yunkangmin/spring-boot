## JPA

#### 엔티티
1. 엔티티는 테이블에 대응하는 하나의 클래스이다.
```
@Entity
public class Account {
  String username;
  String password;
}
```

spring-boot-starter-data-jpa 의존성을 추가하고 @Entiy 어노테이션을 붙이면 테이블과 자바 클래스가 매핑이 됩니다.  
그래서 JPA에서 '하나의 엔티티 타입을 생성한다'라는 의미는 '하나의 클래스를' 작성한다는 의미가 됩니다.  

### 엔티티 매니저
1. 엔티티 객체들을 관리하는 역할을 합니다. 여기서 관리란 Life Cycle이라고 할 수 있습니다.  

### 식별관계, 비식별관계
https://velog.io/@jch9537/DATABASE-%EC%8B%9D%EB%B3%84%EA%B3%BC-%EB%B9%84%EC%8B%9D%EB%B3%84-%EA%B4%80%EA%B3%84  


### IdClass
http://blog.breakingthat.com/2018/03/16/jpa-entity-%EB%B3%B5%ED%95%A9pk-%EB%A7%B5%ED%95%91-embeddedid-idclass/   
http://wonwoo.ml/index.php/post/830  
https://woowabros.github.io/experience/2019/01/04/composit-key-jpa.html  
레거시 테이블에 대해서 Entity 매핑하고 나서 JOIN 으로 얽혀 있는 코드들을 API로 격리 시키는 작업  

### 복합키, 대리키 등
https://dog-foot-story.tistory.com/60

### EtityManager
https://perfectacle.github.io/2018/01/14/jpa-entity-manager-factory/

