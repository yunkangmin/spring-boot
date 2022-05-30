#### Swagger 사용하기
Swagger는 Rest API를 눈으로 보기 쉽게 그려주는 역할을 하고 있다.  
  
우선 pom.xml에 Dependency를 설정해 주어 사용할 수 있는 환경을 구성한다.
(참고로 2개의 버전이 다르면 에러가 날 수 있으니 버전을 동일하게 해주자.)

```
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger2</artifactId>
  <version>2.6.1</version>
</dependency>
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger-ui</artifactId>
  <version>2.6.1</version>
</dependency>
```

그 다음, Controller에 annotation을 추가한다.

```java
@GetMapping("/post")
@ApiOperation(value="사용자 정보 조회", notes="사용자 정보를 조회")
public List goodPersons() {
  return personRepository.findAll();
}

@PostMapping("/post")
@ApiOperation(value="사용자 정보 저장", notes="사용자 정보를 저장한다.")
public void registration(@ModelAttribute("personForm") Post postForm) {
    System.out.println(postForm);
    personService.savePost(postForm);
}
```

@ApiOperation는 Get, Post 방식 등 url 요청을 읽어드리는 부분에 작성하면 된다.

```java
@SpringBootApplication
@EnableSwagger2
public class BackedApplication {
  
  public static void main(String[] args) {
    SpringApplication.run(BackendApplication.class, args);
  }
  
  @Bean
  public Docket api() {
    return new Docket(DocumentationType.SWAGGER_2)
                    .select()
                    .apis(RequestHandlerSelectors.any())
                    .paths(PathSelectors.ant("/**"))
                    .build();
  }
}
```

BootApplication에 @EnableSwagger2를 붙여 Swagger를 이용할 수 있게끔 하자.  
안 붙이면 404 에러가 난다.  
  
@Bean으로 Docket을 설정하여, 어떠한 paths, apis 등 설정이 가능한데, 현재 설정한 것은  
어느 요청이든지 캐치하겠다라는 것이다. 만약 원하는 Controller만 보고 싶다면,  
Controller에 맨 위에 @RequestMapping("/resource") 이런 형식으로 어노테이션을 붙여놓고
paths에서 ("/\*\*")이 아닌, ("/resource/\*\*") 이런 식으로 붙여줘야 지정이 가능하다.

마지막으로 JPA 부분에 (domain, entity, model 등)

```java
@Data
@NoArgsConstructor
@Entity
@ApiModel
public class Post {
  public Post(String title) {
    this.title = title;
  }
  
  @Id
  @GeneratedValue
  @ApiModelProperty(value="아이디")
  private Long id;
  
  @ApiModelProperty(required = true, value="제목")
  private String title;
  
  @ApiModelProperty(required = true, value="내용")
  private String content;
}
```

@ApiModel, @ApiModelPropety를 설정해주자.
여기서 ID는 @GeneratedValue가 붙어있기 때문에 autoIncrease가 되므로 required = true가  
따로 필요가 없다.
(required = true로 해두면 꼭 값을 받아야지만 실행이 된다.)

그 후 Spring boot를 실행시킨후  
http://localhost:8080/swagger-ui.html에 들어가면

Rest API를 관리할 수 있는 화면이 나온다.
controller에 어노테이션을 붙여놨으므로 들어가 보면 
Controller의 세부 내용이 보이게 된다.  
get은 회원읽기용, post는 저장용이니 저장용으로 일단 데이터를 집어넣어 보자

<img src="/img/swagger.PNG" />
<img src="/img/swagger2.PNG" />
<img src="/img/swagger3.PNG" />
<img src="/img/swagger4.PNG" />


