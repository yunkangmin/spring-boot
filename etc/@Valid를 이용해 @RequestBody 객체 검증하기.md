Springboot를 이용해서 어노테이션을 이용한 validation을 하는 방법을 소개한다. 
RestController를 이용해서 @RequestBody 객체를 사용자로부터 가져올 때, 들어오는 값을  
검증할 수 있는 방법을 소개한다.   

@Valid를 사용하면, Service 단이 아닌 객체 안에서, 들어오는 값에 대해 검증을 할 수 있다.  

### 1. @Valid를 사용하는 방법
```
@RestController
@Slf4j
public class TestController {
  
  @PostMapping("/user")
  public ResponseEntity<String> savePost(final @Valid @RequestBody UserDto userDto) {
    log.info(userDto.toString());
    return ResponseEntity.ok().body("postDto 객체 검증 성공");
  }
}
```
파라미터로 @RequestBody 어노테이션 옆에 @Valid를 작성하면, RequestBody로 들어오는 객체에  
대한 검증을 수행한다. 이 검증의 세부적인 사항은 객체 안에 정의를 해두어야 한다. 
```
@ToString
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class UserDto {
  
  //@NotNull
  //인자로 들어온 필드 값에 null 값을 허용하지 않는다.
  @NotNull
  private String name;
  
  //@Email
  //인자로 들어온 값이 Email 형식을 갖추어야 한다. 
  @Email
  private String email;
}
```
[참고](https://jyami.tistory.com/55)
