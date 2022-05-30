스프링 부트 프레임워크는 인증과 권한에 관련된 강력한 기능인 스프링 부트 시큐리티를 제공한다.  
스프링 부트 시큐리티는 스프링 시큐리티의 번거로운 설정을 간소화시켜주는 래핑 프레임워크이다.  
스프링 시큐리티는 십여 년간 보안 노하우를 쌓아 와서 기본적인 틀 안에서 원하는 대로 인증, 권한 처리를 편리하게  
관리할 수 있다.  
따라서 보안 문제는 스프링 부트 시큐리티에 맡겨두고 우리는 핵심 로직만 개발하면 된다.  
  
일반적인 인증은 사용자명과 비밀번호로 이뤄진다.  
반면 회원 가입 과정을 생략하고 빠른 인증을 제공하는 (페이스북, 구글, 카카오 등이 사용하는) 인증 방식인  
OAuth2도 많이 사용한다.  
이번 글에서는 OAuth2 사용자 인증 후 각 사용자에게 허용되는 권한을 부여하는 방법을 알아본다.  
  
이번 글에서는 특별히 스프링 부트 1.5버전부터 알아본다.  
1.5버전에서 지원하는 스프링 시큐리티와 OAuth API를 사용해 소셜 미디어 인증을 빠르고 쉽게 적용한다.  
2.0 버전부터는 스프링 시큐리티 내부에 OAuth2 API가 포함되는 등 구조가 많이 바뀌었다.  
1.5 버전을 통해 전체적인 적용 방식을 익히고 2.0 버전으로 어떻게 업그레이드하는지 알아보자.  

#### 내용
- 배경지식 소개
- 스프링 부트 시큐리티 + OAuth2 설계하기
- 스프링 부트 시큐리티 + OAuth2 의존성 설정하기
- 스프링 부트 시큐리티 + OAuth2 구현하기
- 스프링 부트 2.0 기반의 OAuth2 설정하기

## 배경지식 소개
스프링 부트 시큐리티는 스프링 시큐리티에 스타터를 제공해 더 빠른 설정을 지원하는 프로젝트이다.  
빠르게 설정하고 적용하는 것도 중요하지만 기본적으로 시큐리티와 OAuth2가 무엇이며 어떻게 인증이 수행되는지  
확실하게 이해하고 넘어가도록 한다.  

### 스프링 부트 시큐리티
스프링 부트 시큐리티에서 가장 중요한 개념은 '인증(authentication)'과 '권한부여(authorization)'이다.  
인증은 사용자(클라이언트)가 애플리케이션의 특정 동작에 관하여 허락(인증)된 사용자인지 확인하는 절차를 말한다.  
보통 웹사이트 로그인을 인증이라 생각하면 된다.  
권한 부여는 데이터나 프로그램 등의 특정 자원이나 서비스에 접근할 수 있는 권한을 허용하는 것이다.  
예를 들어 A는 VIP 회원이고, B는 일반 회원이라면 두 회원의 권한이 다르게 부여된다.  
  
인증 방식은 다양하다.  
전통적인 인증 방식으로 사용자명(principle)과 비밀번호(credential)로 인증하는 "크리덴셜 기반 인증 방식'이 있다.   
OTP와 같이 추가적인 인증 방식을 도입해 한번에 2가지 방법으로 인증하는 이중 인증 방식도 있다.  
소셜 미디어를 사용해 편리하게 인증하는 OAuth2 인증 방식도 최근에는 필수적으로 쓰이고 있다.  

### OAuth2
OAuth는 토큰을 사용한 범용적인 방법의 인증을 제공하는 표준 인증 프로토콜이다.  
OAuth2는 OAuth 프로토콜의 버전 2이다. 이 프로토콜은 서드파티를 위한 범용적인 인증 표준이다.  
OAuth2에서 제공하는 승인 타입은 총 4가지이다.  

- 권한 부여 코드 승인 타입(Authorization Code Grant Type) : 클라이언트가 다른 사용자 대신 특정  
  리소스에 접근을 요청할 때 사용된다. 리소스 접근을 위한 사용자명과 비밀번호, 권한 서버에 요청해서 
  받은 권한 코드를 함께 활용하여 리소스에 대한 액세스 토큰을 받으면 이를 인증에 이용하는 방식이다. 
