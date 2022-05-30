## Spring에서 @ConditionalOnClass, @ConditionalOnBean 사용할 때 주의할 점
Spring Boot 기반의 자동 설정을 위한 AutoConfiguration 클래스를 만들다가 발생한 이슈를  
다룬다.  

### @Conditional이란?
스프링4에서 도입된 어노테이션으로 조건부로 Bean을 스프링 컨테이너에 등록하는 역할을 한다.  
이 어노테이션은 Condition Interface를 사용하여 특정 조건부로 등록되도록 만들 수 있다.  
그리고 현재의 스프링 프레임워크에서는 미리 정의된 Condition Interface 구현체를 가지고 있는  
@Conditional 어노테이션의 파생 어노테이션들이 있다. 
주의점을 적을 어노테이션은 아래 어노테이션들이다.
- @ConditionalOnClass : 특정 Class 파일이 존재하면 Bean을 등록한다.
- @ConditionalOnBean : 특정 Bean이 존재하면 Bean을 등록한다.  

두 어노테이션의 특징은 어노테이션 메타데이터로 Class를 입력받는 다는 것이다.  
(두 어노테이션 말고도 더 있을 수 있다.)
```
@ConditionalOnClass(Test.class)
...

@ConditionalOnBean(Test.class)
...
```

### @ConditionalOnClass를 왜 쓰는지 집고 넘어가자
@ConditionalOnClass는 해당 class가 있는지를 검사하는 조건 어노테이션이다.  
Class가 있는지 왜 검사를 할지 생각하면 답이 나온다.  
1. 이 어노테이션이 작성되는 클래스는 외부 라이브러리로 제공되는 프로젝트이다.  
하나의 독립 실행형 프로젝트의 Configuration에 ConditionalOnClass를 작성할 일은 없다.  
일단 의존성을 독립 실행형 프로젝트에서 이미 확정짖기 때문에 클래스가 없을 일이 없다.  
그리고 독립 실행형 프로젝트에서 ConditionalOnClass에 Class를 지정하는데, Class가 없다면  
Compile Erroe로 빌드부터 문제이다.
  
2. 이 어노테이션이 작성된다는 것은 해당 클래스가 사용되는 프로젝트에서 해당 Class가 있을 수도  
있고 없을 수도 있다는 의미가 된다. 이 것은 해당 클래스가 작성되는 프로젝트는 해당 프로젝트를  
참조하는 프로젝트의 의존성에 영향을 주지 않는다는 말이다(이 Class가 있어도 그만 없어도 그만이라는  
이야기같다). 
> Spring Boot의 AutoConfiguration을 살펴보면, 모든 선택적 의존성에 Dependency Optional True가  
> 붙어있는 것을 확인할 수 있다. Gradle에서는 complieOnly를 사용할 수 있다. 대부분의 선택적  
> 의존성들은 Starter Pack에 의하여 주입된다.

### @ConditionalOn* 어노테이션에서 지정한 Class가 없을 때 주의해야 할 점
>@ConditionalOnClass를 제외한 다른 @ConditionalOn* 어노테이션에서 Class가 없는 상황은  
>ConditionalOnClass 자체가 작성되는 것이 누락되었을 가능성이 있으며, 아래 설명드릴  
>주의사항과 해결 방법이 동일하므로 모든 @ConditionalOn* 어노테이션에서 동일하므로  
>@ConditionalOnClass 어노테이션을 중심으로 작성한다.  

#### ConditionalOnClass가 Class가 없을 때를 위한 조건 어노테이션인데, 문제가 있다니!  

### Method에 ConditionalOnClass를 사용할 때 Exception!

```
//TestBean 클래스가 없을 때
@Configuration
public class TestConfiguration {
  @Bean
  @ConditionalOnClass(TestBean.class)
  public TestBean testBean() {
    return new TestBean();
  }
}
```
위 코드는 ClassNotFoundException이 발생한다.
```
//TestBean 클래스는 있고 Test클래스가 없을 때
@Configuration
public class TestConfiguration {
  @Bean
  @ConditionalOnClass(Test.class) 
  public TestBean testBean() {
    return new TestBean();
  }
}
```
위 코드는 ArrayStoreException(TypeNotPresentExceptionProxy)이 발생한다.  

