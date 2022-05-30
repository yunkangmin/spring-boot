AWS SDK for Java에서는 클라이언트 측 클래스를 Amazon DynamoDB 테이블로 매핑  
할 수 있는 DynamoDBMapper 클래스를 제공한다.   
DynamoDBMapper를 사용하려면 코드에서 DynamoDB 테이블의 항목과 해당하는  
객체 인스턴스 간의 관계를 정의한다. DynamoDBMapper 클래스를 당하는 객체  
인스턴스 간의 관계를 정의한다. DynamoDBMapper 클래스를 사용하면 테이블에  
액세스하여 다양한 생성, 읽기, 업데이트 및 삭제(CRUD) 작업을 수행할 뿐  
아니라 쿼리를 실행할 수도 있다.  
  
> #### 참고  
> DynamoDBMapper 클래스는 테이블 생성, 업데이트 또는 삭제를 허용하지 않는   
> 다. 이러한 작업을 위해서는 대신 하위 수준 SDK for Java 인터페이스를  
> 사용해야 한다. 자세한 내용은 섹션을 참조하자. [Java에서 DynamoDB 테이블  
> 작업](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/JavaDocumentAPIWorkingWithTables.html)  

SDK for Java는 클래스를 테이블로 매핑할 수 있도록 일련의 주석 유형을 제공  
한다. 예를 들어, ProductCatalog 테이블의 Id가 파티션 키인 경우   
```java
ProductCatalog(Id, ...)
```
아래 Java 코드와 같이 클라이언트 애플리케이션의 클래스를 ProductCatalog  
테이블로 매핑할 수 있다. 이 코드는 CatalogItem이라는 POJO(Plain Old Java  
Object)를 정의한다. 여기서는 주석을 사용하여 객체 필드를 DynamoDB 속성   
이름에 매핑한다.  
예)   
```java
@DynamoDBTable(tableName="ProductCatalog")
public class CatalogItem {
  
  private Integer id;
  private String title;
  private string ISBN;
  private Set<String> bookAuthors;
  private String someProp;
  
  @DynamoDBHashKey(attributeName="Id")
  public Integer getId() { return id; }
  public void setId(Integer id) { this.id = id; }
  
  @DynamoDBAttribute(attributeName="Title")
  public String getTitle() { return title; }
  public void setTitle(String title) { this.title = title; }
  
  @DynamoDBAttribute(attributeName="ISBN")
  public String getISBN() { return ISBN; }
  public void setISBN(String ISBN) { this.ISBN = ISBN; }
  
  @DynamoDBAttribute(attributeName="Authos")
  public Set<String> getBookAuthors() { return bookAuthors; }
  public void setBookAuthors(Set<String> bookAuthors) { this.bookAuthors = bookAuthors; }
  
  @DynamoDBIgnore
  public String getSomeProp() { return someProp; }
  public void setSomeProp(String someProp) { this.someProp = someProp; }
}
```
위의 코드에서 @DynamoDBTable 주석은 CatalogItem 클래스를 ProductCatalog  
테이블로 매핑한다. 각 클래스 인스턴스는 항목 형태로 이 테이블에 저장할  
수 있다. 클래스 정의에서 @DynamoDBHashKey 주석은 Id 속성을 기본 키로  
매핑한다.  
  
기본적으로 클래스 속성은 테이블에서 동일한 이름의 속성으로 매핑된다.   
예를 들어 Title 및 ISBN 속성은 테이블에서 동일한 이름의 속성으로 매핑된다.   
  
DynamoDB 속성의 이름이 클래스에서 선언된 속성의 이름과 일치하는 경우   
@DynamoDBAttribute 주석을 선택적으로 사용할 수 있다. 이러한 이름이 서로  
다르면 이 주석을 attributeName 파라미터와 함께 사용하여 이 속성에 대응하는  
DynamoDB 속성을 지정한다.  
  
위 예제에서는 속성 이름이 @DynamoDBAttribute에서 생성한 테이블과 정확히   
일치하면서 이 안내서의 다른 코드 예제에서도 일관된 속성 이름을 사용할 수   
있도록 [DynamoDB에서 테이블 생성 및 코드 예제에 대한 데이터 로드 주석](https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/DynamoDBMapper.html)이    
각 속성에 추가되었다.  
  
클래스 정의는 테이블의 어떤 속성에도 매핑되지 않는 속성을 가질 수도 있다.   
@DynamoDBIgnore 주석을 추가하여 이러한 속성을 확인할 수 있다. 위 예제  
에서는 SomeProp 속성이 @DynamoDBIgnore 주석으로 표시되어 있다. Catalog  
Item 인스턴스를 테이블에 업로드할 때 DynamoDBMapper 인스턴스에는 SomeProp  
속성이 포함되지 않는다. 또한 테이블에서 항목을 가져오더라도 매퍼가 이 속성  
을 반환하지 않는다.   
  
매핑 클래스를 정의한 후에는 DynamoDBMapper 메서드를 사용하여 해당 클래스의  
인스턴스를 Catalog 테이블의 해당 항목에 쓸 수 있다. 다음 코드 예제에서는  
이 과정을 보여준다.   
```java
AmazonDynamoDB client = AmazonDynamoDBClientBuilder.standard().build();

DynamoDBMapper mapper = new DynamoDBMapper(client);

CatalogItem item = new CatalogItem();
item.setId(102);
item.setTitle("Book 102 Title");
item.setISBN("222-22222222");
item.setBookAuthors(new HashSet<String>(Arrays.asList("Author 1", "Author 2")));
item.setSomeProp("Test");

mapper.save(item);
```
다음 코드 예제에서는 항목을 검색하고 일부 속성에 액세스하는 방법을 보여준다. 
```kotlin  
CatalogItem partitionKey = new CatalogItem();

partitionKey.setId(102);
DynamoDBQueryExpression<CatalogItem> queryExpression = new DynamoDBQueryExpression<CatalogItem>()
  .withHashkeyValues(partitionKey);
  
List<CatalogItem> itemLit = mapper.query(CatalogItem.class, queryExpression);

for (int i = 0; i < itemList.size(); i++) {
  System.out.println(itemList.get(i).getTitle());
  System.out.println(itemList.get(i).getBookAuthors());
}
```
DynamoDBMapper는 Java에서 DynamoDB 데이터를 사용하는 직관적이고 자연스러  
운 방법을 제공한다. 또한 낙관적 잠금, ACID 트랜잭션, 자동 생성된 파티션  
키 및 정렬 키 값, 객체 버전 관리 등 기본적인 여러 기능을 제공한다.   




