- 암시적 승인 타입(Implicit Grant Type) : 권한 부여 코드 승인 타입과 다르게 권한 코드 교환 단계 없이
  액세스 토큰을 즉시 반환받아 이를 인증에 이용하는 방식이다. 
- 리소스 소유자 암호 자격 증명 승인 타입(Resource Owner Password Credentials Grant Type) :
  클라이언트가 암호를 사용하여 액세스 토큰에 대한 사용자의 자격 증명을 교환하는 방식이다. 
- 클라이언트 자격 증명 승인 타입(Client Credentials Grant Type) : 클라이언트가 컨텍스트 외부에서 
  액세스 토큰을 얻어 특정 리소스에 접근을 요청할 때 사용하는 방식이다. 
 
눈여결볼 방식은 '권한 부여 코드 승인 타입'이다.  
왜냐하면 이 번 글에서 적용하고자 하는 페이스북, 구글, 카카오 등의 소셜 미디어들이 웹 서버 형태의 클라이언트를 지원하는데  
이 방식을 사용하기 때문이다.  
이 방식은 웹 서버에서 장기 액세스 토큰(long-lived access token)을 사용하여 사용자 인증을 처리한다.  
다음 그림을 보자. 

권한 부여 코드 승인 타입 시퀀스 다이어그램  
![image](https://user-images.githubusercontent.com/33191974/124387477-d573f780-dd19-11eb-9031-7a6d1983c8f5.png)


다음은 시퀀스 다이어그램에 표시된 각 주체에 대한 예이다.
- 리소스 주인(resource owner) : 예) 인증이 필요한 사용자
- 클라이언트(client) : 예) 웹사이트
- 권한 서버(authorization server) : 예) 페이스북/구글/카카오 서버
- 리소스 서버(resource server) : 예) 페이스북/구글/카카오 서버

상당이 복잡해보이지만 번호 흐름을 따라가며 사펴보면 어렵지 않게 이해할 수 있다.  
1. 클라이언트가 파라미터로 클라이언트 ID, 리다이렉트 URI, 응답 타입을 code로 지정하여 권한 서버에 전달한다.  
   정상적으로 인증이 되면 권한 부여 코드를 클라이언트에 보낸다.(응답 타입은 code, token이 사용가능하다.  
   응답 타입이 token일 때가 암시적 승인 타입에 해당한다.)
2. 성공적으로 권한 부여 코드를 받은 클라이언트는 권한 부여 코드를 사용하여 액세스 토큰(로그인 세션에 대한 보안자격을 증명하는 식별코드이다.  
   사용자, 사용자 그룹, 사용자 권한 및 경우에 따라 특정 API 사용을 보증하는 역할을 한다.)을 권한 서버에 추가로 요청한다.  
   이 때 필요한 파라미터는 클라이언트 ID(client-id), 클라이언트 비밀번호(client-secret), 리다이렉트 URI, 인증 타입이다.
3. 마지막으로 응답받은 액세스 토큰을 사용하여 리소스 서버에 사용자의 데이터를 요청한다. 

'사용자명 + 비밀번호' 인증 방식은 저장된 사용자명과 비밀번호가 같은지 한 번만 요청하면 되지만 OAuth2 방식은 최소 세번 요청한다.  
하지만 OAuth2는 회원가입 없이 이미 사용하는 소셜 미디어 계정으로 인증하기 때문에 사용자 입장에서는 더욱 편리하게 로그인을  
처리할 수 있다.  
서비스 측면에서는 회원 가입 관련 기능을 축소시키고 소셜에서 제공하는 User 정보를 가져올 수 있어 편리하다.  
  
여기서 '권한 부여 코드 승인 타입'의 흐름을 이해하는 것은 굉장히 중요하다.  
스프링이 아닌 다른 어떤 라이브러리도 이 흐름을 바탕으로 코드를 구현하기 때문에 위 그림을 정확히 파악하는 것만으로도  
소셜 인증 구현을 위한 준비 중 절반을 진행했다고 해도 과언이 아니다.  
정확하게 이해가 안되었다면 다시 한번 그림을 살펴보자.  

## 스프링 부트 시큐리티 + OAuth2 설계하기
완성할 어플리케이션 내의 OAuth2 인증과정을 아래 그림과 같은 흐름도로 표현했다.
  
