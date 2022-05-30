Mockito를 이용한 테스트 코드 작성 시 발생한 이슈를 간단히 정리하고자 한다.  
```
@Table(name = "category")
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Category extends BaseEntity {
  
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
  
  @Column(name = "name")
  private String name;
  
  @Column(name = "parent_id")
  private Long parenId;
  
  @Builder
  public Catagory(String name, Long parentId) {
    this.name = name;
    this.parentId = parendId;
  }
}
```
- 클래스 상단에 @Builder를 선언하면 IDENTITY 전략을 설정한 기본키 값을 할당할 수 있기에  
안전하지 않다.  
- 따라서, 생성자에 @Builder를 선언하여 엔티티 생성시 필요한 데이터만 받을 수 있도록  
작성했다.   
- 추가적으로 엔티티에는 @Setter 메소드를 선언하면 좋지 않다. 엔티티 혹은 객체는   
불변한 성질을 가져야 하기 때문이다. 만약 중간에 변결될 요지를 준다면 추후 코드추적  
하기가 어렵다.  
```
@ExtendWith(MockitoExtension.class)
class CategoryServiceTest {
  
  @Mock
  private CategoryRepository categoryRepository;
  
  @Test
  @DisplayName("카테고리 조회")
  void categoryOneDepthSelect() {
    given(categoryRepository.findAll()).willReturn(getStubCategories());
    CategoryService categoryService = new CategoryService(categoryRepository);
    
    CategoryDto categoryRoot = categoryService.categoryRoot();
    
    System.out.println(categoryRood());
    
    assertThat(categoryRoot.getSubCategories().size()).isEqualTo(2);
    assertThat(categoryRoot.getSubCategories().get(0).getSubCategories().size()).isEqualTo(2);
    assertThat(categoryRoot.getSubCategories().get(1).getSubCategories().size()).isEqualTo(2);  
    
    private List<Category> getStubCategories() {
      Category sub1 = new Category("SUB1", 0L);
      Category sub2 = new Category("SUB2, 0L);
      Category sub11 = new Category("SUB1-1", 0L);
      Category sub12 = new Category("SUB1-2", 0L);
      Category sub21 = new Category("SUB2-1", 0L);
      Category sub22 = new Category("SUB2-2", 0L);
      
      //ReflectionTestUtils를 이용하여 private 필드값에 직접 할당할 수 있다.  
      //stubing 시 반환될 값 세팅
      ReflectionTestUtils.setField(sub1, "id", 1L);
      ReflectionTestUtils.setField(sub2, "id", 2L);
      ReflectionTestUtils.setField(sub11, "id", 3L);
      ReflectionTestUtils.setField(sub12, "id", 4L);
      ReflectionTestUtils.setField(sub21, "id", 5L);
      ReflectionTestUtils.setField(sub22, "id", 6L);
      
      List<Category> categories = List.of(
        sub1, sub2, sub11, sub12, sub21, sub22
      );
      return categories;
    }
    ```
      
    
























