스프링에서는 다양한 데이터베이스를 동일한 방식으로 사용하기 위해서  
추상화를 아주 잘 제공한다.  
  
따라서 Redis 또한 RedisRepository로 쉽게 사용할 수 있다. 

## application.properties
```
spring.redis.port = 6379
spring.redis.host = localhost
```
## AppConfig 클래스
```java
//설정 클래스로 사용
@Configuration
//프로퍼티 설정 파일을 application.properties로 사용
@PropertySource(valu = "application.properties")
//RedisRepository를 사용한다고 지정
@EnableRedisRepositories
public class AppConfg {
  
  @Value("${spring.redis.port}")
  private String port;
  @Value("${spring.redis.host}")
  private String host;
  
  @Bean
  public LettuceConnectionFactory redisConnectionFactory() {
    return new LettuceConnectionFactory(host, Integer.parseInt(port));
  }
  
  @Bean
  public RedisTemplate<?, ?> redisTemplate() {
    RedisTemplate<byte[], byte[]> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory());
    return template;
  }
}
```

## Person 클래스
```java
@RedisHash("people")
@Data
public class Person {
  
  @Id
  String id;
  String firstname;
  string lastname;
  Address address;
  
  public Person(String id, String firstname, string lastname, Address address) {
    this.id = id;
    this.firstname = firstname;
    this.lastname = lastname;
    this.address = address;
  }
}
```

## Address 클래스
```
@Data
public class Address {
  String country;
  String city;
  
  public Address(String country, String city) {
    this.country = country;
    this.city = city;
  }
}
```
Person 클래스에서 사용할 주소 클래스이다.
## Repository Interface
```
public interface PersonRepository extends CrudRepository<Person, String> {
}
```

## Repository Test
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = AppConfig.class)
public class PersonRepositoryTest {
  
  @Autowired
  PersonRepository repo;
  
  @Test
  public void basicCrudOperations(){  
    Address home = new Address("Korea", "Seoul");
    Person person = new Person(null, "chiman", "kim", home);
    
    Person savedPerson = repo.save(person);
    
    Optional<Person> findPerson = repo.findById(savedPerson.getId());
    
    assertThat(findPerson.isPresent()).isEqualTo(Boolean.TRUE);
    assertThat(findPerson.get().getFirstname()).isEqualTo(person.getFirstname());
  }
}
```




























    
  
  
