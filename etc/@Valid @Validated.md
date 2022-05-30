## 들어가기에 앞서
request 후에 서버 측에서 데이터를 바인딩할 때, 데이터가 유효한지(ex. 누락, 최대 크기 초과 등)  
검사해야 하는 경우가 있을 수 있다. 그럴 때는 @Valid 또는 @Validated 어노테이션을 이용하여  
데이터 유효성 검증을 할 수 있다.  
이번 게시물에서는 @Valid와 @Validated의 차이점을 알아보고, 각각의 어노테이션을 사용해서  
데이터 유효성을 검증 해보는 방법을 알아본다.  

### 라이브러리 세팅
pom.xml에 다음 코드를 추가한다.
```
<dependency>
  <groupId>javax.validation</groupId>
  <artifactId>validation-api</artifactId>
  <version>2.0.1.Final</version>
</dependency>

<dependency>
  <groupId>org.hibernate.validator</groupId>
  <artifactId>hibernate-validation</artifactId>
  <version>6.0.1.Final</version>
</dependency>
```
버전은 상황에 맞게 설정하면 된다.

### @Valid
@RequestBody를 통해 데이터를 바인딩하는 경우를 예시로 한다.  
다음과 같은 User 클래스가 있다고 하자.
```
public class User {
  private Integer id;
  private String account;
  private String password;
  
  public void setId(Integer id) {
    this.id = id;
  }
  
  public Integer getId() {
    return id;
  }
  
  public void setAccount(String accout) {
    this.account = account;
  }
  
  public String getAccount() {
    return account;
  }
  
  public void setPassword(String password) {
    this.password = password;
  }
  
  public String getPassword() {
    return password;
  }
}
```
유저 데이터를 서버측으로 보내게 되면 바인딩 형태는 다음과 같을 것이다.  
```
@RequestMapping(value = "/user/login", method = RequestMethod.POST)
public ResponseEntity login(@RequestBody User user) {
  //do something
}
```
만약 User의 account, password 속성값이 비어있는지 대해 유효성 검사를 하려면 다음과 같이  
해주면 된다.  

1. User 클래스의 accout, password 멤버 변수 상단에 제약조건 어노테이션을 선언한다.  
(더 많은 제약 조건 어노테이션을 알아보고 싶다면 javax.validation.constraints 에 있는  
어노테이션들을 살펴보면 된다.)
```
public class User {
  private Integer id;
  @NotEmpty
  private String accout;
  @NotEmpty
  private String password;
  
  ...
}
```
2. @RequestBody 옆에 @Valid 어노테이션을 추가한다.(@RequestBody의 오른쪽, 왼쪽 어디든 상관없다.)  
```
@RequestMapping(value = "/valid", method = RequestMethod.POST)
public ResponseEntity login(@RequestBody @Valid User user) {
  ...
}
```

3. 컨트롤러 메소드 파라미터로 BindingResult 타입 변수를 추가한다.
```
@RequestMapping(value = "/valid", method = RequestMethod.POST)
public ResponseEntity login(@RequestBody @Valid User user, BindingResult bindingResult) {
  ...
}
```
만약 위와 같이 세팅을 마치면 account 또는 password 데이터가 null인 상태로 request하게 되었을  
때 발생하는 상황은 다음과같다.

- @Valid가 선언되어 있으므로 User 클래스의 account, password의 제약조건(@NotEmpty)에 따라  
데이터 유효성을 검사한다.
- 모든 속성의 데이터가 유효하다면 아무일도 일어나지 않는다.
- 데이터가 유효하지 않는 속성이 있으면 그에 대한 에러 정보를 BindingResult 변수에 담는다.

BindingResult에서 모든 에러 정보를 얻으려면 getAllErrors() 메소드를 호출해야 한다.  
(기타 메소드들은 공식 문서를 참고하자.)  
ObjectError의 List 타입으로 리턴하며, 각 ObjectError 오브젝트들은 속성 하나의 에러 정보를 담고  
있으므로 에러 정보를 뽑아내어 상황에 맞게 코딩하면 된다.  

<예시>
```
@RequestMapping(value = "/user/login", method = RequestMethod.POST)
public ResponseEntity login(@RequestBody @Valid User user, BindingResult bindingResult) {
  //에러가 발생했다면
  if (bindingResult.hasErrors()) {
    List<ObjectError> errorList = bindingResult.getAllErrors();
    for (ObjectError error : errorList) 
      System.out.println(error.getDefaultMessage());
  }
}
```

### @Validated
@Validated 어노테이션은 기본적으로 @Valid와 기능이 같지만, 속성 제약조건에 대한 그룹을 만들어  
적용시킬 수 있다.  
**@Valid를 적용시킬때는 제약조건을 달아놓은 속성에 대해 전부 유효성 검사를 하게 되는데 만약  
제약조건은 그대로 선언해놓되 원하는 속성만 유효성 검사를 하고 싶은 경우에 사용하는 것이  
@Validated이다.**  
원하는 속성들만 유효성 검사를 하려면 먼저 그룹핑을 해놓아야 한다.   
속성에 대한 제약조건 그룹핑은 메소드 선언이 없는 [마커 인터페이스](https://woovictory.github.io/2019/01/04/Java-What-is-Marker-interface/)를 이용한다.  
예시는 위에서 설명했었던 User 클래스의 account, password 속성을 이용하여 설명한다.  
```
public class User {
  private Integer id;
  @NotEmpty
  private String accout;
  @NotEmpty
  private String password;
  
  ...
}
```
@RequestBody로 데이터 바인딩을 할 때 account에 대한 제약조건만 검사하게 하고 싶은 경우가  
있을 수 있고, password에 대한 제약조건만 검사하게 하고 싶은 경우가 있을 수도 있다.  
따라서 따로 그룹핑을 해줄 것이다.   

```
public class ValidationGroups {
  public interface group1 {};
  public interface group2 {};
}
```
```
public class User {
  private Integer id;
  @NotEmpty(groups = {ValidationGroups.group1.class})
  private String account;
  @NotEmpty(groups = {ValidationGroups.group2.class})
  private String password;
}
```
따로 ValidationGroups라는 클래스를 생성하여 각각 group1, group2라는 이름의 마커 인터페이스를  
선언하였고, User의 속성 제약조건 어노테이션에는 groups 속성에 각 인터페이스에 대한 클래스 정보를  
로드하게 하였다.   
  
위 과정까지 끝났으면 @RequestBody 옆에 @Validated를 선언하고 괄호를 열러 원하는 그룹을 넣어주면 된다.  
```
@RequestMapping(value = "/user/login", method = RequestMethod.POST)
public ResponseEntity login(@RequestBody @Validated(ValidationGroups.group1.class) User user, BindingResult bindingResult) {
  ...
}
```
코드를 보면 group1에 대해 효력이 발생하기 때문에 accout에 대한 유효성 검사만 진행될 것이다.  
만약 password만 유효성 검사를 하고 싶으면 group2로 바꾸어 주면 된다.  
  
위 예시는 속성 하나에 대해 그룹핑을 하였지만, 당연히 속성 여러개도 하나의 그룹핑으로 가능하다. 





  
  
  
  
  
  
  
  


