스프링 시큐리티 + OAuth2 적용 흐름도
![image](https://user-images.githubusercontent.com/33191974/124388088-1d941980-dd1c-11eb-8fe4-4d90c449a3f5.png)

1. 사용자가 어플리케이션에 접속하면 해당 사용자에 대한 이전 로그인 정보(세션)의 유무를 체크한다.
2. 세션이 있으면 그대로 세션을 사용하고 없으면 OAuth2 인증과정을 거친다.  
3. 이메일을 키값으로 사용하여 이미 가입된 사용자인지 체크한다.  
   이미 가입된 사용자라면 등록된 정보를 반환하여 요청한 URL로의 접근을 허용하고 아니라면  
   새롭게 User 정보를 저장하는 과정을 진행한다.  
4. 각 소셜 미디어에서 제공하는 User 정보가 다르기 때문에 소셜 미디어에 따라 User 객체를 생성한 후  
   DB에 저장한다. 

\*세션이 있거나 4번까지 성공한 사용자는 요청한 URL로의 접근 허용한다.
 
다음 그림은 소셜 미디어 계정으로 커뮤니티 게시판에 로그인하는 흐름을 보여준다.  
앞으로 구현할 내용이다.  

커뮤니티 게시판 시큐리티/OAuth2 흐름  
![image](https://user-images.githubusercontent.com/33191974/124422994-40b3dd00-dd9f-11eb-97a5-c64712265a81.png)
  
```java
//소셜 미디어의 타입 정보를 나타내는 enum 객체  
// /com/web/domain/enums/SocialType.java
public enum SocialType {
   FACEBOOK("facebook"),
   GOOGLE("google"),
   KAKAO("kakao");
   
   private final String ROLE_PREFIX = "ROLE_";
   private String name;
   
   SocialType(String name) {
      this.name = name;
   }
   
   public String getRoleType() { return ROLE_PREFIX + name.toUpperCase(); }
   
   public String getValue() { return name; }
   
   public boolean isEquals(String authority) {
      return this.getRoleType().equals(authority);
   }
}
```
각 소셜 미디어의 정보를 나타내는 SocialType enum을 생성했다.  
getRoleType() 메서드는 'ROLE_\*' 형식으로 소셜 미디어의 권한명을 생성한다.  
enum을 사용해 권한 생성 로직을 공통 코드로 처리하여 중복 코드를 줄일 수 있다.  
  
로그인과 관련하여 인증 및 권한이 추가되므로 User.java의 User 테이블에 컬럼을 추가한다.  
OAuth2 인증으로 제공받는 키값인 principal과 어떤 소셜 미디어로 인증받았는지 여부를 구분해주는  
socialType 컬럼도 추가한다.  
  
```java
//principal, socialType 추가
// /com/web/domain/User.java

...
@Column
private String principal;

@Column
private SocialType socialType;

...

@Builder
public User(String name, String password, String email, String principal, SocialType socialType, LocalDateTime createdDate, 
               LocalDateTime updatedDate) {
               this.name = name;
               this.password = password;
               this.email = email;
               this.principal = principal;
               this.socialType = socialType;
               this.createdDate = createdDate;
               this.updatedDate = updatedDate;
 }
}
```

두 컬럼을 추가한 User 테이블은 다음과 같다. 

![image](https://user-images.githubusercontent.com/33191974/124431906-f5ec9200-ddab-11eb-843a-e1385d5de72e.png)

## 스프링 부트 시큐리티 + OAuth2 의존성 설정하기
새 프로젝트를 생성하지 않고 앞서 진행했던 게시판 프로젝트에 의존성을 추가해서 진행한다.  
build.gradle에 spring security OAuth2를 추가한다.  
OAuth2 의존성 안에 security까지 포함되어 있어서 따로 security 의존성을 부여할 필요는 없다. 

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'com.h2database:h2'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

## 스프링 부트 시큐리티 + OAuth2 구현하기
이제까지 스프링 부트를 사용하여 설정을 빠르게 적용한 것처럼
시큐리티, OAuth2도 적절한 프로퍼티 설정값만 지정하며 빠르고 편히라게 적용할 수 있다.  
  
프로젝트를 구현하기 전에 페이스북, 구글, 카카오의 개발자 센터에서 '클라이언트 ID'와  
'Secret(클라이언트 시크릿 키값, 클라이언트 보안 비밀)'을 발급 받아야 한다.  

## 페이스북, 구글, 카카오 개발자 센터 연동
소셜 미디어와 연동하여 로그인 및 기타 소셜 미디어에서 제공하는 기능을 사용하기 위해서는  
각 소셜 미디어 개발자센터에서 클라이언트 ID와 클라이언트 시크릿 키(클라이언트 보안 비밀)  
를 발급받아야 한다.  
여기서 사용하는 OAuth2 로그인 인증을 위해 페이스북, 구글, 카카오의 개발자센터 연동방법을
알아본다.  (스프링 부트 2.0.3 버전의 OAuth2 스타터 기준으로 설명한다.)
  
### 페이스북 연동
아래 주소로 접속한다.  
https://developers.facebook.com/apps

![image](https://user-images.githubusercontent.com/33191974/124435238-be7fe480-ddaf-11eb-8edb-c478d05be303.png)
![image](https://user-images.githubusercontent.com/33191974/124435313-d6576880-ddaf-11eb-9d42-ebea7463693a.png)
![image](https://user-images.githubusercontent.com/33191974/124435516-0999f780-ddb0-11eb-9f52-49966a6d2c90.png)
![image](https://user-images.githubusercontent.com/33191974/124435715-377f3c00-ddb0-11eb-9091-6d68d3d505e8.png)
![image](https://user-images.githubusercontent.com/33191974/124435841-5978be80-ddb0-11eb-896f-be2606c65ffe.png)
![image](https://user-images.githubusercontent.com/33191974/124435925-72816f80-ddb0-11eb-8d73-f8e14fac0099.png)
![image](https://user-images.githubusercontent.com/33191974/124436614-19fea200-ddb1-11eb-8cd5-45ff71b589ce.png)

localhost는 자동으로 지원하기 때문에 url에 적지 않아도 된다.

![image](https://user-images.githubusercontent.com/33191974/124436684-326ebc80-ddb1-11eb-910c-1807e34a5498.png)

## 구글 연동
아래 주소로 접속한다.  
https://console.cloud.google.com/

![image](https://user-images.githubusercontent.com/33191974/124437156-be80e400-ddb1-11eb-9df5-607760eba0f3.png)
새 프로젝트 선택  
![image](https://user-images.githubusercontent.com/33191974/124437174-c3de2e80-ddb1-11eb-930b-07bec41dac5d.png)
![image](https://user-images.githubusercontent.com/33191974/124440992-305b2c80-ddb6-11eb-932a-36a05c3292e1.png)
![image](https://user-images.githubusercontent.com/33191974/124441552-c0997180-ddb6-11eb-85a0-f2b77ffbf038.png)
처음 프로젝트를 생성하면 아무런 정보가 없기 때문에 OAuth 로그인을 위한 인증 정보를 생성해야 한다.  
![image](https://user-images.githubusercontent.com/33191974/124441832-1110cf00-ddb7-11eb-8dc6-2227c73856a0.png)
![image](https://user-images.githubusercontent.com/33191974/124442646-e8d5a000-ddb7-11eb-8d9e-818e1d707eb2.png)
![image](https://user-images.githubusercontent.com/33191974/124442670-effcae00-ddb7-11eb-9503-630322eef2ef.png)
![image](https://user-images.githubusercontent.com/33191974/124442744-0145ba80-ddb8-11eb-824d-b733d55bb4d9.png)
생성된 앱을 클릭하면 보안키 값을 얻을 수 있는 클라이언트ID와 클라이언트 보안 비밀을 사용하여 애플리케이션에서  
연동을 시작한다.  

## 카카오 연동
https://developers.kakao.com/apps
![image](https://user-images.githubusercontent.com/33191974/124443105-5c77ad00-ddb8-11eb-833c-02fd04bf9fa5.png)
![image](https://user-images.githubusercontent.com/33191974/124443235-787b4e80-ddb8-11eb-8dc6-ca21c4ca7b79.png)
![image](https://user-images.githubusercontent.com/33191974/124443330-8d57e200-ddb8-11eb-8bbb-69c6606491c0.png)
![image](https://user-images.githubusercontent.com/33191974/124443408-9cd72b00-ddb8-11eb-90ed-732ebeb9fdae.png)
![image](https://user-images.githubusercontent.com/33191974/124443468-abbddd80-ddb8-11eb-8158-f656ce1583e5.png)
![image](https://user-images.githubusercontent.com/33191974/124443535-b8dacc80-ddb8-11eb-89ed-d1a64f4b0788.png)
![image](https://user-images.githubusercontent.com/33191974/124443576-c001da80-ddb8-11eb-97eb-0b6d12f3c2a7.png)
![image](https://user-images.githubusercontent.com/33191974/124443662-d6a83180-ddb8-11eb-906d-b30cdedfe013.png)
![image](https://user-images.githubusercontent.com/33191974/124443825-f93a4a80-ddb8-11eb-941c-5eb62dd4f04f.png)
카카오는 따로 클라이언트 시크릿 키를 제공하지 않는다.  
REST API 키를 사용한다.  

앞으로의 구현 절차는 다음과 같다.
1. SNS 프로퍼티 설정 및 바인딩
2. 시큐리티 + OAuth2 설정하기
3. 어노테이션 기반으로 User 정보 불러오기
4. 인증 동작 확인하기
5. 페이지 권한 분리하기

### SNS 프로퍼티 설정 및 바인딩
소셜 미디어 연동을 위해 필요한 기본적인 프로퍼티 정보는 다음과 같다.  
- clientId : OAuth 클라이언트 사용자명, OAuth 공급자가 클라이언트를 식별하는 데 사용한다.  
- clientSecret : OAuth 클라이언트 시크릿 키값.
- accessTokenUri : 액세스 토큰을 제공하는 OAuth의 URI
- userAuthorizationUri : 사용자가 리소스에 접근하는 걸 승인하는 경우 리다이렉션할 URI.  
  소셜 미디어에 따라 필요없는 경우도 있다. 
- scope : 리로스에 대한 접근 범위를 지정하는 문자열. 쉼표로 구분하여 여러 개 지정할 수 있다.  
- userInfoUri : 사용자의 개인정보 조회를 위한 URI

### 스프링 부트 2.0 방식의 OAuth2 인증 설정
소셜 정보를 제공해줄 객체를 생성한다.  
시큐리티의 OAuth2 스펙에서는 여러 소셜 정보를 기본값으로 제공해준다.  

```java
//구글, 페이스북 등의 인증 프로퍼티 정보를 담고 있는 enum 객체
//spring-security-config-5.0.6.RELEASE.jar/org/springframework/security/config/oauth2/client/CommonOAuth2Provider.java
public enum CommonOAuth2Provider {

	GOOGLE {

		@Override
		public Builder getBuilder(String registrationId) {
			ClientRegistration.Builder builder = getBuilder(registrationId,
					ClientAuthenticationMethod.CLIENT_SECRET_BASIC, DEFAULT_REDIRECT_URL);
			builder.scope("openid", "profile", "email");
			builder.authorizationUri("https://accounts.google.com/o/oauth2/v2/auth");
			builder.tokenUri("https://www.googleapis.com/oauth2/v4/token");
			builder.jwkSetUri("https://www.googleapis.com/oauth2/v3/certs");
			builder.issuerUri("https://accounts.google.com");
			builder.userInfoUri("https://www.googleapis.com/oauth2/v3/userinfo");
			builder.userNameAttributeName(IdTokenClaimNames.SUB);
			builder.clientName("Google");
			return builder;
		}

	},

	FACEBOOK {

		@Override
		public Builder getBuilder(String registrationId) {
			ClientRegistration.Builder builder = getBuilder(registrationId,
					ClientAuthenticationMethod.CLIENT_SECRET_POST, DEFAULT_REDIRECT_URL);
			builder.scope("public_profile", "email");
			builder.authorizationUri("https://www.facebook.com/v2.8/dialog/oauth");
			builder.tokenUri("https://graph.facebook.com/v2.8/oauth/access_token");
			builder.userInfoUri("https://graph.facebook.com/me?fields=id,name,email");
			builder.userNameAttributeName("id");
			builder.clientName("Facebook");
			return builder;
		}

	},
...
}
```
위와 같이 구글, 페이스북에 대한 기본 정보는 스프링 부트 시큐리티 OAuth2 API에서 제공한다.  
즉 이전에 yaml 설정 파일에 일일이 등록했던 부분을 시큐리티 OAuth2 API가 제공한다.  
그러므로 ID와 Secret만 등록해주면 된다.  

```yaml
# 소셜별 ID, Secret 정보 입력
# /resources/application.yml
spring: 
    ...
    security:
       oauth2:
          client:
              registration:
                  google:
                      client-id:
                      client-secret:
                  facebook:
                      client-id:
                      client-secret:
```

ID와 Secret은 'security.oauth2.client.registration.{소셜명}' 경로로 프로퍼티 등록이 가능하다.  
각 소셜 미디어별로 제공되는 ID와 Secret을 등록한다.  
만약 구글, 페이스북에 대한 기본 정보를 수정하고 싶다면 프로퍼티에 새로 등록하는 방법으로 오버라이드하여  
변경할 수 있다.  
  
구글과 페이스북은 범용적인 소셜 그룹이라 시큐리티에서 제공하지만 카카오와 같이 국내에서만 사용되는 소셜은  
어떻게 처리할까?  
  
카카오는 살짝 편법을 사용하여 등록한다.  
사실 OAuth2 API에서 제공하는 방법과 동일하게 제공할 것이다.  
다음과 같이 카카오만을 위한 정보를 담은 객체를 생성한다.  

```java
//카카오 정보를 담은 CustomOAuth2Provider 객체
// /com/web/oauth2/CustomOAuth2Provider.java
public enum CustomOAuth2Provider {

   KAKAO {
      @Override
      public ClientRegistration.Builder getBuilder(String registrationId) {
          ClientRegistration.Builder builder = getBuilder(registrationId,
                ClientAuthenticationMethod.POST, DEFAULT_LOGIN_REDIRECT_URL);
          builder.scope("profile");
          builder.authorizationUri("https://kauth.kakao.com/oauth/authorize");
          builder.tokenUri("https://kauth.kakao.com/oauth/token");
          builder.userInfoUri("https://kapi.kakao.com/v1/user/me");
          builder.clientName("Kakao");
          return builder;
       }
       
       private static final String DEFAULT_LOGIN_REDIRECT_URL = 
           "{baseUrl}/login/oauth2/code/{registrationId}";
           
       protected final ClientRegistration.Builder getBuilder(String registrationId, 
                       ClientAuthenticationMethod method, String redirectUri) {
           ClientRegistration.Builder builder = ClientRegistration.
                 withRegistrationId(registrationId);
           builder.clientAuthenticationMethod(method);
           builder.authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE);
           return builder;
       }
       
       public abstract ClientRegistration.Builder builder getBuilder(String registrationId);
}
```

위 코드를 사용해 이제 카카오와 OAuth2 로그인 정보를 빌던로 생성하여 제공할 수 있게 되었다.  

카카오는 클라이언트 ID 값만 필요하기 때문에 임의로 custom.oauth2.kakao.client-id의 값을  
참조할 수 있도록 프로퍼티에 다음과 같이 추가한다.  

```yml
//카카오의 클라이언트 ID 프로퍼티값 추가
// /resources/application.yml
spring: 
    ...
    security:
       oauth2:
          client:
              registration:
                  google:
                      client-id:
                      client-secret:
                  facebook:
                      client-id:
                      client-secret:
custom:
    oauth2:
       kakao:
          client-id:
```

이제 2.0 방식으로 시큐리티 + OAuth2 설정을 해보자.  

```java
//변경된 시큐리티 + OAuth2 설정
// /com/web/config/SecurityConfig.java
@Configuration
@EnableWebSecurity

public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        
        http
           .authorizeRequests()
                  .antMatchers("/", "/oauth2/**", "/login/**", "/css/**", "/images/**", "/js/**", "/console/**") 
                  

  


































           
                
           



















모든 리소스 정보는 YAML 파일에 저장한다.  
YAML 파일에 저장하여 사용하면 정보를 매핑하기 훨씬 수월하다.  
각 소셜 미디어로부터 발급받은 clientID와 clientSecret은 개인마다 고유한 값이기 때문에  
예제에는 포함시키지 않는다.

application.properties로 되어 있다면 yml로 변경하자.
```yaml
facebook : 
   client : 
      clientId : 533166791430810
      clientSecret : ee41ece3c4143e1e6996ecd830467331
      accessTokenUri : https://graph.facbook.com/oauth/access_token
      userAuthorizationUri : https://www.facebook.com/dialog/oauth?display=popup
      tokenName : oauth_token
      authenticationScheme: query
      clientAuthenticationScheme : form
      scope : email
   resource : 
      userInfoUri : https://graph.facebook.com/me?fields=id,name,email,link
       
google : 
   client :
      clientId : 497079057139-p505slv918eqpit6bjutdqmu1lo519p9.apps.googleusercontent.com
      clientSecret : qoXm3MymFY700Ma_zkB3yt81
      accessTokenUri : https://accounts.google.com/o/oauth2/token
      userAuthorizationUri : https://accounts.google.com/o/oauth2/auth
      scope : email. profile
   resource:
      userInfoUri : https://www.googleapis.com.oauth2/v2/userinfo
      
kakao :
   client : 
      clientId : clientIdTest
      accessTokenUri : https://kauth.kakao.com/oauth/token
      userAuthorizationUri : https://kauth.kakao.com/oauth/authorize
   resource:
      userInfoUri : https://kapi.kakao.com/v1/user/me
```

예제에서는 선행 접두사를 소셜 미디어 명으로 정했고 각 소셜 미디어마다 프로퍼티 값을 client와  
resource로 나누었다.  
client 프로퍼티는 소셜 미디어에서 토큰 인증을 위해 필요한 키/값(clientId와 clientSecret)을 제공한다.  
resource 프로퍼티는 사용자의 정보를 가져올 URI를 제공한다.  
  
페이스북은 특이하게 userInfoUri의 파라미터로 원하는 정보를 요청한다.  
원래 OAuth2 라이브러리는 client.scope에 요청 정보를 담아서 가져간다.  
페이스북 API 규격은 파라미터 형식으로 되어 있어서 client.scope로 정보를 요청하면  
적용되지 않는 문제가 있으므로 fields-id,name,email,link와 같이 파라미터로 넣어서 처리했다.  
  
clientId와 clientSecret은 개인이 발급받은 값으로 채운다.  
나머지 값은 거의 바뀌지 않는 고유의 정보이니 그대로 사용하면 된다.  

매핑 방식은 앞서 배웠던 @ConfigurationProperties 어노테이션을 사용하며  
소셜미디어에 따라 프로퍼티값을 바인딩할 수 있다.  

```java
//소셜 미디어 리소스 프로퍼티를 객체로 매핑해주는 ClientResources 객체
// /com/web/oauth/ClientResources.java
public class ClientResources {

   //@NestedConfigurationProperty는 해당 필드가 단일값이 아닌 중복으로 바인딩된다고 표시하는 어노테이션이다.  
   //소셜 미디어 세 곳의 프로퍼티를 각각 바인딩하므로 @NestedConfigurationProperty 어노테이션을 붙였다. 
   @NestedConfigurationProperty
   //AuthorizationCodeResourceDetails 객체는 앞서 설정한 각 소셜의 프로퍼티 값중 'client'를  
   //기준으로 하위의 키/값을 매핑해주는 대상 객체이다. 
   private AuthorizationCodeResourceDetails client =
      new AuthorizationCodeResourceDetails();
      
   @NestedConfigurationProperty
   //ResourceServerProperties 객체는 원래 OAuth2 리소스값을 매핑하는 데 사용하지만
   //예제에서는 회원 정보를 얻는 userInfoUri 값을 받는데 사용했다. 
   private ResourceServerProperties resource = new ResourceServerProperties();
   
   public AuthorizationCodeResourceDetails getClient() {
      return client;
   }
   
   public ResourceServerProperties getResource() {
      return resource;
   }
}
```

위 클래스는 각 소셜 미디어의 client와 resource 프로퍼티값을 매핑한다.  

SecurityConfig.java에 각 소셜 미디어의 프로퍼티값을 호출하는 빈을 등록한다.  

```java
//각 소셜 미디어 리소스 정보를 빈으로 등록
// /com/web/config/SecurityConfig.java
@Configuration
public class SecurityConfig {
    @Bean
    @ConfigurationProperties("facebook")
    public ClientResources facebook() {
        return new ClientResources();
    }
    
    @Bean
    @ConfigurationProperties("google")
    public ClientResources google() {
       return new ClientResources();
    }
    
    @Bean
    @ConfiguraitonProperties("kakao")
    public ClientResources kakao() {
       return new ClientResources();
    }
}
```

소셜 미디어 리소스 정보는 시큐리티 설정에서 사용하기 때문에 빈으로 등록했고 3개의 소셜 미디어  
프로퍼티를 @ConfigurationProperties 어노테이션에 접두사를 사용하여 바인딩했다.  
만약 @ConfigurationProperties 어노테이션이 없었다면 일일이 프로퍼티값을 불러와야 했을 것이다.

### 시큐리티 + OAuth2 설정하기 스프링 부트 1.0 버전
시큐리티와 OAuth2를 설정한다.  
시큐리티 부분을 먼저 설정하고 OAuth2를 적용시킬 필터를 시큐리티 설정에 추가한다.  
시큐리티와 OAuth2 간의 연관된 설정에 유의해서 살펴보자.  
  
우선 시큐리티만 설정한다.  

```java
// 시큐리티 설정
// /com/web/config/SecurityConfig.java
@Configuraion
//@EnableWebSecurity
//@EnableWebSecurity 어노테이션은 웹에서 시큐리티 기능을 사용하겠다는 어노테이션이다.  
//스프링 부트에서는 @EnableWebSecurity를 사용하면 자동 설정이 적용된다.
@EnableWebSecurity
//WebSecurityConfigurerAdapter
//자동 설정 그대로 사용할 수도 있지만 요청, 권한 기타 설정에 대해서는 필수적으로
//최적화된 설정이 들어가야 한다.
//최적화 설정을 위해 WebSecurityConfigurerAdapter를 상속받고 Configure(HttpSecurity http)
//메서드를 오버라이드하여 원하는 형식의 시큐리티를 설정한다. 
public class SecurityConfig extends WebSecurityConfigurerAdapter {
   
   @Override
   protected void configure(HttpSecurity http) throws Exception {
       //오버라이드한 configure() 메서드의 설정 프로퍼티에 대한 설명이다.
       CharacterEncodingFilter filter = new CharacterEncodingFilter();
       http
           //authorizeRequests()
           //인증 메커니즘을 요청한 HttpServletRequest 기반으로 설정한다.
           .authorizeRequests()
                //antMatchers
                //요청 패턴을 리스트 형식으로 설정한다.
                //permitAll()
                //설정한 요청 패턴을 누구나 접근할 수 있도록 허용한다. 
                .antMatchers("/", "/login/**", "/css/**", "/images/**", "/js/**", "/console/**").permitAll()
                //anyRequest()
                //설정한 요청 이외의 요청을 표현한다.
                //authenticated()
                //해당 요청은 인증된 사용자만 할 수 있다.
                .anyRequest().authenticated()
           .and()
                //headers()
                //응답에 해당하는 header를 설정한다.
                //설정하지 않으면 디폴트 값으로 설정된다.
                //frameOptions().disabled()
                //XFrameOptionsHeaderWriter의 최적화 설정을 허용하지 않는다. 
                //사이트 내 콘텐츠들이 다른 사이트에 포함되지 않도록 하여 clickjacking 공격을 막기 위해 이 헤더를 사용합니다.
                //클릭재킹(Clickjacking, User Interface redress attack, UI redress attack, UI redressing)은 웹 사용자가 자신이 클릭하고 있다고 인지하는 것과 다른 어떤 것을 클릭하                 //게 속이는 악의적인 기법
                .headers().frameOptions().disable()
           .and()
                //예외처리기능이 작동한다.
                .exceptionHandling()
                //인증 진입 지점이다.
                //인증되지 않은 사용자가 허용되지 않은 경로로 요청할 경우 '/login'으로 이동된다.
                .authenticationEntryPoint(new LoginUrlAuthenticationEntryPoint("/login"))
           .and()
                //로그인에 성공하면 설정된 경로로 포워딩된다.
                .formLogin()
                .successForwardUrl("/board/list")
           .and()
                //로그아웃에 대한 설정을 할 수 있다.
                .logout()
                //로그아웃이 수행될 URL
                .logoutUrl("/logout")
                //로그아웃이 성공했을 때 포워딩 될 URL
                .logoutSuccessUrl("/")
                //로그아웃을 성공했을 때 삭제될 쿠키값
                .deleteCookies("JSESSIONID")
                //로그 아웃을 성공했을 때 설정된 세션의 무효화
                .invalidateHttpSession(true)
           .and()
                //첫 번째 인자보다 먼저 시작될 필터를 등록한다.
                //문자인코딩필터(filter)보다 CsrfFilter를 먼저 실행하도록 설정한다.
                .addFilterBefore(filter, CsrfFilter.class)
                
                .csrf().disable();
   }
   
   @Bean
    @ConfigurationProperties("facebook")
    public ClientResources facebook() {
        return new ClientResources();
    }
    
    @Bean
    @ConfigurationProperties("google")
    public ClientResources google() {
       return new ClientResources();
    }
    
    @Bean
    @ConfiguraitonProperties("kakao")
    public ClientResources kakao() {
       return new ClientResources();
    }
}
```
   























































