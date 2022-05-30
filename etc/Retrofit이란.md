Backend 또는 FrontEnd를 개발하다보면 거의 필수적으로, 다른 서버에 데이터를 요청할 일이 생긴다.  
특히나 FrontEnd의 경우에는 서버로부터 데이터를 받아와 출력하는 경우가 잦다.  
  
이번 시간에는 Java의 HttpClient(HttpClient객체는 GET(HttpGet)이나 POST(HttpPost) 같은 HTTP 명령어  
객체와 함께 HTTP Request 메시지를 서버로 전송하는 역할을 한다.) 라이브러리 중 하나인 Retrofit에  
대해 알아보자.

### 1. Retrofit이란?
- Retrofit은 TypeSafe한 HttpClient 라이브러리이다. TypeSafe하다는 게 어떤 의미일까?  
  바로 네트워크로부터 전달된 데이터를 우리 프로그램에서 필요한 객체로 받을 수 있다는 의미이다.  
-사실 Retrofit은 기본적으로 OKHttp(OkHttp는 REST API, HTTP 통신을 간편하게 구현할 수 있도록  
  다양한 기능을 제공해주는 Java 라이브러리다. "Square"라는 회사가 만든 OkHttp 라이브러리는   
  Retrofit 이라는 라이브러리의 베이스가 된다. )에 의존하고 있다.   

### 2. HttpClient Library를 왜 사용할까?
Http 통신을 가장 간단히 사용한다면,  
HttpUrlConnection을 많이 사용했을 것이다. Java.net에 내장되어 있기 때문에  
별도의 라이브러리없이 사용할 수 있다. 그렇다면, 이러한 클래스를 이용하면 되는데  
굳이 Retrofit, OKHttp, Volley등과 같은 라이브러리를 사용할까?
  
#### Http 개발의 어려움
라이브러리들만을 이용해 개발해본 사람은 이렇게 생각할 수도 있다.  
그냥 HttpRequest하고 Response의 Body를 Parsing해서 사용하면 되는 것 아닌가?
  
이렇게 생각한다면 사용한 라이브러리가 정말 잘 만들어졌다는 것을 반증하는 것일 수도  
있다. 그게 아니면, 정말 간단히 사용하는 경우에는 그럴수도 있겠지만, 보통 Http를 개발한다면   
아래의 것들을 고려해야 한다.  
- 연결
- 캐싱
- 실패한 요청의 재시도
- 스레딩
- 응답 분석
- 오류처리

정말 많다. 단지, Http 요청을(성능좋고, 오류가 적은)위해서, 저 많은 것들을 개발하다보면  
배보다 배꼽이 커지는 것은 당연하다.  

#### HttpURLConneciton
HttpURLConnection은 가장 원시적인 방법의 HttpClient이다.
> 장점

- java.net에 포함된 클래스로 별도의 라이브러리 추가가 필요없다.
- 자신이 원하는 방식으로 커스텀하여 사용할 수 있다.(단점이기도 함)

> 단점

- 자유도가 높은 대신, 직접 구현해야하는 것들이 많다.

### 3. Retrofit 사용법
#### 3.1 dependency(gradle)추가
```
//retrofit
compile group: 'com.squareup.retrofit2', name: 'retrofit', version: '2.5.0'
//converter-gson
compile group: 'com.squareup.retrofit2', name: 'converter-gson', version: '2.5.0'
```
Retrofit을 사용하기 위해서 기본적으로 retrofit의 의존성이 필요하다.  
하지만 결과를 원하는 객체로 변환하여 받기 위해서는 gson converter라이브러리를 같이 등록해  
주어야 한다. 

#### 3.2 interface에 HTTP API 기술
```
import retrofit2.Call;
import retrofit2.http.GET;

public interface TestService {
  @GET("/api/users/2")
  Call<Object> getTest();
}
```
retrofit은 Interface에 기술된 명세를 Http API로 전환해준다.  
따라서 우선,우리가 요청할 API들에 대한 명세만을 Interface에 기술해두면 된다.  
  
> API 어노테이션

기본적으로 GET, POST, DELETE, PUT을 지원한다.  
위 예제의 경우 HTTP GET 요청을 의미한다.  
\* URL의 스킴, 프로토콜, 호스트위치, port를 포함하는 BaseURL의 경우 Retrofit Client 생성 시  
입력하기 때문에 ()안에는 요청할 서버의 자원의 위치를 나타내는 url만을 입력해주면 된다.  
> 반환 타입
반환되는 타입은 Call<객체타입>의 형태로 기술해야 한다.  

### 3.3 HTTP API 인터페이스의 구현체 새성
```
public class RetrofitClient {
  private static final String BASE_URL = "https://reqres.in/";
  
  public static TestService getApiService() {
    return getInstance().create(TestService.class);
  }
  
  private static Retrofit getInstance() {
    Gson gson = new GsonBuilder()
            .setLenient()
            .create();
    
    return new Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create(gson))
            .build();
  }
}
```
앞서 말했듯이 Retrofit은 Interface에 기술된 HTTP API의 구현체를 생성해준다.  
메소드와 변수를 하나씩 알아보자. 

> BASE_URL

앞서 말했듯이 우리가 요청할 서버의 기본 URL이다.  
딱히 요청할 서버가 없다면 https://reqres.in/과 같은 Open RestAPI Test서버를  
이용하는 것도 좋은 방법이다.  

> getInstance()

- 우선 setLenient() 설정이 된 Gson 객체를 만든다.  
  이는 json 응답을 객체로 변환하기 위해 필요하다. 
- Retrofit.Builder()를 이용해 BaseUrl 설정, 응답을 객체로 변환하기 위한 GsonConverter 설정을  
  하여 Retrofit Client를 생성한다.  
  
getInstance() 메서드는 위의 2가지 행위의 결과로 반환되는 Retrofit 클라이언트 객체를 반환한다. 

> getApiService()

getApiService() 메소드는 앞서 구현한 getInstance() 메소드를 이용해, Retrofit 클라이언트를  
생성한 뒤, Retrofit 클라이언트를 이용하여, Http API 명세가 담긴 Interface의 구현체를 생성한 뒤  
반환한다.  

### 3.4 [동기](https://jieun0113.tistory.com/73) 호출(결과값 받기)
```
public class MainTest {
  public static void main(String[] args) {
    Call<Object> getTest = RetrofitClient.getApiService().getTest();
    try {
      System.out.println(getTest.execute().body());
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
앞서 생성한 HTTP API 명세가 담긴 Interface에 getTest() 메소드의 결과값을  
Call<Object>로 지정했기 때문에 결과값 또한 Call<Object>에 담아준다.  
이후 getTest의 execute() 메소드를 호출하면 요청이 전송되고 body()메서드를  
호출하여 아래와 같이 결과값을 콘솔에 출력한다.
![image](https://user-images.githubusercontent.com/33191974/139644463-dbca310e-c4a9-40c1-bd24-21236b61d98d.png)

  


  



























































