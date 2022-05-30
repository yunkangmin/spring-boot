# DynamoDBMapper의 구성설정  
DynamoDBMapper의 인스턴스를 생성할 때는 일정한 기본 동작이 있지만 DynamoDB  
MapperConfig 클래스를 사용하면 이러한 기본 동작을 재정의할 수 있다.   
  
다음은 사용자 지정 설정으로 DynamoDBMapper를 생성하는 코드 조각이다.   
```java
AmazonDynamoDB client = AmazonDynamoDBClientBuilder.standard().build();

DynamoDBMapperConfig mapperConfig = DynamoDBMapperConfig.builder()
   .withSaveBehavior(DynamoDBMapperConfig.SaveBehavior.CLOBBER)
   .withConsistentReads(DynamoDBMapperConfig.ConsistentReads.CONSISTENT)
   .withTableNameOverride(null)
   .withPaginationLoadingStrategy(DynamoDBMapperConfig.PaginationLoadingStartegy.EAGER_LOADING)
 .build()

DynamoDBMapper mapper = new DynamoDBMapper(client, mapperConfig);
```

---

# spring-data-dynamodb로 쿼리 메서드 사용하기   
spring-data-dynamodb의 최신버전은 5.1.0이다. 이 버전부터 Spring Boot 2.1  
을 지원하지만 문제가 있어 실제로는 spring-boot가 2.1.4부터 spring-data-  
dynamodb 5.1.0을 사용할 수 있다.   
  
spring-boot의 버전이 2.1.0 ~ 2.1.3이고, 2.1.4로 업그레이드를 할 수 없는  
상황이라면 dynamoDB-dynamoDBMapper빈을 직접 정의해 오버라이딩하는 방법  
으로 spring-data-dynamodb 5.1.0을 사용할 수 있다.  
  
spring-boot 2.0대를 사용하고 있다면, spring-data-dynamodb 5.0.4를 사용  
하면 된다. spring-data-dynamodb의 버전에 따라 설정이 조금 다르다.  
5.1.0에서 설정이 조금 더 간단해졌다. 여기서는 spring-boot 2.1.4와 spring-  
data-dynamodb 5.1.0을 사용한다.   
  
guestbook-api-comments/build.gradle의 dependencies에 implementation "   
com.github.derjust:spring-data-dynamodb:5.1.0"을 추가하고, guestbook-  
api-comments/src/main/java/guestbook/comments/config/DynamoDbConfig.java  
에 다음과 같이 설정했다.  
```java
//spring-data-dynamodb 5.1.0
@Configuration
@EnableDynamoDBRepositories(basePackages = "guestbook.comments.domain")
public class DynamoDbConfig {
  @Value("$")
  private String amazonDynamoDbEndpoint;
  
  @Value("$")
  private String amazonDynamoDbRegion;
  
  @Value("$")
  private String amazonAwsAccessKey;
  
  @Value("$")
  private String amazonAwsSecretKey;
  
  @Primary
  @Bean
  public DynamoDBMapper dynamoDbMapper(AmazonDynamoDB amazonDynamoDb) {
    return new DynamoDBMapper(amazonDynamoDb, DynamoDBMapperConfig.DEFAULT);
  }
  
  @Bean
  public DynamoDBMapper dynamoDBMapper(AmazonDynamoDB amazonDynamoDb, DynamoDBMapperConfig config) {
    return new DynamoDBMapper(amazonDynamoDb, config);
  }
  
  @Bean(name = "amazonDynamoDB")
  public AmazonDynamoDB amazonDynamoDb() {
    AWSStaticCredentialsProvider credentialsProvider = new AWSStaticCredentialsProvider(
      new BasicAWSCredentials(amazonAwsAccessKey, amazonAwsSecretKey));
    EndpointConfiguration endpointConfiguration = 
      new EndpointConfiguration(amazonDynamoDbEndpoint, amazonDynamoDbRegion);
    
    return AmazonDynamoDBClientBuilder.standard()
            .withCredentials(credentialsProvider)
            .withEndpointConfiguration(endpointConfiguration)build();
  }
  //LocalDateTime Converter 생략
}
```
DynamoDbConfig 클래스에 @Configuration과 @EnableDynamoDBRepositories를  
달아주고 Bean들을 작성하면 된다. @Value 어노테이션에 필요한 값들은  
application.yml에 정의되어 있다.   
```
amazon:
  dynamodb:
    endpoint: "http://localhost:8000"
    region: "ap-northeast-2"
  aws:
    accesskey: "key"
    secretkey: "key2"
```
@Primary가 달려있는 DynamoDBMapper Bean은 사실 spring-data-dynamodb 내부  
에서 dynamoDB-DynamoDBMapper라는 이름으로 알아서 생성해서 사용한다. 하지만   
CommentRespository 만으로는 테이블을 생성하거나 삭제할 수 없기 때문에   
@Primary를 붙여 재정의해 우리가 직접 DynamoDBMapper를 조작할 수 있도록  
했다. 테이블 생성, 삭제를 하지 않아도 되면 DynamoDBMapper를 재정의하지 않  
아도 된다.   

