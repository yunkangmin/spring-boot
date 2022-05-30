## open feign 설정 시 주의점(@Configuration)
spring-cloud에 REST client open feign client가 있다.  
API endpoint마다 사용하기 위해 여러 feign client를 만드는 경우 서로 다른 설정을  
해줘야 해서 configuration을 만드는데, 이 때 configuration 클래스에 @Configuration을  
달아주면 안된다.

```
@FeignClient(name = "foo-api", url = "${ext.foo-api.url}", configuration = FooApiRequesterConfiguration.class)
public interface FooApi {
  @PostMapping("/v2/accounts/{accountId}/name")
  String name(
    @PathVariable("accountId") Long accountId,
    @RequestBody MyRequestDto myRequestDto);
}

@FeignClient(name = "bar-api", url = "${ext.bar-api.url}", configuration = BarApiRequestConfiguration.class)
public interface BarApi {
  @GetMapping("/v2/bars/{barId}/name")
  String name(@PathVariable("barId") Long barId);
}
```
위와 같이 만들어서 FooApi와 BarApi가 있을 때 서로 다른 설정을 하기 위해 아래처럼  
configuration 2개를 따로 만들어서 @FeignClient에 넣어줬다.  
  
이 샘플은 Bearer 인증을 사용하는데, FooApi와 BarApi가 서로 다른 토큰 값을 가질 때 각자  
설정해서 사용하는 샘플이다.  

```
public class FooApiRequestConfiguration {
  @Bean
  public RequestInterceptor fooApiRequestHeader(@Value("{ext.foo-api.bearerToken}") String token)
    return new BearerAuthRequestInterceptor(token);
  }
}

public class BarApiRequesterConfiguration {
  @Bean
  public RequestInterceptor barApiRequestHeader(@Value("${ext.bar-api.bearerToken}") String token)
    return new BearerAuthRequestInterceptor(token);
  }
}

public class BearerAuthRequesterInterceptor implements RequestInterceptor{
  private String token;
  
  public BearerAuthRequestInterceptor(String token) {
    Preconditions.checkNotNull(token, "Token should not be null");
    this.token = token;
  }
  
  @Override
  public void apply(RequestTemplates template) {
    template.header("Authorization, "Bearer " + token);
  }
}
```
  
### 정리  
이 때 configuration 이니까 (Spring 설정이 아닌데도) 습관처럼 @Configuratiom을 달아주면  
의도와는 다르게 동작한다. 여기서의 의도는 FooApi에는 FooApiRequestConfiguration만 설정되고  
BarApi에는 BarApiRequesteConfiguration만 설정되기를 원하는 것이다.  
하지만 @Configuration을 달아주게 되면 Spring Bean으로 등록되서 모든 feign client의 설정으로   
동작하게 되고, FooAPi를 사용할 때 fooApiRequestHeader()와 barApiRequestHeader() 모두  
실행하게 된다. BarApi도 마찬가지이다.




















