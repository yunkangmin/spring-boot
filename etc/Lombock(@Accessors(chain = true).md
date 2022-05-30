# @Accessors(chain = ture)
@Setter를 이용해서 객체를 생성할 때는 아래와 같이 여러 줄로 setMethod를 생성해야  
하는 단점이 있다.  
```java
@Test
public void create() {
  String account = "Test01";
  String password = "Test01";
  String status = "REGISTERED";
  String email = "TEST01@gmail.com";
  String phoneNumber = "010-1111-2222";
  LocalDateTime registeredAt = LocalDateTime.now();
  LocalDateTime unregisteredAt = LocalDateTime.now();
  
  User user = new User(); //@NoArgsConstructor
  
  user.setAccount(account); //@Setter
  user.setPassword(password);
  user.setStatus(status);
  user.setEmail(email);
  user.setPhoneNumber(phoneNumber);
  user.setRegisteredAt(registeredAt);
  user.setUnregisteredAt(unregisteredAt);
}
```
이러한 단점을 보완하기 위해 `@Accessors(chain = true)`를 사용한다.
먼저 User class에 `Accessors(chain = true)를 추가한다.
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
@Accessors(chain = true)
public class User {
  private String accout;
  private String password;
  private String status;
  private String email;
  private String phoneNumber;
  private LocalDateTime registeredAt;
  private LocalDateTime unregisteredAt;
}
```
아래와 같이 Test 코드에서 Setter를 이용할 수 있다. 
```java
@Test
public void create() {
  String accout = "Test01";
  String password = "Test01";
  String status = "REGISTERED";
  String email = "TEST01@gmail.com";
  String phoneNumber = "010-1111-2222";
  LocalDateTime registeredAt = LocalDateTime.now();
  LocalDateTime unregisteredAt = LocalDateTime.now();
  
  User user = new User() //chaining
                  .setAccount(account)
                  .setPassword(password);
}
```
`@Accessros(chain = true)를 사용하면 일일이 setMethod를 여러 줄로 생성할  
필요없이 Chain 형태로 이어서 원하는 setMethod를 생성할 수 있다.





