---

# DynamoDBMapper.query
테이블 또는 보조 인덱스를 쿼리한다. 테이블에 복합 기본 키(파티션 키 및   
정렬 키)가 있는 경우에만 테이블 또는 인덱스를 쿼리할 수 있다. 이 메서드를  
실행하려면 파티션 키 값을 비롯해 정렬 키에 적용되는 쿼리 필터를 입력해야   
한다. 필터 표현식에는 조건과 값이 추가된다.   
  
포럼 스레드 회신이 저장되는 Reply 테이블이 있는 경우 스레드 제목마다 회신  
수가 0개 또는 그 이상이 될 수 있다. Reply 테이블의 기본키는 Id 및 Reply  
DateTime 필드로 구성된다. 여기에서 Id는 파티션 키이고, ReplyDateTime은  
기본 키의 정렬 키이다.  
```
Reply(Id, ReplyDateTime, ...)
```
Reply 클래스와 DynamoDB의 해당 Reply 테이블 간 매핑을 생성한 경우 다음  
Java 코드는 DynamoDBMapper를 사용하여 특정 스레드 제목에 대한 지난 2주 간  
모든 회신을 찾는다.  
예)  
```java
String forumName = "&DDB;";
String forumSubject = "&DDB; Thread 1";
String partitionKey = forumName + "#" + forumSubject;

long twoWeeksAgoMilli = (new Date()).getTime() - (14L * 24L * 60L * 60L * 1000L);
Date twoWeeksAgo = new Date();
twoWeeksAgo.setTime(twoWeeksAgoMilli);
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:SSS'Z'");

Map<String, AttributeValue> eav = new HashMap<String, AttributeValue>();
eav.put(":v1", new AttributeValue().withS(partitionKey));
eav.put(":v2", new AttributeValue().withS(twoWeeksAgoStr.toString()));

DynamoDBQueryExpression<Reply> queryExpression = new DynamoDBQueryExpression<Reply>()
    .withKeyConditionExpression("Id = :v1 and ReplyDateTime > :v2")
    .withExpressionAttributeValues(eav);
    
List<Reply> latestReplies = mapper.query(Reply.class, queryExpression);
```
쿼리결과 Reply 객체 컬렉션이 반환된다.   
  
기본적으로 query 메서드는 "지연 로딩된(lazy-loaded)" 컬렉션을 반환한다.  
즉, 처음에는 결과 페이지를 하나만 반환하고, 필요에 따라 서비스를 호출하여  
다음 페이지를 반환한다. 일치하는 항목을 모두 가져오려면 latestReplies  
컬렉션을 반복한다.  
  
