## 들어가며
JWT에 대한 간단한 설명과, Spring Boot에서는 JWT를 어떻게 사용하는지 소개한다.  

## 사전준비
### 1. 서버 기반 인증 vs 토큰 기반 인증
특정 사용자가 서버에 접근을 했을 때, 이 사용자가 인증된 사용자인지 구분하기 위해서는  
여러 방법을 사용할 수 있다.  
대표적인 방법으로는
- 서버 기반 인증
- 토큰 기반 인증
위 2가지 방벙으로 나눌 수 있다.  
위 방법들은 각각의 장, 단점이 존재하기 때문에 상황에 맞게 적절한 방법을 선택해야 한다.  
그 중 JWT는 '토큰 기반 인증'에 해당하는 방법이다.  
  
토큰을 사용한다는 것은 요청과 응답에 토큰을 함께 보내 이 사용자가 유효한 사용자인지를 검색하는  
방법이다.  
이 때, 보통 Json Web Token(JWT)를 사용해서 토큰을 전달한다.  
  
조금 더 들어가서 어떻게 동작하는지 살펴보자  
![image](https://user-images.githubusercontent.com/33191974/134165607-2aa0bfa3-b543-4a65-a24d-7ece80bceb60.png)
- 클라이언트가 아이디와 비밀번호를 서버에게 전달하며 인증을 요청한다.
- 서버는 아이디와 비밀번호를 통해 유효한 사용자인지 검증하고, 유효한 사용자인 경우 토큰을  
  생성해서 응답한다.  
- 클라이언트는 토큰을 저장해두었다가, 인증이 필요한 api에 요청할 때 토큰 정보와 함께 요청한다. 
- 서버는 토큰이 유효한지 검증하고, 유효한 경우에는 응답을 해준다. 

### 2. 토큰 사용 방식의 특징
#### 2-1 무상태성
사용자의 인증 정보가 담겨있는 토큰을 클라이언트에 저장하기 때문에 서버에서 별도의 저장소가 필요없어  
완전한 무상태(stateless)를 가질 수 있다.  
그리고 이로인해 서버를 확장할 때 용이하다.

#### 2-2 확장성
HMAC(Hash-based Message Authentication) 기법이라고도 불리며, 발급 후의 토큰의 정보를 변경하는  
행위가 불가능하다.  
즉, 토큰이 변조되면 바로 알아차릴 수 있다.

#### 2-3 보안성
클라이언트가 서버에 요청을 보낼 때, 쿠키를 전달하지 않기 때문에 쿠키의 취약점은 사라진다.  

### 3. JWT?
이제 토큰 기반 인증에 대해 대충 알아보았으니, JWT가 무엇이고, 어떻게 구성이 되어 있는지   
알아보도록 하자.  
JWT는 아까 서술했듯이, 토큰 기반 인증 시스템의 대표적인 구현체이다.  
Java를 포함한 많은 프로그래밍 언어에서 이를 지원하며, 보통 회원 인증을 할 때에  
사용된다.
![image](https://user-images.githubusercontent.com/33191974/134173492-7b6a6707-4e0e-40e6-92bd-b6e0835f5c34.png)
JWT는 '.'을 기준으로 헤더(header)-내용(payload)-서명(signature)으로 이루어져 있다.  
각각 무슨 역할을 하는지 간단하게 알아보도록 하자.  
  
#### 3-1 헤더(header)
헤더는 토큰의 타입과 해싱 알고리즘을 지정하는 정보를 포함한다.  
- typ : 토큰의 타입을 지정한다. JWT라는 문자열이 들어가게 된다. 
- alg : 해싱 알고리즘을 지정한다.
```
{
  "typ" : "JWT",
  "alt" : "HS256"
}
```
위 예제를 해석하면, JWT 토큰으로 이루어져있고 해당 토큰은 HS256으로 해싱 알고리즘으로  
사용되었다는 것을 알 수 있다.  

#### 3-2 정보(payload)
토큰에 담을 정보가 들어간다.  
정보의 한 덩어리를 클레임(claim)이라고 부르며, 클레임은  
key-value의 한 쌍으로 이루어져 있다.  
클레임의 종류는 세 종류로 나눌 수 있다.  

- 이미 등록된(registered) 클레임
  - 토큰에 대한 정보를 담기 위한 클레임들이며, 이미 이름이 등록되어있는 클레임
  - iss : 토큰 발급자(issur)
  - sub : 토큰 제목(subject)
  - aud : 토큰 대상자(audience)
  - exp : 토큰의 만료시간(expiraton). 시간은 NumericDate 형식으로 되어있어야 하며,
          (예: 1480849147370) 항상 현재 시간보다 이후로 설정되어 있어야 한다. 
  - nbf : Not Before를 의미하며, 토큰의 활성 날짜와 비슷한 개념, NumericDate 형식으로  
          날짜를 지정하며, 이 날짜가 지나기 전까지는 토큰이 처리되지 않는다.
  - iat : 토큰이 발급된 시간(issued at)
  - jti : JWT의 고유 식별자로서, 주로 일회용 토큰에 사용한다.

- 공개(public) 클레임
  - 말 그대로 공개된 클레임. 충돌을 방지할 수 있는 이름을 가져야하며, 보통 클레임 이름을  
    URI로 짓는다.

- 비공개(private) 클레임
  - 클라이언트 - 서버 간에 통신을 위해 사용되는 클레임.

#### 예제 payload
```
{
  "iss" : "ajufresh@gmail.com", //등록된(registered) 클레임
  "iat" : 1622370878, //등록된(registered) 클레임
  "exp" : 1622372678, //등록된(registered) 클레임
  "https://shinsunyoung.com/jwt_claims/is_admin" : true, // 공개(public) 클레임
  "email" : "ajufresh@gmail.com", //비공개(private) 클레임
  "hello" : "안녕하세요!" //비공개(private) 클레임
}
```
  
#### 3-3 서명(signature)
해당 토큰이 조작되었거나 변경되지 않았음을 확인하는 용도로 사용하며,  
헤더(header)의 인코딩 값과 정보(payload)의 인코딩 값을 합친 후에 주어진  
비밀키를 통해 해쉬값을 생성한다.  

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```
이제 JWT의 구조까지 알아보았으니, (드디어) 본격적으로 Spring 환경에서 JWT를 다루기  
위해 사용하는 jsonwebtoken 사용법에 대해 알아보도록 하자.


## 구현
### 1. 의존성 추가
jsonwebtoken을 사용하기 위해 의존성을 추가해준다.
```
implementation 'io.jsonwebtoken:jjwt:0.9.1'
```

### 2. JWT 토큰 만들기
```
public String makeJwtToken() {
  Date now = new Date();
  
  return Jwts.builder()
    //헤더의 타입을 지정할 수 있다.
    //jwt를 사용하기 때문에 Header.JWT)_TYPE로 사용해준다.
    .setHeaderParam(Header.TYPE, Header.JWT_TYPE)
    //등록된 클레임 중, 토큰 발급자(iss)를 설정할 수 있다.
    .setIssuer("fresh")
    //등록된 클레임 중, 발급 시간(iat)를 설정할 수 있다.
    //Date 타입만 추가가 가능하다.
    .setIssuedAt(now)
    //등록된 클레임 중, 만료시간(exp)를 설정할 수 있다.  
    //마찬가지로 Date 타입만 추가가 가능하다.
    .setExpiration(new Date(now.getTime() + Duration.ofMinutes(30).toMillis()))
    //비공개 클레임을 설정할 수 있다.(key-value)
    .claim("id", "아이디")
    .claim("email", "ajufresh@gmail.com")
    //해싱 알고리즘과 시크릿 키를 설정할 수 있다.
    .signWith(SignatureAlgorithm.HS256, "secret")
    .compact();
}
```
더 많은 기능이 있다.  
모든 설정이 끝나면 compact()를 통해 JWT 토큰을 만들 수 있다.  
  
그 이후에 실행하면,
![image](https://user-images.githubusercontent.com/33191974/134178821-09114448-7332-435c-8509-db63f9a79ed3.png)
위와 같은 토큰을 획득할 수 있다.

### 3. JWT 토큰 파싱하기
클라이언트에서 토큰을 저장해두었다가 Authorization 헤더에 Bearer라는 문자열을 붙여  
토큰을 보내게 된다. 
![image](https://user-images.githubusercontent.com/33191974/134179059-667011cd-514e-444a-8a08-6cca2b99c6c3.png)

전달받은 토큰을 해석해서 유효한 토큰인지 확인이 가능하다.

```
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServiceResponse response,
                                       FilterChain filterChain) throws IOException, ServiceException {
  String authorizationHeader = request.getHeader(HttpHeaders.AUTHORIZATION);
  Claims claims = jwtTokenProvider.parseJwtToken(authorizationHeader);
  filterChain.doFilter(request, response);
}
```
```
public Claims parseJwtToken(String authorizationHeader) {
  //헤더가 'Bearer'로 시작하는지 검사한다.
  validationAuthorizationHeader(authorizationHeader);
  //'Bearer"을 제외한 문자열만 반환해주도록 처리해준다.
  String token = extractToken(authorizationHeader);
  
  return Jwts.parser()
      //시크릿 키를 넣어주어 토큰을 해석할 수 있다.
      .setSigningKey("secret")
      //해석할 토큰을 문자열(String) 형태로 넣어준다.
      .parseClaimsJws(token)
      .getBody();
}

private void validationAuthorizationHeader(String header) {
  
  if (header == null || !header.startsWith("Bearer ")) {
    throw new IllegalArgumentException();
  }
}

private String extractToken(String authorizationHeader) {
  return authorizationHeader.substring("Bearer ".length());
}
```
위와 같은 정보를 넣어준 후에 getBody()를 호출하게 되면, claims 타입의  
결과 객체를 반환하게 되는데, 여기에 저장된 크레임 정보들을 확인할 수 있다.  
![image](https://user-images.githubusercontent.com/33191974/134180805-98e06bf6-0ed8-4253-b27e-245f514e5c0b.png)

발생할 수 있는 예외는 다음과 같다.
- UnsupportedJwtException : 예상하는 형식과 다른 형식이거나 구성의 JWT일 때
- MalformedJwtException : JWT가 올바르게 구성되지 않았을 때
- ExpiredJwtException : JWT를 생성할 때 지정한 유효기간이 초과되었을 때
- SignatureException : JWT의 기존 서명을 확인하지 못했을 때
- IllegalArgumentException 

위의 예외에 대해 적절한 처리를 해주는 것이 좋다.

































