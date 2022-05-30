해당 내용는 [공식문서](https://spring.io/projects/spring-restdocs) 2.0.5.RELEASE   
버전을 참고 및 토대로 작성했다.   

# Overview
Spring REST Docs는 RESTful 서비스의 문서화를 도와주는 도구이다. Spring REST Docs는  
문서 작성 도구로 기본적으로 Asciidoc를 사용하며, 이 것을 사용해 HTML을 생성한다.  
필요한 경우 Markdown을 사용하도록 변경할 수 있다.  
  
Spring REST Docs는 Spring MVC의 테스트 프레임워크, Spring WebFlux WebTestClient  
또는 Rest Assured 3로 작성된 테스트 코드에서 생성된 Snippet을 사용한다.  
테스트 기반 접근 방식은 서비스 문서의 정확성을 보장해준다. Snippet이 올바르지  
않을 경우 테스트가 실패하기 때문이다.    
  
RESTful 서비스를 문서화하는 것은 해당 리소스를 설명하는 것이다. 리소스 설명의 핵심은  
HTTP 요청과 HTTP 응답의 세부정보이다. Spring REST Docs를 사용하면 리소스와  
HTTP 요청 및 응답을 사용하여 서비스 내부의 세부 정보로부터 문서를 보호할 수 있다.   
즉, 서비스 내부 코드에 추가 또는 수정과 같이 영향을 주지 않고도 별도의 문서화  
작업을 진행할 수 있게 된다. 이는 서비스 구현보다 API를 문서화하는데 도움이 된다.  
또한 문서를 재 작업하지 않고도 버전에 따라 변경할 수 있다.  

---
많이 사용되는 RESTful API 문서도구로는 대표적으로 Swagger가 있다. 아래 표는  
Swagger와 Spring REST Dosc의 비교 내용이다.   
(Spring REST Docs | Swagger)   
- 코드의 추가 및 수정이 없다. | 코드에 어노테이션등을 추가해야 한다.  
- 테스트 코드 작성이 필요하면, 테스트 성공 시 문서가 생성된다. | 테스트 코드없이  
서비스 쪽 코드 및 어노테이션 추가로 문서를 생성할 수 있다.  
- 버전 변화에 유연하고 정확성이 높다 | 버전 변화에 맞춰 재작성해야 하며 이를 하지  
않을 때 정확성이 낮다.  

## 빌드 시스템 설정
Gradle  
```
plugins {
  //Asciidoctor 플러그인 적용
  id "org.asciidoctor.convert" version "1.5.9.2"
}

dependencies {
  /*
    asciidocker에 대한 spring-restdocs-asciidoctor 의존성 추가
    Maven 처럼 build/generated-snippets 밑에 생성된 Snippet을 .adoc 파일이 
    자동으로 가리키도록 하는 설정 추가.
    operation 블록 매크로 사용 가능
  */
  asciidoctor 'org.springframework.restdocs:spring-restdocs:asciidoctor:{project-version}'
  //Maven과 같이 test Scope에 대한 mockMvc 의존성을 추가(WebClient, Assured 사용가능)
  testCompile 'org.springframework.restdocs:spring-restdocs-mockmvc:{project-version}'
}

ext {
  //Snippet의 생성 위치를 지정
  snippetsDir = file('build/generated-snippets')
}

test {
  //Snippets 디렉토리를 출력으로 작업하도록 설정
  outputs.dir.snippetsDir
}

asciidoctor {
  //Snippets 디렉토리를 Input 디렉토리로 설정
  inputs.dir.snippetsDir
  //문서 생성 전 테스트가 실행되도록 test에 종속 설정
  dependsOn test
}
```

## 문서 패키징
생성된 API 문서를 jar 또는 war에 패키징하기 위한 설정이다. 이는 Spring Boot에  
의해 정적 콘텐츠로 제공된다.  
1. jar가 빌드되기 전에 문서가 생성
2. 생성된 문서는 jar에 포함

Gradle   
```
bootJar {
  //빌드 전 문서 생성 확인
  dependsOn asciidoctor
  //생성된 문서를 static/docs에 복사
  from("${asciidoctor.outputDir}/html5") {
    into 'static/docs'
  }
}
```

## 문서 조각 생성
Spring REST Docs는 테스트 프레임워크, WebTestClient 등을 사용하여 문서화에  
필요한 서비스에 요청을 한다. 그 다음 요청 및 결과 응답에 대한 문서 Snippet을  
생성한다.  

### 테스트 설정
테스트 프레임워크에 따라 다르다. 여기에서는 JUnit 5에 대한 설정을 다룬다.  

- JUnit 5 테스트 설정
   - RestDocumentationExtension을 테스트 클래스에 적용한다.  
   ```
   @ExtendWith(RestDocumentationExtension.class)
   public class JUnit5ExampleTests() {}
   ```  
   RestDocumentationExtension은 Maven의 경우 "target/generated-snippets"  
   Gradle의 경우 "build/generated-snippets"를 자동으로 출력 디렉토리로  
   설정한다.
   - 일반적인 Spring Application의 경우에는 SpringExtention을 적용한다.  
   ```
   @ExtendWith({RestDocumentationExtension.class, SpringExtension.class})
   public class JUnit5ExampleTests {}
   ```

JUnit 5.1을 사용하는 경우 클래스의 필드로 등록해서 출력 디렉토리를 임의로  
지정할 수 있다.  
```
public class JUnit5ExampleTests {
  
  @RegisterExtension
  final RestDocumentationExtension restDocumentaton = new RestDocumentationExtension("custom");
}
```
다음은 MockMvc를 사용한 설정 방법이다.  
```
private MockMvc mockMvc;

@BeforeEach
public void setUp(WebApplicationContext webApplicaitonContext,
                RestDocumentationContextProvider restDocumentation) {
  this.mockMvc = MockMvcBuilder.webAppContextSetup(webApplicationContext)
    .apply(documentationConfiguration(restDocumentation))
    .alwaysDo(document("{method-name}", preprocessRequest(prettyPrint()), preprocessResponse(prettyPrint())))
    .build();
}
```
documentationConfiguration은 기본값이 있지만 사용자 정의를 위한 다양한 설정도  
제공한다. 내용은 [여기](https://docs.spring.io/spring-restdocs/docs/2.0.5.RELEASE/reference/html5/#configuration)를 참고한다.  

---
추가로 alwayDo() 메서드에서 문서화로 보여줄 때 이쁘게 보여주기 위한 옵션도 추가한다.  

### JUnit없이 테스트 설정
- JUnitRestDocumentation 대신 ManualRestDocumentation을 사용
- @Rule 어노테이션 제거
```
private MockMvc mockMvc;

@Autowired
private WebApplicationContext context;

private ManualRestDocumentation restDocumentation = new ManualRestDocumentation();

@BeforeMethod
public void setUp(Method method) {
  this.mockMvc = MockMvcBuilders.webAppContextSetUp(this.context)
                    .apply(documentationConfiguration(this.restDocumentation))
                    .build()
  this.restDocumentation.beforeTest(getClass(), method.getName());
}
```  
마지막으로 각 테스트 후에 ManualRestDocumentation.afterTest를 호출해야 한다.  
```
@AfterMethod
public void tearDown() {
  this.restDocumentation.afterTest();
}
```
### RESTful 서비스 호출
RESTful 호출 예제  
```
//서비스의 루트("/")로 요청하고 응답 타입은 application/json으로 한다.  
this.mockMvc.perform(get("/").accept(MediaType.APPLICATION_JSON))
  //응답 확인
  .andExpect(status().isOk())
  //서비스 호출을 문서화 하는데 Snippet 이름을 index로 지정.
  //Snippet은 "RestDocumentationResultHandler"에 의해 작성되며, 클래스의 인스턴스는
  //"org.springframework.restdocs.mockmvc.MockMvcRestDocumentation"의 "document"
  //메서드를 참고한다.  
  .andDo(document("index"));
```
기본적으로 6개의 Snippet이 작성된다. 그 외에 Snippet에 대한 정보는 [여기](https://docs.spring.io/spring-restdocs/docs/2.0.5.RELEASE/reference/html5/#documenting-your-api)를  
참고한다.
- <output-directory>/index/curl-request.adoc
- <output-directory>/index/http-request.adoc
- <output-directory>/index/http-response.adoc
- <output-directory>/index/httpie-request.adoc
- <output-directory>/index/request-body.adoc
- <output-directory>/index/response-body.adoc  

## Snippet 사용
테스트 후 생성된 Snippet을 사용하기 위해 .adoc 파일을 생성해야 한다.  
파일명은 아무렇게해도 상관없다. 그리고 생성된 snippet을 연결해주면 된다.   
```
include::{snippets}/index/curl-request.adoc[]
```
만들어 놓은 .adoc과 생성된 Snippet의 연결이 끝났다면 maven install 또는   
gradle build로 .html 파일을 생성해준다.  
(빌드도구 | 소스 파일 | 생성된 파일)  
- Maven | src/main/asciidoc/*.adoc | target/generated-docs/*.html
- Gradle | src/docs/asciidoc/*.adoc | build/asciidoc/html5/*.html
  
# Simple Example
코드는 [여기](https://github.com/subji/spring-rest-docs-example)를 참고한다. 

## 테스트 코드 작성
```
@ExtendWith({ RestDocumentationExtension.class, SpringExtension.class })
@SpringBootTest(classes = ExampleSpringRestDocsApplication.class)
class ExampleSprintRestDocsApplicationTests {
    
  private MockMvc mockMvc;
  
  @Test
  void contextLoads() {
  }
  
  @BeforeEach
  public void setUp(
            WebApplicationContext webApplicationContext,
            RestDocumentationContextProvider restDocumentationContextProvider) {
    this.mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext) 
            .apply(documentationConfiguration(restDocumentationContextProvider))
            .alwary
  
  
  
   































