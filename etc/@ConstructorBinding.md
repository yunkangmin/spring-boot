## @ConstuctorBinding
@ConstructorBinding 어노테이션은 @ConfigurationProperties 어노테이션의 주석 내용을 보면

>Annotation for Externalized configuration. Add this to a class definition or a  
>@Bean method in a @Configuration class if you want to bind and validate some  
>external Properties (e.g. from a .properties file).  
>Binding is either performed by calling setters on the  
>annotated class or, if @ConstuctorBinding is in use, by   
>binding to the constructor parameters.

>번역  
>외부화 구성에 대한 주석. 일부 외부 속성(예: .properties 파일에서)을  
>바인딩하고 유효성을 검사하려면 @Configuration 클래스의 클래스 정의 또는  
>@Bean 메서드에 이것을 추가합니다.  
>바인딩은 setter를 호출하여 수행됩니다.  
>주석이 달린 클래스 또는 @ConstuctorBinding이 사용 중인 경우  
>생성자 매개변수에 바인딩

>정리  
>@ConfigurationProperties를 사용하면 setter를 통해 바인딩되고  
>@ConstructorBinding을 사용하면 생성자 매개변수를 통해 바인딩한다?

ConstructorBinding이 사용중이라면 생성자 파라미터 바인딩을 한다고 한다.  
실제로 PostgresProperties 클래스를 컴파일했을 때는 아래와 같이 클래스파일이  
생성되는 것을 볼 수 있다. (생성자가 자동으로 생긴다) 

### 사용이유
Spring Boot 2.3 버전 이전에는 불변성을 지킬 수 없는 문제가 있었기 때문에  
생성자 주입방식으로 불변성을 가지고 Properties 파일을 만들 수 있는 방식이 추가되었다.  
  
@ConstructorBinding 어노테이션을 이용하면 final 필드에 대해 값을 주입해준다.  
그리고 중첩 클래스가 있으면 자동으로 중첩 클래스의 final 필드 또한 자동으로 값을  
주입하는 대상이 된다.  

final 키워드를 명시하지 않는다면 setter를 이용해서 값을 binding하려하기 때문에  
setter가 없다는 exception이 발생한다. 