### 왜 Exception이 발생하는가?
ConfigurationClassPostProcessor의 동작과정을 이해하면 답은 간단하다.  
ConfigurationClassPostProcessor Class는 @Configuration 어노테이션이 적용된 빈 객체에서  
@Bean 어노테이션이 적용된 메서드로부터 빈 객체를 가져와 스프링 컨테이너에 등록하는 역할을  
한다. @Bean 어노테이션이 적용된 메서드로부터 빈 객체를 가져와 스프링 컨테이너에 등록하는  
역할을 한다.  
ConfigurationClassPostProcessor는  
1. ConfigurationClassParser를 통해 먼저 Class의 생성 여부를 Conditional 어노테이션들을  
포함한 특수한 과정을 통해 정의한다.  
2. 위 과정을 통과한 Configuration Class는 ConfigurationClassEnhancerConfiguration을 통해  
Enhance라는 ByteCode를 읽어들이는 과정을 거쳐 Class를 로드하게 된다.  

위 예시에서 작성하였던 Exception은 모두 이 과정에서 발생한다.  
ClassNotFoundException  
Enhance 과정에서 Annotation 내부에 정의된 Class는 문제가 되지 않는다.  
이 Exception은 필드를 해석하는 과정에서 알 수 없는 Class가 존재하기 때문에 발생한다.  
우리가 Compile할 때 보는 Exception과 같은 Exception이다.  
  
ArrayStoreException - TypeNotPresentExceptionProxy  
이 Exception은 어노테이션에 정의해놓은 존재하지 않는 Class때문에 발생한다.  
위에서 분명 enhance 과정에서 annotation은 문제가 되지 않느다고 했는데??  
더 자세히 들어가서 보면 이 Exception은 Reflection의 isAnnotationPresent 때문에  
발생한다. ConfigurationClassPostProcessor는 Class를 enhance하고 난 후 Bean을  
등록하기 위해 Method를 enhance하는 과정을 거친다. 이 때 해당 Method가 Bean 객체를  
등록하기 위한 Method인지를 확인하기 위해 isAnnotationPresent(Bean.class)를 사용한다.  
그리고 isAnnotationPresent은 해당 Method의 전체 정보를 조사하게 되고, 그 과정에서  
ArrayStoreException이 발생하게 되는 것이다.   
  
ConfigurationClassPostProcessor가 원인이 아닌 사례도 있다.  
Configuration Class 자체가 WebApplicationInitializer를 implements하여 SpringServlet  
ContainerInitializer가 Class 전체를 조사하여 위와 같은 Exception이 발생한 사례도 있다. 

### 해결방법
1. Configuration Class 내부 코드에서 사용될 선택적 의존성 Class는 반드시 Class에 작성
```
@Configuraition
@ConditionalOnClass(TestBean.class)
public class TestConfiguration {
  @Bean
  public TestBean testBean() {
    return new TestBean();
  }
}
```

2. ConditionalOn*에서 사용될 Class가 선택적 의존성 CLass이면 Class에 작성  
전체 설정에 동일하게 적용될 수 있는 조건이라면 Class레벨로 옮기는 방법이 있다.  
```
@Configuration
@ConditionalOnClass(TestCondition.class)
public class TestConfiguration {
  @Bean
  public TestBean testBean() {
    return new TestBean();
  }
}
```
전체 설정에 동일하게 적용될 수 없다면,  내부 혹은 외부 Class로 분리하는 것을  
생각해볼 수도 있다.   
```
@Configuration
@ConditionalOnClass(TestCondition.class) 
public class TestAConfiguration {
  @Bean
  public TestBeanA testBean() {
    return new TestBean();
  }
}

@Configuration
@ConditionalOnClass(TestConditionB.class) 
public class TestBConfiguration {
  @Bean
  public TestBeanB testBeanB() {
    return new TestBeanB();
  }
}
```
3. Class 지정 대신 String을 사용  
Class를 찾지 못하는 것만 우회하면 되므로, String으로 작성하는 방벙이 있다.  
실제 많은 스프링 부트 이슈에서 이 방법으로 해결을 하고 있다.  
```
@Configuration
public class TestConfiguration {
  @Bean
  @ConditionalOnClass(name = "com.external.lib.TestConditionA")
  public TestBeanA testBeanA() {
    return new TestBeanA();
  }
  
  @Bean
  @ConditionalOnClass(name = "com.external.lib.TestConditionB")
  public TestBeanB testBeanB() {
    return new TestBeanB();
  }
}
```

   

  









































