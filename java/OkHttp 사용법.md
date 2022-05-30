OkHttp는 REST API, HTTP 통신을 간편하게 구현할 수 있도록 다양한 기능을 제공해주는 Java  
라이브러이이다. "Square"라는 회사가 만든 OkHttp 라이브러리는 Retrofit이라는 라이브러리의  
베이스가 된다. OkHttp를 이용하면 간편하게 몇 줄의 코딩으로 REST 호출을 전송, HTTP 기반의  
요청, 응답을 처리할 수 있다.  
  
OkHttp는 오픈소스로 공개된 소프트웨어이며, [깃허브](https://github.com/square/okhttp)에 가면 소스코드를 볼 수 있다.   
  
### OkHttp 사용하기 - 메이븐(Maven) 설정
OkHttp 라이브러리를 사용하기 위해서 메이븐 Dependencies 설정이 필요하다.  
```
<dependency>
  <groupId>com.squareup.okhttp3</groupId>
  <artifactId>okHttp</artifactId>
  <version>3.10.0</version>
</dependency>
```
메이븐 외에 그래들(Gradle)같은 빌드 도구를 이용하고 싶으면 [여기](https://mvnrepository.com/artifact/com.squareup.okhttp/okhttp)  
를 참고하자.   
 
 ### REST API 예제
 다음과 같은 REST API가 있다고 하자. 
 
 ```
 http://127.0.0.1:8080/v1/test/get
 http://127.0.0.1:8080/v1/test/head
 http://127.0.0.1:8080/v1/test/delete
 http://127.0.0.1:8080/v1/test/post
 http://127.0.0.1:8080/v1/test/put
 ```
 
 http:127.0.0.1:8080/v1/test URL 밑에 함수를 의미하는 문자열이 있다.  
 각각 get, head, delete, post, put 요청을 처리하는 REST API라고 하자.  
 이제 이 REST API에 요청을 날리는 Java 코드를 작성해보겠다.  
 
 ### GET, HEAD, DELETE 예제
 우선 리소스를 조회하는 GET 요청을 구현해보자. http://127.0.0.1:8080/v1/test/get이라는  
 API가 공개되어 있고 이 URL에 정보를 조회하고자 하는 Key를 쿼리 파라미터로 전달하는 예제이다.  
   
 우리가 잘 알고 있는 curl 명령을 이용하면 다음과 같다.  
   
 [curl 옵션](https://www.lesstif.com/software-architect/curl-http-get-post-rest-api-14745703.html)  
   
 ```
 $curl -v -H "password: BlahBlah" -X GET http://127.0.0.1:8080/v1/test/get?key=123
 ```
 REST API URL의 끝에 key=123이라는 파라미터를 같이 넘겼다.  
 또한 사용자를 식별하기 위한 패스워드(Password)라는 헤더로 전달받았다. 
   
 이 동작을 OkHttp를 이용한 Java 소스로 구현하면 다음과 같다. 
 ```java
 public boolean getUserInfo(String key) {
  try {
    String url = "http://127.0.0.1:8080/v1/test/get?key=" + key;
    OkHttpClient client = new OkHttpClient();
    Request.Builder builder = new Request.Builder().url(url).get();
    builder.addHeader("password", "BlahBlah");
    
    Request request = builder.build();
    Response response = client.newCall(request).execute();
    if (response.isSuccessful()) {
      ResponseBody body = response.body();
      if (body != null) {
        System.out.println("Response:" + body.string());
      }
    } else {
      System.out.println("Error Occurred");
      return true;
    }
  } catch (Exception e) {
    e.printStackTrace();
  }
  return false;
}
```
우선 OkHttp 요청을 전송할 OkHttpClient 객체가 필요하다.  
그 다음 GET 요청을 Request.Builder를 통해 만든다. 이 때, REST API 요청을 전송할 URL  
과 get() 메소드를 사용하여 GET 요청임을 명시한다. addHeader() 메소드를 이용하여 헤더 정보에  
Password()를 명시한다.  
  
그리고 client 객체의 newCall() 메소드에 만들어진 Request를 전달하고, execute() 메소드를 실행한다.  
execute() 메소드는 요청에 대한 응답이 올 때가지 기다렸다가 반환한다.  
  
요청에 대한 응답이 오면 Response 객체를 반환받게 된다.  
Response 객체에는 요청에 대한 바디(Body) 정보가 담겨있으며, 요청이 실패했을 경우  
에러 코드등이 담겨있다.  
  
HEAD 요청과, DELETE 요청은 기본적으로 GET 요청과 동일하게 처리된다.  
다만 Request.Builder에서 get() 메소드 대신 head() 혹은 delete() 메소드로 바꾸기만 하면 된다.  
  
### POST, PUT 예제
POST 요청과 PUT 요청은 리소스를 생성하거나 수정한다. 우선 POST 요청부터 보자. 
http://127.0.0.1:8080/v1/test/post라는 REST API가 있고 이 URL로 POST 요청을 해보겠다.  
curl 명령을 이용하면 다음과 같다.  
```
//-v는 명령어가 동작하면서 자세한 옵션을 출력한다.  
//-X는 GET, POST등의 요청 메소드 지정
//-d는 생성될 데이터의 내용을 명시한다.
$curl -v -H "password: BlahBlah" -X POST http://127.0.0.1:8080/v1/test/post -d "{\"key\":123,  
\"value\":55323}"
```

이 동작을 Java로 구현하면 다음과 같다.  
```java
public boolean postUserInfo() {
    
  try {
      String url = "http://127.0.0.1:8080/v1/test/post";
      String postBody = "" + "{" + "\"key\":123," + "\"value\":55323" + "}";
    
      OkHttpClient client = new OkHttpClient();
      RequestBody requestBody = RequestBody.create(
            MediaType.parse("application/json; charset=utf-8"), postBody);
    
      Request.Builder builder = new Request.Builder().url(url)
        .addHeader("Password", "BlahBlah")
        .post(requestBody);
      Request request = builder.build();
    
      Response response = client.newCall(request).execute();
      if (response.isSuccessful()) {
        ResponseBody body = response.body();
        if (body != null) {
            System.out.println("Response:" + body.string());
        }
      } else {
          System.out.println("Error Occurred");
      }
    
      return true;
  } catch(Exception e) {
      e.printStackTrace();
  }
  
  return false;
}
```
기본적으로 GET 요청을 구현했던 코드와 비슷하다.  
다만 RequestBody 객체에 전달한 Body의 내용과 컨텐츠 타입(Json)이 추가되었다.  
Body가 없이 빈 POST 요청을 보내고 싶으면, 
```java
RequestBody body = RequestBody.create(null, new byte[0]);
```
이런 식으로 RequestBody를 만들어도 된다.  
  
PUT 요청도 POST 요청처럼 RequestBody body = RequestBody.create() 요청으로 body를  
만들어서 request builder의 put() 메소드의 인자로 주면된다.   
  
### 비동기 요청
여기까지는 동기 요청(Synchronous Request)였다.  
즉, client.newCall(request).execute()를 실행하면, 요청에 대한 응답이 얻어질 때까지 대기했다.  
OkHttp는 비동기적인 요청(Asynchronous Request)도 지원한다.  
OkHttpClient의 execute() 메소드 대신 enqueue() 메서드를 사용하면 된다.  
enqueue() 메서드의 인자로 요청이 완료되면 실행될 콜백(Callback)을 넘겨주면 비동기적인 요청을  
전송하게 된다.  
  
Callback은 인터페이스로 다음과 같은 메소드를 구현해야 한다. 

```java
public interface Callback {
    void onFailure(Call call, IOException e);
  
    void onResponse(Call call, Response response) throws IOException;
}
```

비동기적으로 요청한 Request가 성공적으로 응답을 받으면 onResponse() 메소드가 호출되고  
예외가 발생하는 등 실패하면 onFailure() 메소드가 호출된다. 
  
위에서 구현한 POST 요청을 비동기적인 방식으로 구현마녀 다음과 같다.  
```java
public void postAsyncUserInfo() {
    
  try {
      String url = "http://127.0.0.1:8080/v1/test/post";
      String postBody = "" + "{" + "\"key\":123," + "\"value\":55323" + "}";
      
      OkHttpClient client = new OkHttpClient();
      RequestBody requestBody = RequestBody.create(
              MediaType.parse("application/json; charset=utf-8"), postBody);
      
      Request.Builder builder = new Request.Builder().url(url)
                                           .addHeader("Password", "BlahBlah")
                                           .post(requestBody);
      Request request = builder.build();
    
      client.newCall(request).enqueue(new Callback() {
          
        @Override
        public void onFailure(Call call, IOException e) {
          System.out.println("Error Occurred");
        }
        
        @Override
        public void onResponse(Call call, Response response) throws IOException {
            ResponseBody body = response.body();
            if (body != null) {
               System.out.println("Response:" + body.string());
            }
        }
      });
  } catch (Exception e) {
    e.printStackTrace();
  }
}
```
enqueue() 메소드는 요청에 대한 응답을 기다리지 않고 바로 리턴한다.  
이 후, 비동기적으로 요청이 전송되며, 요청이 실패하거나 응답을 받으면 등록해놓은  
Callback() 객체가 실행되어 상황에 따라 onFailure(), onResponse() 메소드가 실행된다.  
  
OkHttp 라이브러리를 이용하면 몇 줄의 코드로 쉽게 REST API를 사용할 수 있다. 


 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