컬렉션에서 size() 메서드를 호출하면 정확한 개수를 제공하기 위해 모든   
결과가 로드된다. 이로 인해 프로비저닝된 처리량이 많이 사용될 수 있으며   
매우 큰 테이블에서는 JVM의 모든 메모리가 소모될 수도 있다.  
  
인덱스를 쿼리하려면 먼저 인덱스를 매퍼 클래스로 모델링해야 한다. Reply  
테이블에는 Posted-Message-Index라는 글로벌 보조 인덱스가 있다. 이 인덱스  
의 파티션 키는 PostedBy이고, 정렬 키는 Message이다. 이 인덱스에서 항목의  
클래스 정의는 다음과 같을 것이다.  
```java
@DynamoDBTable(tableName="Reply")
public class PostedByMessage {
  private String postedBy;
  private String messgae;
  
  @DynamoDBIndexHashKey(globalSecondaryIndexName = "PostedBy-Message-Index", attributeName = "PostedBy")
  public String getPostedBY() { return postedBy; }
  public void setPostedBy(String postedBy) { this.postedBy = postedBy; }
  
  @DynamoDBIndexRangeKey(globalSecondaryIndexName = "PostedBy-Message-Index", attributeName = "Message")
  public String getMessage() { return message; }
  public void setMessage(String message) { this.message = message; }
  
  //Additional properties go here
}
```
@DynamoDBTable 주석은 이 인덱스가 Reply 테이블과 연결되어 있음을 나타  
낸다. @DynamoDBIndexHashKey 주석은 인덱스의 파티션키(PostedBy)를 표시  
하고, @DynamoDBIndexRangeKey는 인덱스의 정렬 키(Message)를 표시한다.   
  
이제 DynamoDBMapper를 사용하여 특정 사용자가 게시한 메시지 하위 집합을  
검색하는 인덱스 쿼리를 실행할 수 있다. DynamoDB가 어느 인덱스를 쿼리할지  
알 수 있도록 withIndexName을 지정해야 한다. 다음 코드에서는 글로벌 보조   
인덱스를 쿼리한다. 글로벌 보조 인덱스는 최종적 일관된 읽기는 지원하지만   
강력히 일관된 읽기는 지원하지 않으므로 withConsistentRead(false)를 지정  
해야 한다.  
```java
HashMap<String, AttributeValue> eav = new HashMap<String, AttributeValue>();
eav.put(":v1", new AttributeValue().withS("User A"));
eav.put(":v2", new AttributeValue().withS("DynamoDB"));

DynamoDBQueryExpression<PostedByMessage> queryExpression = new DynamoDBQueryExpression<PostedByMessage>()
  .withIndexName("PostedBy-Message-Index")
  .withConsistentRead(false)
  .withkeyConditionExpression("PostedBy = :v1 and begins_with(Message, :v2)")
  .withExpressionAttributeValues(eav);

List<PostedByMessage> iList = Mapper.query(PostedByMessage.class, queryExpression);
```
쿼리 결과 PostedByMessage 객체 컬렉션이 반환된다.  

---

# load
테이블에서 항목을 가져온다. 검색할 항목의 기본 키를 제공해야 한다.   
DynamoDBMapperConfig 객체를 사용하여 옵션으로 구성 파라미터를 입력할 수도  
있다. 예를 들어 아래 Java 문과 같이 이 메서드를 실행하여 최신 항목 값만  
가져오려면 옵션으로 strongly consistent read를 요청하면 된다.  
```java
CatalogItem item = mapper.load(CatalogItem.class, new DynamoDBMapperConfig(DynamoDBMapperConfig.ConsistentReads.CONSISTENT));
```
기본적으로 DynamoDB는 최종적으로 일관된 값을 갖는 항목을 반환하기 때문  
이다. DynamoDB의 최종 일관성 모델에 대한 자세한 내용은 [읽기 일관성](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)  
단원을 참조하자.  


https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/DynamoDBMapper.Methods.html#DynamoDBMapper.Methods.query





















