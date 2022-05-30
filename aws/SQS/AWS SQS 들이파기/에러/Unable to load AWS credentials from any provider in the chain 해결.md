spring-cloud-aws를 사용할 때 이런 에러가 날 수 있다.

```
com.amazonaws.SdkClientException: 
Unable to load AWS credentials from any provider in the chain:
[com.amazonaws.auth.EC2ContainerCredentialsProviderWrapper@5ba95e9b:
Failed to connect to service endpoint: 
, com.amazonaws.auth.profile.ProfileCredentialsProvider@3c3bb254:
profile file cannot be null]
```
이 경우, credentials를 넣어주어야 하고 몇 가지 해결책이 있다.

1. Access Key 발급받아 설정해주기   
AWS 계정은 모든 AWS 리소스에 무제한으로 접근이 가능하므로 AWS 계정의 액세스 키를 사용하는 것은  
아무래도 보안성 위험이 있다. 따라서 AWS에서는 IAM(Identity Access Management)에서 계정을 생성한 뒤  
권한을 제한할 것을 권장하고 있다.  
![image](https://user-images.githubusercontent.com/33191974/139529391-413522c7-f3e4-4e45-af33-c47bc2be1f64.png)    
2. 사용자 추가  
![image](https://user-images.githubusercontent.com/33191974/139530195-e82541f9-1b94-41c3-98c9-05c452663d9f.png)  
만약 이미 등록한 사용자가 있다면 그 사용자 정보를 들어가 액세스 키 발급 받기를 바로 누르면 된다.  
3. 사용자 세부 설정 정보  
![image](https://user-images.githubusercontent.com/33191974/139530329-ff71e2c0-2ae1-4f4f-803b-1a715c4c9299.png)      
4. 권한 설정  
![image](https://user-images.githubusercontent.com/33191974/139530444-8e6a7f52-5e99-4758-91f7-9f20dc64481b.png)
적절한 권한을 선택해주면 된다. 미리 만들어져 있는 정책에 연결해도 되고 그룹에 추가해도 된다.    
5. 태그 추가 설정  
![image](https://user-images.githubusercontent.com/33191974/139530465-da571091-f3d5-4ae1-847f-a97fb71f82c4.png)  
입력없이 패스한다.  
6. 사용자 만들고 액세스 키 다운받기  
![image](https://user-images.githubusercontent.com/33191974/139530515-79f0281a-dbb4-4aa7-afe5-2abb08b05c9a.png)  
7. 발급받은 Access Key를 application.yml 설정에 넣어주기(git local credentials에 넣어줄 수 도 있다.)   
```
cloud:
  aws:
    region:
      static: ap-northeast-2
    stack:
      auto: false
    credentials:
      access-key: AKIA5K5N3XSUKRZ2C4N7
      secret-key: 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
```      




 


